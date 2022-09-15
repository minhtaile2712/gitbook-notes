# Raspberry Pi

## Ubuntu server

Burn image and boot normally.

Wait for cloud-init to boot.

Login with username `ubuntu`, password `ubuntu`.

Retype old password, change password to anything as required.

### Show IP address

```bash
hostname -I
```

### Change password

```bash
sudo passwd ubuntu
```

### Disable cloud-init

```bash
sudo touch /etc/cloud/cloud-init.disabled
```

### Change hostname

```bash
sudo nano /etc/hostname
```

### Add user

```bash
sudo useradd -m -s /usr/bin/bash <username>
sudo passwd <username>
sudo adduser <username> sudo
```

### Auto login

```bash
sudo systemctl edit getty@tty1.service
```

```
[Service]
ExecStart=
ExecStart=-/sbin/agetty --noissue --autologin username %I $TERM
Type=idle
```
