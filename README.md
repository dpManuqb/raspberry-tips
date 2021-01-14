# RaspberryPi-Tips
# Index
1. [OS installation](#1-os-installation)
2. [Management](#2-management)
3. [Static IP](#3-static-ip)
4. [Wifi](#4-wifi)
. [WLAN-Ethernet Bridge](#-wlan-ethernet-bridge)
. [Docker](#-docker)
. [Service](#-service)

## 1. OS installation
Requirements:
* Raspberry Pi (and power source :/)
* SD (>= 8GB)
* Another PC with internet conection

Follow the steps:
1. (Optional) Download Raspberry Pi OS (previously known as Raspbian) in your local pc from [here](https://www.raspberrypi.org/software/operating-systems/).
2. Download and install Rasberry Pi Imager in your local pc from [here](https://www.raspberrypi.org/software/).
3. With an SD inserted in your local pc, open Raspberry Pi Imager and select an OS to install (or select the one you downloaded in 1) and click write.
4. Extract SD from your local pc, insert it to the raspberry pi and turn on. _Now the raspberry is working!_

## 2. Management
There are 2 main ways to manage the raspberry pi:
* **PC mode**: It consists on managing the raspberry pi as any other computer, so you will need a **Screen Monitor, USB Mouse, USB Keyboard, and a HDMI cable.** Connect all peripherals and you will be able to manage the raspberry like a regular computer.
* **SSH**: It must be enabled by creating an empty ssh file at the root of the SD or typing ```sudo raspi-config``` in the raspberry command line. You need first your raspberry to be connected someway to your computer. In both cases you need to know the raspberry IP and if you use Windows, install [Putty](https://www.putty.org/):
  - Ethernet Shared connection: connect your raspberry to your computer through an ethernet cable and share the computer connection so that you can access internet from your raspberry (look for something like _Shared connection_ in your adapter options (Windows)). Get your raspberry IP by typing ```arp -a``` (the only command you need to use in your computer apart from ```ssh```). 
  - Independant Device: connect your raspberry to your router by ethernet. Get the IP of the raspberry by getting the LAN Map from your router (http://192.168.0.1 or http://192.168.1.1 in a browser). The user/password should be downside your router device.
  
Once you know the raspberry IP go to a terminal (or Putty) and type ```ssh pi@YourRaspberrryIP```. Change pi password by typing ```passwd``` and create a root password by typing ```passwd root```. To connect ssh from root you need to change a file :```nano /etc/ssh/sshd_config``` and change _#PermitRootLogin prohibit_password_ to _PermitRootLogin yes_. Save and reboot.

## 3. Static IP
If you don't want to guess the raspberry ip (what may change thanks to DHCP) whenever you want to connect to it, you need to configure a static IP. Add these line to dhcpcd.conf below the first line that says something about interface(```nano /etc/dhcpcd.conf```):
```
interface eth0 (or wlan0 if you want to configure wifi)
static ip_address=ChooseIPFromYourNetwork (192.168.1.23 for example in my case)
static routers=YourRouterIP (192.168.1.1 in my case)
```

## 4. Wifi
If you want to connect your raspberry to Wifi you can do it easily by [PC mode](#2-management) or through ssh following these steps:
1. If you don't know the SSID: ```sudo wlist wlan0 scan```.
2. ```nano /etc/wpa_supplicant/wpa_supplicant.conf```and add:
```
country=YourCountryCode (ES in my case for Spain)
network={
    ssid="YourSSID"
    psk="YourWifiPassword"
}
```
For more details and options go [here](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

## . WLAN-Ethernet Bridge
## . Docker
## . Service
