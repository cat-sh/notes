# Performance Tweaks

## Troubleshoot Performance/Battery Life issues with Powertop

`powertop`

## Install TLP and Thermald

`sudo apt install tlp tlp-rdw thermald`

## Enable ZRAM

[Debian Wiki](https://wiki.debian.org/ZRam)

```
sudo apt install systemd-zram-generator
```

Create /etc/systemd/zram-generator.conf:

```
[zram0]
zram-size = ram / 2
compression-algorithm = lz4
swap-priority = 100
```

Apply and verify:

```
sudo systemctl daemon-reload
sudo systemctl start /dev/zram0
zramctl
swapon --show	# should see /dev/zram0 listed as swap
```

---


## Disable unnecessary services

Find slow services:

```
systemctl list-unit-files --type=service
systemd-analyze blame
```

```
sudo systemctl disable NetworkManager-wait-online.service
sudo systemctl disable bluetooth.service	# optional
sudo systemctl disable cups.service
sudo systemctl enable cups.socket
```

## Mask unnecessary stuff

```bash
sudo systemctl mask ModemManager.service
```

---

## Set I/O scheduler

Check:

```bash
cat /sys/block/sda/queue/scheduler
```

Set to `mq-deadline`:

```bash
echo mq-deadline | sudo tee /sys/block/sda/queue/scheduler
```

Persist:

```bash
sudo nano /etc/udev/rules.d/60-ioschedulers.rules
```

Add:

```bash
ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/scheduler}="mq-deadline"
```

---

# Kernel & scheduler tweaks

Edit:

```bash
sudo nano /etc/sysctl.d/99-performance.conf
```

Add:

```bash
kernel.sched_migration_cost_ns = 5000000
kernel.sched_autogroup_enabled = 0
vm.dirty_ratio = 10
vm.dirty_background_ratio = 5
```

Apply:

```
sudo sysctl --system
```

---

# GPU (Intel HD 4000 tweaks)

Create config:

```
sudo nano /etc/X11/xorg.conf.d/20-intel.conf
```

Add:

```
Section "Device"
  Identifier "Intel Graphics"
  Driver "intel"
  Option "TearFree" "false"
  Option "DRI" "3"
EndSection
```

