# RaspberryPi-Tips
# Index
1. [OS installation](#1-os-installation)
2. [Management](#2-management)
3. [Static IP](#3-static-ip)
4. [Wifi](#4-wifi)
5. [WLAN-Ethernet Bridge](#5-wlan-ethernet-bridge)
6. [Home DNS](#6-home-dns)
7. [Cron](#7-cron)
8. [Docker](#8-docker)
9. [Service](#9-service)

## 1. OS installation
Requirements:
* Raspberry Pi (and power source :/)
* SD (>= 8GB)
* Another PC with internet conection

Follow the steps:
1. (Optional) Download Raspberry Pi OS (previously known as Raspbian) in your pc from [here](https://www.raspberrypi.org/software/operating-systems/).
2. Download and install Rasberry Pi Imager in your pc from [here](https://www.raspberrypi.org/software/).
3. With an SD inserted in your pc, open Raspberry Pi Imager, select an OS to install (or select the one you downloaded in 1) and click write.
4. (Optional) If you are going to use the rpi in headless mode through ssh, create an empty file called **ssh** in the root of the installation.
5. Extract SD from your pc, insert it to the rpi and turn on. _Now the raspberry is working!_

## 2. Management
There are 2 main ways to manage the rpi:
* **PC mode**: It consists on managing the rpi as any other computer, so you will need a **Screen Monitor, USB Mouse, USB Keyboard, and a HDMI cable.** Connect all peripherals and you will be able to manage the rpi like a regular computer.
* **SSH**: It must be enabled by creating an empty **ssh** file at the root of the SD or typing ```sudo raspi-config``` in the rpi command line. You need first your rpi to be connected someway to your pc. You will need to know the rpi IP and some ssh program to be installed. If you don't have any ssh program, you can install [Putty](https://www.putty.org/). Before you connect your pc with your rpi you have to decide how:
  - Ethernet Shared connection: if your pc has more than one connection interface (wifi and ethernet for example) you can connect your rpi to your pc through an ethernet cable and share the pc internet connection so that you can access internet from your rpi (look for something like _Shared connection_ in your adapter options on Windows or System Preferences/Share/Share Internet on Mac). Once connected, get your rpi IP by typing ```arp -a``` on your pc.
  - Independant Device: connect your rpi to your router by ethernet. Get the IP of the rpi by getting the LAN Map from your router (http://192.168.0.1 or http://192.168.1.1 in a browser). The user/password should be downside your router device.
  
Once you know the raspberry IP go to Putty or a terminal in your pc and type ```ssh pi@YourRaspberrryIP```. Change pi password by typing ```passwd``` and create a root password by typing ```passwd root```. To connect ssh from root you need to change a file :```nano /etc/ssh/sshd_config``` and change _#PermitRootLogin prohibit_password_ to _PermitRootLogin yes_. You should occasionally update your OS software by typing ```sudo apt-get update -y && sudo apt-get upgrade -y```.

## 3. Static IP
If you don't want to guess the rpi IP (what may change due to DHCP) whenever you want to connect to it, you need to configure a static IP. Add these line to dhcpcd.conf below the first line that says something about interface(```nano /etc/dhcpcd.conf```):
```
interface eth0 (or wlan0 if you want to configure wifi)
static ip_address=192.168.1.32 (or any other you like)
static routers=192.168.1.1 (In my case but you have to guess yours)
```

## 4. Wifi
If you want to connect your raspberry to Wifi you can do it easily by [PC mode](#2-management) or through ssh following these steps:
1. You can see in range SSID's by typing: ```sudo wlist wlan0 scan``` in your rpi.
2. ```nano /etc/wpa_supplicant/wpa_supplicant.conf```and add:
```
country=ES (YourCountryCode, ES in my case for Spain)
network={
    ssid="YourSSID"
    psk="YourWifiPassword"
}
```
For more details and options go [here](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

## 5. WLAN-Ethernet Bridge
A bridge is a logial interface that connect two or more interfaces. You can create a wlan-ethernet bridge with your rpi for example if you want to extend your wifi range. You just need to have connection to the router from your rpi, what can be achieve by several ways (100m ethernet cable, PLC or USB Wifi Dongle). Suppose you connect your rpi to your router by PLC and you want your rpi to offer wifi-internet connection:
1. Install hostapd, this allows the rpi to be connected by any other device that knows the credentials ```sudo apt-get install hostapd```
2. Configure hostapd ```sudo nano /etc/hostapd/hostapd.conf``` and add:
```
country_code=ES (Your country code)
interface=wlan0
bridge=br0
ssid=HereTheNameOfYourWifi
hw_mode=g
channel=11
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=HereThePassword
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```
3. Now enable the hostapd service so it init's on every reboot ```sudo systemctl unmask hostapd && sudo systemctl enable hostapd``` 
4. Create a bridge interface. In linux ALL is a file, so to create a bridge interface you have to create a file ```sudo nano /etc/systemd/network/bridge-br0.netdev``` and add:
```
[NetDev]
Name=br0
Kind=bridge
```
5. Add eth0 as a member of the bridge ```sudo nano /etc/systemd/network/br0-member-eth0.network``` and add:
```
[Match]
Name=eth0

[Network]
Bridge=br0
```
You don't need to add wlan0 to the bridge manually as it is defined in the hostapd.conf file.
6. Edit wlan0 and eth0 interfaces the deny them ```sudo nano /etc/dhcpcd.conf``` and add the following lines before any interface:
```
denyinterfaces wlan0 eth0
interface br0
static ip_address=192.168.1.151 (for example)
static routers=192.168.1.1 (find yours)
```

## 6. Home DNS
A Domain Name Server is a server that translates domain names (for example google.es) to IP addresses (223.123.32.41). We can make our rpi to be a DNS to give LAN network names to our home devices to access them from LAN (instead of 192.168.1.1 we can write router.myhome.com):
1. Install dnsmasq ```sudo apt-get install -y dnsmasq``` and enable the service ```sudo systemctl unmask dnsmasq && systemctl enable dnsmasq```
2. Define the names ```sudo nano /etc/hosts.conf```
3. Configure dnsmasq ```sudo nano /etc/dnsmasq.conf``` Uncomment lines listen-address=RpiIp, expand-hosts, domain=YourDomain.com.
4. Modify the upstream dns servers ```sudo nano /resolv.conf``` and add your preferred dns (8.8.8.8 for example from google).
5. Restart dnsmasq service ```sudo service dnsmasq restart```

## 7. Cron

## 8. Docker

## 9. Service
