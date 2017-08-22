# DG-M1Q

This nice little device can be had for just over $15 (https://www.banggood.com/Digoo-DG-M1Q-960P-2_8mm-Wireless-Mini-WIFI-Night-Vision-Smart-Home-Security-IP-Camera-Onvif-Monitor-p-1123595.html) and while it is quite capable it has some horrible software.

Operating it requires connecting to some shady "cloud" service and downloading some apps to your mobile device.

Unacceptable.

Here's an alternative way of using this thing.

## Usage

### Root Access

1. Setup an isolated wireless network called `Free-AP0` with no authentication
2. Device wil connect to it on boot
3. Hook up to serial UART pins
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

Running on an unauthenticated wireless network isn't very smart.

After setting up your network, create a new file `/rom/wpa_supplicant.conf` to match your network configuration, e.g.:

```
ctrl_interface=/etc/Wireless
network={
    ssid="YOURSSID"
    psk="YOURPSK"
}
```

Then hook it into `/npc/boot.sh`, add the line:

```
cp /rom/wpa_supplicant.conf /mnt/ramdisk/wpa_supplicant0.conf
```

### Stream Access

If your network configuration is good to go, you can launch your favorite media player and watch your video + audio stream:

```
$ vlc rtsp://admin:20160404@192.168.1.99/onvif1
```

## Logs

 - [First uninterrupted boot log](unbox.log)
 - [U-boot information](uboot.log)

## References

 - http://adamwesterberg.se/blog/cheap-chinese-camera-teardown
 - https://github.com/kfowlks/DG-M1Q
