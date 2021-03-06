---
author: "Ranjib Dey"
date: 2017-07-25
linktitle: Installation
title: Install & configuration
highlight: true
description: Installation and configuration of reef-pi controller
weight: 1
keywords:
- reef-pi
- reef tank
- controller
- raspberry pi
- coral
---

This guide will walk you through the process of installing and configuring reef-pi in a new Raspberry Pi with a new microSD card. The process involves three main part, first we have to prepare the micorsd card for Raspberry Pi, then start Raspberry Pi with the microSD card and configure it for reef-pi usage, and then finally install and configuration of reef-pi on the freshly prepared Raspberry Pi. This guide assumes prior familiarity with computers, no programming knowledge is required.

### Prepare Raspberry Pi image

We recommend a microSD card of Class10 and at least 8Gb capacity for reef-pi. reef-pi is tested on raspbian, but it should work on most other linux OS, on Raspberry Pi. Follow the official raspberry pi guide for details instructions:
- [Download and write raspbian image to micro SD card](https://www.raspberrypi.org/documentation/installation/installing-images/)

### Connecting to Raspberry Pi

You can either use console cable of and HDMI based display (monitor or TV) to access raspberry pi.

For console access setup follow [this adafruit tutorial](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable?view=all). HDMI based access is fairly easy, just hook up any HDMI enabled display (monitor or TV) to raspberry pi using an HDMI cable.



### Configure raspberry pi

Once raspbian image is written on microSD card, plug it in Raspberry Pi and power it. Once Raspbian OS is started, configure following things internet, date and ssh access.

- Configure wifi: Enabling wifi or ethernet based internet connection allows reef-pi to be accessible from any web browser (mobile, tablet or computers). Note, this is not a mandatory requirement for running wifi. It is just a convenience.

Next set of steps assume you are working on terminal:
- Update system: Perform a system update with the following command. Note: if internet is not enabled, this will not work, and should be skipped.
```
sudo apt-get update -y
sudo apt-get upgrade -y
```

- Change password: Raspbian images comes with a stock password for the "pi" user. Reset it to something unique and stronger

```
passwd
```
- Enable i2c,onewire,spi,uart: Different types of electronics in reef-pi requires a number of kernel modules and device drivers to be loaded in kernel. Edit `/boot/config.txt` to load these modules

```
dtparam=i2c_arm=on
dtparam=spi=on
dtparam=audio=on
enable_uart=1
dtoverlay=w1-gpio
```

- Click the Raspberry Pi icon in the upper left.
- Go to Preferences>Raspberry Pi Configuration
- On the Interfaces tab enable the necessary items (time, localization etc)

- Configure and enable ssh: ssh is used to remotely control and troubleshoot the Raspberry Pi directly.

- The same can be done from the terminal as well, using following commands:

```
sudo systemctl start ssh.service
sudo systemctl enable ssh.service
```
- Set time and date: Configure raspbian to use network time to synchronize its date & time information. Also set specific locale, to reflect the local time instead of UTC

```
sudo systemctl start ntp.service
sudo systemctl enable ntp.service
sudo dpkg-reconfigure locales
```

Finally, reboot Raspberry Pi to apply all the kernel configuration changes

```
sudo reboot
```

### Install reef-pi

- On the raspberry pi, navigate to and download the latest [release](https://github.com/reef-pi/reef-pi/releases)
- Copy the .deb out of the downloads folder to your desired location (generally in Documents folder).
- Open Terminal and type following commands to install reef-pi

```
sudo dpkg -i reef-pi-<version>.deb
```

- Check reef-pi is running

```
sudo systemctl status reef-pi.service
```


After this, you should be able to access reef-pi from the browser
