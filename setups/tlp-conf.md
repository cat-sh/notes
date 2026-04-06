# Install and Setup TLP

```
sudo apt install tlp tlp-rdw
sudo systemctl enable tlp
sudo systemctl start tlp
sudo tlp-stat -s
```

Edit configuration `/etc/tlp.conf`:
 
```
# ---- CPU ----
CPU_SCALING_GOVERNOR_ON_AC=ondemand
CPU_SCALING_GOVERNOR_ON_BAT=powersave

CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=power

CPU_BOOST_ON_AC=1
CPU_BOOST_ON_BAT=0

# ---- CPU idle ----
CPU_IDLE_GOVERNOR_ON_AC=menu
CPU_IDLE_GOVERNOR_ON_BAT=menu

# ---- Disk ----
DISK_DEVICES="sda"
DISK_APM_LEVEL_ON_AC="254"
DISK_APM_LEVEL_ON_BAT="128"

DISK_IDLE_SECS_ON_BAT=2

# ---- SATA ----
SATA_LINKPWR_ON_AC=max_performance
SATA_LINKPWR_ON_BAT=min_power

# ---- PCIe ----
PCIE_ASPM_ON_AC=default
PCIE_ASPM_ON_BAT=powersupersave

# ---- USB ----
USB_AUTOSUSPEND=1

# ---- WiFi power saving ----
WIFI_PWR_ON_AC=off
WIFI_PWR_ON_BAT=on

# ---- Audio ----
SOUND_POWER_SAVE_ON_BAT=1
SOUND_POWER_SAVE_CONTROLLER=Y

# ---- Runtime PM ----
RUNTIME_PM_ON_AC=on
RUNTIME_PM_ON_BAT=auto

# ---- Battery charge thresholds (ThinkPad only) ----
START_CHARGE_THRESH_BAT0=40
STOP_CHARGE_THRESH_BAT0=80
```

Apply changes:

`sudo tlp start`
