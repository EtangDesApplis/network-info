# SET UP RASPBERRY PI

## Setup no monitor

In boot folder create a file ssh (touch ssh)
and wpa_supplicant.conf with content:

```
country=FR
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
  ssid="WIFI-NAME"
  psk="WIFI-PASSWORD"
}
```

## WIFI CONNECTION
create /etc/network/interfaces
```
# interfaces(5) file used by ifup(8) and ifdown(8)

# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d
auto lo
iface lo inet loopback

iface eth0 inet manual

allow-hotplug wlan0
auto wlan0

iface wlan0 inet dhcp
    wpa-ssid "WIFI-NAME"
    wpa-psk "WIWI-PASSWORD"
```
### Raspberry 4 with Ubuntu
```
sudo nano /etc/netplan/50-cloud-init.yaml
```
```
# This file is generated from information provided by the datasource. Changes
# to it will not persist across an instance reboot. To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    wifis:
        wlan0:
            dhcp4: true
            optional: true
            access-points:
                "SSID_name":
                    password: "WiFi_password"
```
```
sudo netplan apply
sudo systemctl start wpa_supplicant
sudo reboot
```

## KEYBOARD
Modify the /etc/default/keyboard
```
# KEYBOARD CONFIGURATION FILE

# Consult the keyboard(5) manual page.

#XKBMODEL="pc105"
#XKBLAYOUT="us"
#XKBVARIANT=""
#XKBOPTIONS=""

XKBMODEL="pc105"
XKBLAYOUT="fr"
XKBVARIANT="azerty"
XKBOPTIONS=""

BACKSPACE="guess"
```
In case of need
```
sudo loadkeys fr
```

## k3s
Now let’s edit the boot options /boot/firmware/nobtcmd.txt and add cgroup_memory=1 cgroup_enable=memory to the end of the line so that file will look like this

ubuntu@ubuntu:~$ cat /boot/firmware/nobtcmd.txt 
net.ifnames=0 dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=LABEL=writable rootfstype=ext4 elevator=deadline rootwait fixrtc cgroup_memory=1 cgroup_enable=memory

If you skip this part. K3s will fail after bootstrapping the cluster and you will be able to find something along those lines in the logs:

Apr  4 20:22:27 rpi-master k3s[2204]: time="2020-04-04T20:22:27.635180523Z" level=error msg="Failed to find memory cgroup, you may need to add \\"cgroup_memory=1 cgroup_enable=memory\\" to your linux cmdline (/boot/cmdline.txt on a Raspberry Pi)"

however, on ubuntu it’s actually the nobtmcd.txt file.
