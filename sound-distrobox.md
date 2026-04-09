# Fixing sound output on Distrobox Containers

For those who might want to run a browser inside a Distrobox container and play Youtube videos on it, you might find that there is no sound output.

A fix is documented on the Distrobox's docs, namely their Steam Deck guide.

Create `~/.distroboxrc` on your host system and add:

```
xhost +si:localuser:$USER
export PIPEWIRE_RUNTIME_DIR=/dev/null
export PATH=$PATH:$HOME/.local/bin
```

From my experience running a Ubuntu 24.04 container on Debian 13, you also need to install `pavucontrol`:

```
sudo apt install pavucontrol
```

Restart the browser and you should have audio!



### References

1. https://github.com/89luca89/distrobox/commit/16868794122db998d9e6f4d37cd1981420510df1#diff-078b6f46be55b3ffffa41c336a2b2a7f2d5029b90c4cd07b9a17e417a5e362fe

2. https://www.simon-neutert.de/posts/2025/12/05/sound-in-exported-distrobox-apps/
