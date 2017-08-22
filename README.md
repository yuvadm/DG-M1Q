# DG-M1Q

This nice little IP camera can be had for [just over $15](https://www.banggood.com/Digoo-DG-M1Q-960P-2_8mm-Wireless-Mini-WIFI-Night-Vision-Smart-Home-Security-IP-Camera-Onvif-Monitor-p-1123595.html) and while it is quite capable it has some horrible software and security properties.

Operating it requires connecting to some shady "cloud" service and downloading some apps to your mobile device. Why, did you expect anything else?

Either way, unacceptable. Here's an alternative way of using this thing.

## Usage

![](thumb.png)

### Root Access

1. Setup an **isolated** wireless network called `Free-AP0` with no authentication
2. Device will connect to it on boot
3. Hook up to serial UART pins (see PCB pics)
4. Enjoy root access with lots of crap dumped on screen, not good enough...
4. Run `/bin/busybox telnetd`
5. Run `ip a` to get the IP address for the device
6. From a separate device on the network run `telnet $IP_ADDR`
7. Login with `root`, no password is needed
8. Enjoy **real** root access

### Persistent Root

Note that all configs in `/etc` are mounted from ramdisk, so they are not persistent.

Persistent root has to be achieved via other partitions, luckily there is a relevant file that is called by the inittab.

Edit `/npc/boot.sh` and add `/bin/busybox telnetd` to the top of the file.

### Wireless Network Configuration

Running on an unauthenticated wireless network isn't very smart. Do you want your neighbours sniffing into your apartment? Didn't think so.

After setting up your network, edit `/rom/wpa_supplicant0.conf` to match your network configuration, e.g.:

```
ctrl_interface=/etc/Wireless
network={
    ssid="YOURSSID"
    psk="YOURPSK"
}
```

**For the sake of all that is good, put this device on a separate VLAN/WLAN and firewall that shit off from the outside world.**

### Stream Access

If your network configuration is good to go, you can launch your favorite media player and watch your video + audio stream:

```
$ vlc rtsp://admin:20160404@192.168.1.99/onvif1
```

## Board Pics

 - [front.jpg](front.jpg)
 - [back.jpg](back.jpg)

## Logs

 - [First uninterrupted boot log](unbox.log)
 - [U-boot information](uboot.log)

## References

 - http://adamwesterberg.se/blog/cheap-chinese-camera-teardown
 - https://github.com/kfowlks/DG-M1Q
