## Disable network connectivity check

```
sudo tee /etc/NetworkManager/conf.d/20-connectivity-fedora.conf <<EOF
## Bug https://discussion.fedoraproject.org/t/hotspot-popup-window/82237
[connectivity]
enabled=false
EOF
```

## Disable NetworkManager-wait-online.service

sudo systemctl disable NetworkManager-wait-online.service

## Add noatime and discard to BTRFS subvolumes
## https://lurkerlabs.com/fedora-silverblue-ultimate-post-install-guide/

sudo sed -i 's/compress=zstd:1/discard,noatime,compress=zstd:1/' /etc/fstab

## Update Firmware

sudo fwupdmgr refresh --force && \
sudo fwupdmgr get-updates && \
sudo fwupdmgr update

## Add alias in bashrc for updates & cleanup

alias update="rpm-ostree upgrade && flatpak update -y && distrobox upgrade --all"

alias cleanup="rpm-ostree cleanup -b && rpm-ostree cleanup -p && rpm-ostree cleanup -r && sudo rm -rf /var/tmp/flatpak-cache*"

## Enable automatic updates

echo "AutomaticUpdatePolicy=stage" | sudo tee --append /etc/rpm-ostreed.conf
rpm-ostree reload
systemctl enable rpm-ostreed-automatic.timer --now
systemctl start rpm-ostreed-automatic.service

(Optional)
echo "Recommends=false" | sudo tee --append /etc/rpm-ostreed.conf

## Override layered packages

rpm-ostree override remove \
    gnome-tour \
    firefox  `# prefered flatpak version instead of this` \
    firefox-langpacks \
    fedora-workstation-repositories  `# nvidia, crome, steam, pycharm` \
    gnome-classic-session
    
rpm-ostree install distrobox

Reboot now

## Pin current deployment 
## https://projectatomic.io/blog/2018/05/pinning-deployments-ostree-based-systems/

sudo ostree admin pin 0

## Disable Fedora repos and add Flathub

sudo sed -i 's/enabled=1/enabled=0/' \
  /etc/yum.repos.d/_copr:copr.fedorainfracloud.org:phracek:PyCharm.repo \
  /etc/yum.repos.d/fedora-cisco-openh264.repo \
  /etc/yum.repos.d/google-chrome.repo \
  /etc/yum.repos.d/rpmfusion-nonfree-nvidia-driver.repo \
  /etc/yum.repos.d/rpmfusion-nonfree-steam.repo
  
This gets overwritten upon next OSTree upgrade!

flatpak remote-modify --disable fedora
flatpak remote-add --system --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-modify --enable flathub
flatpak update --appstream
flatpak uninstall --all --delete-data --assumeyes

## Install Flatpaks from Flathub

flatpak install flathub \
    org.gnome.Calculator \
    org.gnome.Papers \
    org.gnome.FileRoller \
    org.gnome.Loupe \
    org.gnome.TextEditor \
    org.mozilla.firefox \
    com.github.tchx84.Flatseal \
    com.mattjakeman.ExtensionManager \
    com.ranfdev.DistroShelf \
    org.keepassxc.KeePassXC
    
flatpak update -y
    
## Change Flatpak permissions for stricter filesystem access 

flatpak override --system --nofilesystem=home
flatpak override --system --nofilesystem=host
    
## Gnome Shell Extensions

gnome-extensions disable background-logo@fedorahosted.org

```
busctl --user call org.gnome.Shell \
                   /org/gnome/Shell \
                   org.gnome.Shell.Extensions \
                   InstallRemoteExtension s \
                   "appindicatorsupport@rgcjonas.gmail.com"
```

## Declarative OSTree config

```
mkdir -p /etc/rpm-ostree/origin.d

tee /etc/rpm-ostree/origin.d/layers_n_overrides.yaml <<EOF
packages:
  - distrobox
override-remove:
  - gnome-tour
  - firefox
  - firefox-langpacks
  - fedora-workstation-repositories
  - gnome-classic-session
EOF

rpm-ostree ex rebuild

```
