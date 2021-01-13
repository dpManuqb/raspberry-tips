# raspberry-tips
# Index
1. [OS installation](1.-os-installation)
2. [SSH](2.-ssh)
3. [WLAN-Ethernet Bridge](3.-wlan-ethernet-bridge)
4. [Docker](4.-docker)
5. [Service](5.-service)

## 1. OS installation
## 2. SSH
The easiest way to communicate with the raspberrypi is through ssh by ethernet (no configuration needed). By default ssh is disabled. To enable it you can:
* Create an empty file called ssh (without extension) at the root of the sd where the OS is installed. This is a good option when you first install the OS.
* If you already have communication with the raspberrypi, you can enter ```sudo raspi-config``` command and enable ssh at **Interface options**.
It is highly recommended that you change the default password (*raspberrypi*) of the default user **pi** by typing ```passwd```. You should also add a password to the **root** user by typing ```passwd root```.
From pi user you can change to root user with ```sudo su```. Another option is to enable root ssh by changing
## 3. WLAN-Ethernet Bridge
## 4. Docker
## 5. Service
