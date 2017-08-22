# DG-M1Q

This nice little device can be had for just over $15 (https://www.banggood.com/Digoo-DG-M1Q-960P-2_8mm-Wireless-Mini-WIFI-Night-Vision-Smart-Home-Security-IP-Camera-Onvif-Monitor-p-1123595.html) and while it is quite capable it has some horrible software.

Operating it requires connecting to some shady "cloud" service and downloading some apps to your mobile device.

Unacceptable.

Here's an alternative way of using this thing.

## Usage

1. Setup an isolated wireless network called `Free-AP0` with no authentication
2. Device wil connect to it on boot
3. Hook up to serial UART pins
4. Despite lots of shit being dumped to console, run `/bin/busybox telnetd`
5. Run `ip a` to get the IP address for the device
6. From a separate device on the network run `telnet $IP_ADDR`
7. Login with `root`, no password is needed

## Logs

 - [First uninterrupted boot log](unbox.log)
 - [U-boot information](uboot.log)


