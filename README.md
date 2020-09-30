# SET UP RASPBERRY PI
## WIFI CONNECTION
create /etc/network.interfaces
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
