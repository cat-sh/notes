# SSH key pairs

The most popular way to authenticate in SSH relies on an encrypted pair of SSH keys.

So when I generate my SSH key pair, I'm the only one that should be in possession of the *private* key and I share my *public* key with the SSH server I wish to connect to. When a message is sent from my machine to the server, the message is encoded with my private key **and** the server's public key. On the other end, the server can decrypt the message I sent with my public key in conjunction with their private key. 

This ensures that the source of the message is me and that the message can only be decoded if the recipient holds the private key to match the public key (the one used to encode the message). 

In summary:

- Private keys should only be known by its owner
- Public keys can be safely shared with others
- Sharing secrets via SSH requires both key pairs

## Generating a key pair

SSH key pairs are generated with the command `ssh-keygen`. **ALWAYS** set a passphrase when creating a key pair.

Created keys are saved in `~/.ssh` - the file with `.pub` extension contains the public key. This key needs to be sent over to the remote SSH server. This can be done with:

`ssh-copy-id -i ~/.ssh/mykey user@host`

The  `ssh-copy-id` command checks if the key already exists in the server, otherwise it edits the `authorized_keys` file. If the file doesn't exist, it creates it. If the `.ssh` folder where it resides also doesn't exist, it creates it too. This ensures key files have appropriate permissions[^1].

The public key is then added to the file `~/.ssh/authorized_keys`.

## Adding key to ssh-agent

For convenience (at the cost of some security), one might choose to add the password to the ssh agent[^2], in order not to rewrite the password each time one wants to login. One needs only write it once after login.

```
# make sure the agent is ready
eval $(ssh-agent -s)
# assuming you are using the default path
# otherwise provide the path to the private key
ssh-add
```



[^1]: [SSH Academy - SSH Copy ID for Copying SSH Keys to Servers](https://www.ssh.com/academy/ssh/copy-id)

[^2]: [University of Helsinki - Computing Tools for CS Studies](https://courses.mooc.fi/org/uh-cs/courses/computing-tools-for-cs-studies/chapter-1#dcf75d0f-3494-44e6-ae9f-fba4a1b6d7fb)