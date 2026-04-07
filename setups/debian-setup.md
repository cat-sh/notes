# Debian 13 (Trixie) Setup with BTRFS + ZRAM + Fstrim + Timeshift snapshots and Grub-Btrfs

Updated: April 2026

## Installation with net install ISO

Available [here](https://www.debian.org/distrib/netinst)

- Start the installation in "Expert" mode (not graphical)
- Select language (en_US), location (Germany) and keyboard layout (de)
- Set up user and password - do NOT set Root password

- Partitioning scheme:
  - GPT partition table
  - 1GB EFI partition
  - Main BTRFS partition
  - Skip Swap partition

- Before proceeding with installation, jump into another terminal with `Ctrl + Alt + F2`
- Check drives and partitions with `df -h`

- Unmount target filesystem

```
umount /target/boot/efi
umount /target
```

- Mount largest partition to /mnt and change directory

```
mount /dev/sda2 /mnt  # change /dev/sda2 accordingly - can be /dev/nvme0n1p2
cd /mnt
```

- Rename `@rootfs` subvolume for Timeshift compatibility

```
mv @rootfs @
```

- Create additional BTRFS subvolumes

```
btrfs su cr @snapshots
btrfs su cr @home
btrfs su cr @tmp
btrfs su cr @cache
btrfs su cr @log
```

- Mount largest partition into /target, create subfolders and mount the rest of the partitions

```
mount -o noatime,compress=zstd,subvol=@ /dev/sda2 /target
mkdir -p /target/boot/efi
mkdir -p /target/home
mkdir -p /target/.snapshots
mkdir -p /target/var/tmp
mkdir -p /target/var/cache
mkdir -p /target/var/log
mount -o noatime,compress=zstd,subvol=@home /dev/sda2 /target/home
mount -o noatime,compress=zstd,subvol=@snapshots /dev/sda2 /target/.snapshots
mount -o noatime,compress=zstd,subvol=@tmp /dev/sda2 /target/var/tmp
mount -o noatime,compress=zstd,subvol=@cache /dev/sda2 /target/var/cache
mount -o noatime,compress=zstd,subvol=@log /dev/sda2 /target/var/log
mount /dev/sda1 /target/boot/efi  # change /dev/sda1 accordingly - can be /dev/nvme0n1p1
```

- Use nano to edit `/target/etc/fstab`

```
UUID=bcecfcc8...   /            btrfs  noatime,compress=zstd,subvol=@  /dev/sda2  0    1
UUID=bcecfcc8...   /home        btrfs  noatime,compress=zstd,subvol=@home  /dev/sda2  0    2
UUID=bcecfcc8...   /.snapshots  btrfs  noatime,compress=zstd,subvol=@snapshots  /dev/sda2  0   2
UUID=bcecfcc8...   /var/tmp     btrfs  noatime,compress=zstd,subvol=@log  /dev/sda2  0    2
UUID=bcecfcc8...   /var/log     btrfs  noatime,compress=zstd,subvol=@log  /dev/sda2  0    2
UUID=bcecfcc8...   /var/cache   btrfs  noatime,compress=zstd,subvol=@var  /dev/sda2  0    2
```

- Continue with the installation by pressing `Ctrl + Alt + F1`
- Kernel to install: linux-image-amd64
- Drivers: generic
- Scan extra installation media: no
- Network mirror: yes, http, deb.debian.org
- Use non-free firmware and software: yes
- Enable source repos: no
- Enable backported software
- No automatic updates
- Software selection: "Standard system utilities"
- Force GRUB installation to EFI removable: no
- Update NVRAM: yes
- Run os-prober: no
- Choose "Finish the installation"
- System clock set to UTC: no

You can reboot now into your newly installed Debian system.

## First boot

### ZRAM and fstrim.timer

```
sudo apt update && sudo apt upgrade
sudo apt install zram-tools
```

Edit `/etc/default/zramswap`:

```
ALGO=lz4
PERCENT=25
#SIZE=512
PRIORITY=100
```

Edit `/etc/sysctl.d/99-vm-zram-parameters.conf`:

```
# optimizes swap on zram as described on:
# https://wiki.archlinux.org/title/Zram#Optimizing_swap_on_zram

vm.swappiness = 180
vm.watermark_boost_factor = 0
vm.watermark_scale_factor = 125
vm.page-cluster = 0
```

Enable zram and fstrim services:

```
sudo systemctl enable zramswap.service
sudo systemctl start zramswap.service
sudo systemctl enable fstrim.timer
sudo systemctl start fstrim.timer
```

### Desktop Enviroment

My DE of choice is Gnome, so I install `gnome-core` for a minimal Gnome installation.

Remove extra applications:

```
sudo apt remove --purge gnome-calendar gnome-contacts gnome-tour gnome-maps gnome-snapshot gnome-weather totem simple-scan
sudo apt autoremove
```

Disable NetworkManager-wait-online:

`sudo systemctl disable NetworkManager-wait-online.service`

Reboot now.

## Timeshift

`sudo apt install timeshift`

Create a first snapshot:

`sudo timeshift --create --comments "initial setup"`

Using the GUI, you can also go through the initial setup. I setup 5 on boot snapshots.

Reboot now.

## Grub-Btrfs

```
sudo apt install git build-essential inotify-tools gawk    # dependencies
git clone https://github.com/Antynea/grub-btrfs
cd grub-btrfs
sudo make install
sudo grub-mkconfig
sudo systemctl start grub-btrfsd
sudo systemctl enable grub-btrfsd
```

Configure btrfsd to work with Timeshift:

```
sudo systemctl edit --full grub-btrfsd

# change ExecStart line to point to Timeshift
ExecStart=/usr/bin/grub-btrfsd --syslog --timeshift-auto
```

Restart the service:

```
sudo systemctl restart grub-btrfsd
sudo grub-mkconfig
```

Reboot now.

## Set NetworkManager to control network interfaces

Comment out the lines in `etc/network/interfaces`.

Edit `/etc/NetworkManager/NetworkManager.conf`:

```
[ifupdown]
managed=true
```

Restart NetworkManager service with `sudo service NetworkManager restart`.

## Optional: Nvidia Proprietary Drivers

```
sudo apt install linux-headers-amd64
sudo apt install nvidia-kernel-dkms nvidia-driver
```

Wayland configuration:

```
# /etc/modprobe.d/nvidia-wayland.conf

options nvidia NVreg_PreserveVideoMemoryAllocations=1
options nvidia-drm modeset=1
options nvidia-drm fbdev=1
```

Reboot.

## Optional: TLP from Trixie Backports for laptop power management

`sudo apt -t trixie-backports install tlp tlp-pd tlp-rdw`

## Other Software

`sudo apt install curl tree distrobox flatpak keepassxc-full gnome-shell-extension-manager gnome-tweak-tool`

---

### References

https://www.youtube.com/watch?v=_zC4S7TA1GI
https://medium.com/@inatagan/installing-debian-with-btrfs-snapper-backups-and-grub-btrfs-27212644175f
https://github.com/Aerodyne-Jazz/Debian-13-BTRFS-Install-Guide/blob/main/BTRFS_Timeshift_Grub-BTRFS.md
https://mutschler.dev/linux/debian-btrfs-trixie/
https://wiki.debian.org/%20SSDOptimization%20
https://wiki.archlinux.org/title/Zram
https://github.com/Antynea/grub-btrfs
