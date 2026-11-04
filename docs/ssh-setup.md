# SSH Setup

## Server

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
ip addr show # note the Ethernet IP, e.g. 192.168.1.x
```

## Client (Mac)

Create a key and copy it to the server:

```bash
ssh-keygen -t ed25519
ssh-copy-id username@192.168.1.x
ssh username@192.168.1.x # should log in without a password
```

## Disable password login

On the server, set this in `/etc/ssh/sshd_config`:

```text
PasswordAuthentication no
```

Then restart SSH:

```bash
sudo systemctl restart ssh
```

## Shortcut

Add to `~/.ssh/config` on the Mac:

```text
Host homelab
    HostName 192.168.1.x
    User yourusername
```

Connect with `ssh homelab`. Once Tailscale is running, use the server's Tailscale IP to connect from anywhere.
