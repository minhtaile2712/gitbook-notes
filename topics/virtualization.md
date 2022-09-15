# Virtualization



### Check

#### Check if the CPU supports hardware virtualization

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

`>1`: yes. `0`: no.

#### Optional: Check if the CPU supports hardware virtualization using `cpu-checker`

```bash
sudo apt install cpu-checker
kvm-ok
```

#### Optional: Check if the CPU is 64-bit

```bash
egrep -c ' lm ' /proc/cpuinfo
```

`>1`: yes. `0`: no.

#### Optional: Check if running kernel is 64-bit

```bash
uname -m
```

`x86_64` or `amd64`: yes.

### Installation

#### Install

```bash
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients
```

#### Add Users to Groups

```bash
sudo adduser `id -un` libvirt
```

#### Verify

```bash
virsh list --all
```

### Optional: Graphical User Interface

#### Install

```bash
sudo apt install virt-manager
```

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
