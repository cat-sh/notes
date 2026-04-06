# ssh - secure shell protocol

Encrypts the connection between a client and a server.

All user authentication, commands, output and file transfers are encrypted.

## How does it work

1. SSH client initiates by contacting server
2. SSH server sends server public key
3. SSH client "negotiates parameters"
4. SSH client logs user in to server OS

## Security Concerns

- Unrestricted outbound SSH via firewall TCP rule allowing port 22
- Inbound SSH is usually restricted to one (or very few) servers
- [Tunneling](https://www.ssh.com/academy/ssh/tunneling) SSH connection is supported but not recommended
- All connections on port 22 can be forwarded to a particular IP address on the internal network or DMZ
- Alternatively, SSH access can be restricted to only be allowed after VPN login.


### References

[SSH Academy](https://www.ssh.com/academy/ssh)
[SSH Protocol page](https://www.ssh.com/ssh/protocol/)
[SFTP page](https://www.ssh.com/academy/ssh/sftp-ssh-file-transfer-protocol)


Interesting read: [How did SSH get port 22?](https://www.ssh.com/academy/ssh/port)
