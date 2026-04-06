# Configuring sshd_config

On servers running `sshd`, the `sshd_config` file specifies the locations of host keys (mandatory) and the location of `authorized_keys` files.

This is (my) example of a simple, balanced configuration that keeps some things convenient but avoids big risks.

```
# /etc/ssh/sshd_config
# Updated: 25/03/2026

# --- Basic access ---
Port 2222		# reduces noise from scanners
Protocol 2		# mostly redundant, forces usage of SSH v2

# --- Restrict access to specific IP range ---
# AllowUsers username@192.168.1.*

# --- Authentication ---
# Root login is allowed only via SSH key, forces use of SSH keys
PermitRootLogin prohibit-password
PasswordAuthentication no
PubkeyAuthentication yes

# --- SSH agent & forwarding ---
AllowAgentForwarding no
X11Forwarding no

# --- Allow X11 only for specific users and/or group---
# Match User username
# Match Group groupname
#     X11Forwarding yes
#     AllowAgentForwarding yes

# --- Connection hardening ---
MaxAuthTries 4
LoginGraceTime 30		# limits connection flooding
ClientAliveInterval 300
ClientAliveCountMax 2
```

Configuring Fail2Ban is also an important consideration on publicly exposed SSH servers.