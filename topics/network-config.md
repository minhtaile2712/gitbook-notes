# Network Config

## synopsis

### Network address translation (NAT)

Give the guest operating system access to external Ethernet network connection using the host's IP address.

The NAT service works in a similar way to a home router, grouping the systems using it into a network and preventing systems outside of this network from directly accessing systems inside it, but letting systems inside communicate with each other and with systems outside using TCP and UDP over IPv4 and IPv6.

Function: add DHCP server, support port-forwarding.

NAT is the default. NAT is useful when the number of IP addresses is limited or the host system is connected to the network through a non-Ethernet adapter. NAT causes some performance loss.

### Bridged networking

Give the guest operating system direct access to an externet Ethernet network. The guest must have its own IP address on the external network. Can set up routing or bridging between guest and rest of the network

## Show configs

{% tabs %}
{% tab title="Linux" %}
```
ip a
ifconfig
ifconfig -a
arp -na
sudo nmap -sn 172.29.5.0/24
nslookup raspberrypi
hostname -I
sudo arp-scan --localnet
cat /etc/resolv.conf
```
{% endtab %}

{% tab title="Second Tab" %}
```
ipconfig /all
ipconfig
ipconfig /?
nslookup raspberrypi
ping raspberrypi
arp -a
```
{% endtab %}
{% endtabs %}

## Set up proxies on Windows

```
netsh int ipv4 reset
netsh interface portproxy show all
netsh interface portproxy add v4tov4 listenport=4000 listenaddress=0.0.0.0 connectport=4000 connectaddress=192.168.101.100
```

```
netsh interface portproxy delete v4tov4 listenport=5000 listenaddress=0.0.0.0
```

## Related technology

vnc, lighttpd, u-boot

## OFFICIAL CONFIGURE

### Config Netplan

#### Open file to configure Netplan

```
sudo nano /etc/netplan/50-cloud-init.yaml
```

#### Add this content

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      optional: true
    wlan0:
      dhcp4: no
      optional: true
  bridges:
    br0:
      interfaces: [eth0, wlan0]
      addresses: [192.168.0.1/24]
      routes:
        - to: default
          via: 0.0.0.0
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
      optional: true
```

#### Apply changes

```
sudo netplan apply
```

### Config hostapd

#### Install hostapd and add conf file

```
sudo apt install hostapd
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
sudo nano /etc/hostapd/hostapd.conf
```

#### Add this content

```
interface=wlan0
bridge=br0

hw_mode=g
channel=7

ssid=tmtcontrol

macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0

wpa=2
wpa_passphrase=tmtcontrol
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

#### Test if AP is visible on phone

```
sudo systemctl start hostapd
```

### Config dnsmasq

#### Install dnsmasq and create conf file

```
sudo apt install dnsmasq
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.bak
sudo nano /etc/dnsmasq.conf
```

#### Add this content

```
port=0
interface=br0
dhcp-range=192.168.0.1,192.168.0.100,12h
dhcp-host=fe:86:7a:47:30:73,192.168.0.2
dhcp-host=f2:11:e6:b9:95:e0,192.168.0.3
dhcp-host=8a:d8:2a:5f:f1:44,192.168.0.4
```

#### Check error and restart dnsmasq

```
dnsmasq --test
sudo systemctl restart dnsmasq
```

## Older configs

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses: [192.168.0.1/24]
      routes:
        - to: default
          via: 0.0.0.0
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
      optional: true
```

```
port=0
interface=eth0
dhcp-range=192.168.0.50,192.168.0.150,12h
dhcp-host=fe:86:7a:47:30:73,192.168.0.2
dhcp-host=f2:11:e6:b9:95:e0,192.168.0.3
dhcp-host=8a:d8:2a:5f:f1:44,192.168.0.4
#dhcp-authoritative
```

## MAC Address of Cameras

front fe-86-7a-47-30-73 left f2-11-e6-b9-95-e0 right 8a-d8-2a-5f-f1-44
