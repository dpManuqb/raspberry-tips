# RaspberryPi-Tips
# Index
1. [OS installation](#1-os-installation)
2. [Management](#2-management)
3. [SSH](#3-ssh)
4. [WLAN-Ethernet Bridge](#4-wlan-ethernet-bridge)
5. [Docker](#5-docker)
6. [Service](#6-service)

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
* **PC mode**: the easiest one for those who don't like command line, but the one that costs more resources. It consists on managing the raspberry pi as any other computer, so you will need a **Screen Monitor, USB Mouse, USB Keyboard, and a HDMI cable.** Connect all peripherals and you will be able to manage the raspberry like a regular computer.
* **SSH**: the best way. You just need your raspberry to be connected to your LAN (ethernet or wifi (although you need to configure wifi raspberry first xD)) and another computer also connected to the same LAN.

## 3. SSH
The easiest way to communicate with the raspberrypi is through ssh by ethernet (no configuration needed). By default ssh is disabled. To enable it you can:
* Create an empty file called ssh (without extension) at the root of the sd where the OS is installed. This is a good option when you first install the OS.
* If you already have communication with the raspberrypi, you can enter ```sudo raspi-config``` command and enable ssh at **Interface options**.
It is highly recommended that you change the default password (*raspberrypi*) of the default user **pi** by typing ```passwd```. You should also add a password to the **root** user by typing ```passwd root```.
From pi user you can change to root user with ```sudo su```. Another option is to enable root ssh by changing
## 4. WLAN-Ethernet Bridge
## 5. Docker
## 6. Service
