# Linux

## View LISTEN ports

```bash
sudo lsof -i -P -n | grep LISTEN
```

## Download SSL packages

For `arm64`:

```bash
wget \
http://ports.ubuntu.com/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_arm64.deb
```

For `amd64`

```bash
wget \
http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
```

## Increase Swap

```
sudo swapoff -a
sudo dd if=/dev/zero of=/swapfile bs=1G count=8
sudo chmod 0600 /swapfile
sudo mkswap /swapfile  # Set up a Linux swap area
sudo swapon /swapfile  # Turn the swap on
```

## Install vi keyboard

```
sudo apt install ibus-unikey
ibus restart
```

## Install build tool

```
sudo apt install build-essential
```

## Grant access to serial port

```
sudo usermod -aG dialout $USER
newgrp dialout
groups
```

## Disable boot timeout

```
sudo gedit /etc/default/grub
# GRUB_TIMEOUT to -1
sudo update-grub
```

## Terminal Shortcuts

**`Ctrl+Shift+Up`** Scroll up

**`Ctrl+Shift+Down`** Scroll down

**`Shift+PageUp`** Scroll page up

**`Shift+PageDown`** Scroll page down

**`Shift+Home`** Scroll to top

**`Shift+End`** Scroll to bottom

## Tips

```
lsblk - list block devices
dmesg - print or control the kernel ring buffer
head - output the first part of files
tail - output the last part of files
parted - a partition manipulation program


### Reset the store
killall snap-store

### Customize Dock
sudo apt install dconf-editor
org > gnome > shell > extensions > dash-to-dock
background-opacity 0.25
custom-background-color true
dock-fixed false
dock-posision BOTTOM
extend-height false
show-trash true

### check file integrity
sha256sum -c SHA256SUMS 2>&1 | grep OK



### report file system disk space usage
df -h

### find a file system
findmnt

### unmount file systems
umount

### convert and copy a file
dd

### print the system information
uname

### show the status of modules in the Linux Kernel
lsmod

### Show if 'package' installed
apt list 'package'

### Show which packages depends on a 'package'
apt-cache depends 'package'

### Rename extension of multiple files
for f in *.jpg; do mv "$f" "${f%.jpg}"; done
for f in *; do mv "$f" "$f.jpg"; done

### After copying
find ~/workspace -type d -exec chmod 775 {} \;
find ~/workspace -type f -exec chmod 664 {} \;

```

###

## Using cat

```
# Usage

## Create a new file:
cat > yourfile.txt
Some text
Ctrl+D

## View a file:
cat yourfile.txt

## Concatenate two files into another file:
cat sasmple1.txt sample2.txt > sample3.txt

## View with line number:
cat -n file1.txt

## Copy and replace content from one to one:
cat file2.txt > file1.txt

## Copy content from one and add to end of another:
cat file3.txt >> file1.txt

## Add content to existing file:
cat >> file1.txt
Some text
Ctrl+D
```

## Services

```
https://www.digitalocean.com/community/tutorials/systemd-essentials-working-with-services-units-and-the-journal

sudo systemctl start mongod.service
sudo systemctl stop mongod.service
sudo systemctl restart mongod.service
sudo systemctl reload mongod.service

sudo systemctl enable mongod.service
sudo systemctl disable mongod.service

## Viewing Basic Log Information
### see all log entries, starting at the oldest entry
journalctl


### see an overview of the current state of a unit
systemctl status mongod.service

### see all of the journal entries for the unit in question
journalctl -u mongod.service

## Inspecting Units and Unit Files
### see the full contents of a unit file
systemctl cat mongod.service

### see the dependency tree of a unit
systemctl list-dependencies mongod.service
systemctl list-dependencies --all nginx.service

### see the low-level details of the unit’s settings on the system
systemctl show nginx.service

## Modifying Unit Files
### add a unit file snippet, which can be used to append
### or override settings in the default unit file
sudo systemctl edit nginx.service
### modify the entire content of the unit file
sudo systemctl edit --full nginx.service
### after modifying a unit file, you should reload the systemd process
sudo systemctl daemon-reload

```

## Virtualization

```
qemu-kvm qemu-kvm virt-manager 

NO install bridge-utils

sudo usermod -a -G libvirt $USER
newgrp libvirt


egrep '^flags.*(vmx|svm)' /proc/cpuinfo


### Check if I have Intel VT or AMD-V?
egrep '^flags.*(vmx|svm)' /proc/cpuinfo

### The modules are correctly loaded
lsmod|grep kvm

###
kvm-ok
```

## User



Add user

```
sudo useradd -m -s /usr/bin/bash <username>
```

\-m is for creating home folder

\-s is for default shell

need create password before can log in

```
sudo passwd <username>
```

add user to sudoers

```
sudo adduser <username> sudo
```

## Login



Automatic login

```
sudo systemctl edit getty@tty1.service
```

Type this and save

```
[Service]
ExecStart=
ExecStart=-/sbin/agetty --noissue --autologin username %I $TERM
Type=idle
```

## See welcome message

```
sudo run-parts /etc/update-motd.d
```

## Disable autoupdate

```bash
 sudo apt remove unattended-upgrades
 sudo apt remove update-manager
```

```
sudo snap install mosquitto
sudo snap install redis
snap services

sudo apt install mosquitto-clients
sudo apt install redis-tools
```
