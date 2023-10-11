---
layout: post
title: "The post-2010 solution for connecting to WiFi from command line on Linux via nmcli"
date: 2023-10-1
author: Forrest Sheng Bao/鮑盛
---

First, forget about solutions using `iwconfig` (which does not support WPA) or `wpa_supplicant`. They are outdated.

Modern (post-2010) Linux distributions use `NetworkManager` to manage network connections. `nmcli` is the command line interface to `NetworkManager`. 

This tutorial assumes that your WiFi adapter has been properly driven by the Linux kernel.

Steps: 

0. Make sure NetworkManager is running (It should be on if you boot into a desktop environment. But it may not be running by default if you are in the non-GUI mode of Linux.) 
   ```bash
   systemctl start NetworkManager
   ``` 
1. Scan for available WiFi networks
   ```bash
   $ nmcli device wifi list
   ```
   You can pipeline it with the `less` command if you have a long list of WiFi access points (APs). 
2. Connect to a WiFi network **for the first time** (If this is not the first time, jump to step 3). 
   ```bash
   sudo nmcli device wifi connect NETWORK_SSID 
   ```
   where `NETWORK_SSID` is the ID of the network. 

   If the network is password-protected, you can pass the password verbatim like this: 
   ```bash
   sudo nmcli device wifi connect NETWORK_SSID password "NETWORK-PASSWORD"
   ```
   Or, if you prefer not to leave your password in your shell history, you can use the `--ask` flag to be prompt to enter a password. 
   ```bash
   sudo nmcli device wifi connect NETWORK_SSID --ask
   ```

   The information of each network (called "connection" in NetworkManager's terminology), including the password, is stored into a configuration file under `/etc/NetworkManager/system-connections/`. That's why you only needed to do this step once, when you initially connect to a WiFi. Otherwise, `nmcli` will create duplicates for you. 
3. To connect to a network that was **previously connected to**, simply 
   ```bash
   nmcli con up NETWORK_SSID
   ```
4. Check your connection. You can simply check your connections by 
   ```bash
   ping google.com
   ```
5. Disconnect from a WiFi, if you need to: 
   ```bash
   nmcli con down NETWORK_SSID
   ```
6. Auto-start NetworkManager at system book (only for non-GUI boot)
   NetworkManager is a GUI program and is usually started when the desktop environment starts. What if you Linux box is configured to boot into non-GUI mode as in a server? 
   Simply enable NetworkManager in `systemctl`.
   ```bash
   sudo systemctl enable NetworkManager
   ```

# Additional fun
You can use `ifconfig` or `iwconfig` (for wireless only) to identify all your network interfaces. Then use `iwlist` to scan to know more information of the networks available around your. 

```bash
$ iwconfig
```
The output is 
```
lo        no wireless extensions.

wlp0s20f3  IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=22 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
```

As you can see, `wlp0s20f3` is the wireless adapter on my computer. Then I just need to run 
```bash
sudo iwlist wlp0s20f3 scan
```
to find out detailed information about WiFi networks available around me. 

# References
* https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/getting_started_with_networkmanager#sec-Overview_of_NetworkManager
* https://www.makeuseof.com/connect-to-wifi-with-nmcli/
* 