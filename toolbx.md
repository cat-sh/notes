# My first experience with Atomic Desktops and Fedora Silverblue

I have been using Linux on various machines, alongside Windows and MacOS, for over 15 years now. That's kinda crazy to think about - I'm not as old as that statement makes me sound!

And while I was aware of the existence of projects like Fedora Silverblue and how great atomic, immutable desktops can be, especially for developers, I never really felt the pull to install one and run it as my main desktop OS.

I have used an immutable desktop OS before on the Steam Deck, I'm familiar with containerization technologies like Docker and I've even had a multiple-month-long affair with NixOS. Heck, on most days I think Flatpaks are *okay*.

## The Problem

Learning a new OS comes with its own quirks. For Atomic desktops like Silverblue, I believe the biggest one has to do with package management.

Up until now, I relied on installing all the software I needed in Silverblue through Flatpak. But today I wanted to remake my vimrc and realized that [Silverblue ships with `vim-minimal`](https://gitlab.com/fedora/ostree/sig/-/work_items/16), which doesn't include documentation and `vimtutor`.

So here's the dilemma, either install `vim-enhanced` via `rpm-ostree` or try out this not-so new piece of technology called **Toolbx**.

## What is Toolbx and how does it work

Toolbx is built on top of containerization tools like Podman, and it's mostly aimed at developers. According to Fedora's own documentation[^2], "[Toolbx] keeps the host OS clean and stable, and helps to avoid the clutter that can happen after installing lots of development tools and packages."

I like the sound of that.

Each toolbox container features:

- Your existing username and permissions.

- Access to your home directory and several other locations.

- Access to both system and session D-Bus, system journal and Kerberos.

- Common command lines tools, including a package manager (e.g., DNF on Fedora).

## Running my first container

First, I run `toolbox create`.

```
cfs@thinkpad:~$ toolbox create
Image required to create Toolbx container.
Download registry.fedoraproject.org/fedora-toolbox:43 (357.1MB)? [y/N]: 
```

As I didn't specific any other distribution, it prompted me to download an image for Fedora 43. By default, Toolbx downloads an "OCI image if one isn't available"[^2] and it should match the host system. If there is no matching image, it downloads a Fedora image.

After the download is done, I get the following message:

```
Created container: fedora-toolbox-43
Enter with: toolbox enter
```

Before proceeding, I run `toolbox list` - works as expected, lists local toolbx images and containers.

```
IMAGE ID      IMAGE NAME                                    CREATED
4e60401c9386  registry.fedoraproject.org/fedora-toolbox:43  13 hours ago
CONTAINER ID  CONTAINER NAME     CREATED        STATUS   IMAGE NAME
5331bbbbde92  fedora-toolbox-43  2 minutes ago  created  registry.fedoraproject.org/fedora-toolbox:43
```

`toolbox enter` prints a welcome message and changes my bash prompt, to signal I am now inside a container. Nice.

Running `ls` shows all folders and files inside my home folder. I'm impressed. Also the memory and CPU usage do not seem to go up by a significant amount, which is vital to me on this Thinkpad T430s with 8GB RAM.

I can now run `dnf update` and `dnf search vim-enhanced` and... I get a match!

```
Updating and loading repositories:
 Fedora 43 openh264 (From Cisco) - x86_64      100% |  12.2 KiB/s |   5.8 KiB |  00m00s
 Fedora 43 - x86_64 - Updates                  100% |   3.2 MiB/s |  12.4 MiB |  00m04s
 Fedora 43 - x86_64                            100% |   4.6 MiB/s |  35.4 MiB |  00m08s
Repositories loaded.
Matched fields: name (exact)
 vim-enhanced.x86_64	A version of the VIM editor which includes recent enhancements
```

Then it's just a matter of running `sudo dnf install vim-enhanced` and I'm ready to run good ol' Vim!

### Ptyxis - the (new) default GNOME terminal

Toolbx seems to integrate well with Ptyxis. It is being promoted as a container centric terminal emulator after all. 

I now have the option to open a new tab directly on my container, a la Windows Terminal and WSL.

![Ptyxis screenshot](/var/home/cfs/Notes/images/Screenshot From 2026-03-24 21-21-17.png)

Not all distros are shipping this by default yet, but I believe this to be only a matter of time. I much prefer Ptyxis to gnome-terminal, and even though I have dabbled with Alacritty, I find no reason to use it as my main terminal app.



[^1]: [Gitlab - Replace vim-minimal with vim-enhanced on Atomic Desktop](https://gitlab.com/fedora/ostree/sig/-/work_items/16) 

[^2]: [Fedora Docs](https://docs.fedoraproject.org/en-US/atomic-desktops/toolbox/)


