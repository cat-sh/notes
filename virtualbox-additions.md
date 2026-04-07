# Setup VirtualBox Additions on a (Debian) Linux Guest

## Install required packages first

```
# apt -y install gcc make bzip2 linux-headers-$(uname -r)

# mount /dev/cdrom /mnt

# cd /mnt

# ./VBoxLinuxAdditions.run

Verifying archive integrity...  100%   MD5 checksums are OK. All good.
Uncompressing VirtualBox 7.2.2 Guest Additions for Linux  100%
VirtualBox Guest Additions installer
VirtualBox Guest Additions: Starting.
VirtualBox Guest Additions: Setting up modules
VirtualBox Guest Additions: Building the VirtualBox Guest Additions kernel
modules.  This may take a while.
.....
.....

# reboot 
```