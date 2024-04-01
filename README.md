# Remote-Kit
Remote-Kit is a OpenWRT based custom firmware designed for the **GL-MT300N-V2** pocket router, specifically tailored to support a variety of tools and utilities aimed at facilitating **remote hardware bring-up, debugging, and testing activities**. This setup offers an array of remote control capabilities, including:

1. Serial terminal access to the Device-Under-Test (DUT) using the ```screen``` command on **Remote-Kit** pocket-router.
1. Provision of a virtual microSD card using [**SDWire**](https://shop.3mdeb.com/shop/open-source-hardware/sdwire/) sdcard-mux for trying out different sdcard-images on the DUT without having to manually prepare bootable sdcards.
1. Ability to virtually unplug and re-plug USB accessories connected to your DUT(using Tasmota based [**XY-WFUSB**](https://templates.blakadder.com/sinilink_XY-WFUSB.html) dongle).
1. Capability to simulate Ethernet cable plug/unplug events for the DUT(using GS308EP PoE switch's api function via [**ntgrrc**](https://github.com/nitram509/ntgrrc) utility).
1. Utilization of [**PiKVM**](https://pikvm.org/) to remotely view and interact with your target hw using its HDMI-Output and USB Input(keyboard/mouse emulation).
1. Integration of a USB webcam for remote monitoring of your Target-HW setup.
1. Control over the power ON/OFF functionality of the DUT via PoE port power control.
1. Measurement of power consumption of the DUT via PoE port power measurement future of Netgear PoE switch.
1. FTP/TFTP server functionality for bootloaders to load images onto the RAM of the DUT.

This comprehensive suite of features empowers engineers to efficiently manage and troubleshoot hardware remotely, enhancing productivity and flexibility in development processes.

## Setup diagram for overwriting OEM firmware of GL-MT300N-V2 router
![Setup Diagram.](/images/setup-diagram.png "Setup Diagram.")

## Preparation
1. Download [this](https://github.com/hackboxguy/lfs-downloads/raw/main/gl-mt300nv2-remote-kit/gl-mt300nv2-remote-kit.bin) openwrt based custom firmware for **GL-MT300N-V2**
1. As shown in the picture above, connect your browser-pc to LAN side network of the pocket-router and power it ON. With OEM firmware, webUI of the pocket router is accessible at ip **192.168.8.1**(note: after overwriting OEM firmware, webUI will be available at ip **192.168.20.1**).
1. Overwrite OEM firmware of **GL-MT300N-V2** with custom firmware downloaded in 1st step(ensure that **keep-settings is disabled** so that we start with default factory settings on next boot)
1. After reboot, webUI of **GL-MT300N-V2** will be available at ip **192.168.20.1** on LAN interface side
1. Userename and password for login page are ```root```/```remote-kit-000```(later change the pw to your own using openwrt-webUI)

## Setup diagram for accessing and controlling the RASPI-4-as-DUT over the network
![Hardware Setup Diagram.](/images/hw-setup-diagram.png "Hardware Setup Diagram.")

## Bill-Of-Material for a setup of Pi-4 as DUT
**Note: The list of items along with their respective models, prices, and online links provided in this table are from the the time when this was written. However, they are presented solely as a reference and may be subject to change in the future. It is advisable to conduct your own research and verify the current information before proceeding to build this setup.**
| Sr.No | Item                         | Quantity | Unit-Price(€) | Price(€) | Remarks                         | Link                              |
| ----- | ---------------------------- | -------- | ------------- | -------- | ------------------------------- | --------------------------------- |
| 1     | GL.iNet GL-MT300N-V2 router  | 1        | 30            | 30       | None                            | [Link](https://amzn.eu/d/1pgoR1M) |
| 2     | 5v MicroUSB PS(Pocket-router)| 1        | 10            | 10       | 5v/3A Adapter                   | [Link](https://amzn.eu/d/0a8NBs1) |
| 3     | Netgear GS308EP PoE Switch   | 1        | 90            | 90       | Managed PoE Switch with Adapter | [Link](https://amzn.eu/d/cyDWo5L) |
| 4     | 5v PoE Splitter(USB-C)       | 2        | 15            | 30       | With USB-C connector            | [Link](https://amzn.eu/d/01gzida) |
| 5     | 3Port USB Hub                | 1        | 5             | 5        | Can work with any usb hub       | [Link](https://amzn.eu/d/4a37xgn) |
| 6     | Highspeed USB drive(16GB)    | 1        | 10            | 10       | Depends on the image size of DUT| [Link](https://amzn.eu/d/fuQYOrf) |
| 7     | Raspberry-Pi-4               | 2        | 50            | 100      | One for PiKVM and other as DUT  | [Link](https://amzn.eu/d/3ly0znl) |
| 8     | MicroSD Card                 | 1        | 10            | 10       | 16GB or higher for PiKVM Image  | [Link](https://amzn.eu/d/e3RXCP2) |
| 9     | Geekwork PiKVM kit           | 1        | 85            | 85       | Other PiKVM versions can be used| [Link](https://amzn.eu/d/fZf16v9) |
| 10    | USB webcam                   | 1        | 70            | 70       | Any UVC supported USB webcam    | [Link](https://amzn.eu/d/in1wPjU) |
| 11    | Tasmota Wifi Smart Socket    | 1        | 10            | 10       | Any wifi socket with tasmota-fmw| [Link](https://amzn.eu/d/a8AOq5A) |
| 12    | USB to TTL Serial Adapter    | 1        | 15            | 15       | TTL Level depends on your DUT   | [Link](https://amzn.eu/d/fqhGfQL) |
| 13    | Sinilink XY-WSUSB Dongle     | 1        | 8             | 8        | Must be flashed with tasmota    | [Link](https://www.ebay.de/itm/324256997915) |
| 14    | SDWire(SDCard-Mux)           | 1        | 90            | 90       | If your DUT boots from sdcard   | [Link](https://shop.3mdeb.com/shop/open-source-hardware/sdwire/) |

## Preparation of accessories
As shown in the picture [**above**](https://github.com/hackboxguy/remote-kit?tab=readme-ov-file#setup-diagram-for-accessing-and-controlling-the-raspi-4-as-dut-over-the-network), prepare the setup with all accessories and follow the steps below..
1. Incase if you have a dhcp server on your corporate network, feel free to access Remote-Kit on the WAN side using hostname **http://remote-kit-000**, else connect your Brower-PC to the 4th port of GS308EP PoE-Switch and it will be assigned an ip in the range of 192.168.20.x
1. IP address list of all the devices assigned by Remote-Kit's OpenWRT can be viewed at **http://192.168.20.1** (root/remote-kit-000)
1. Open webUI of GS308EP at: http://GS308EP/ (or check its ip on overview page of openwrt), Change default login password(```password```) of GS308EP(e.g **Remotekit000**)
1. Download the [**PiKVM-Image**](https://files.pikvm.org/images/v3-hdmi-rpi4-box-latest.img.xz) and using [**Balena-Etcher**](https://etcher.balena.io/#download-etcher), prepare the sdcar and boot the PiKVM by connecting its ethernet to Port-2 of PoE-Switch.
1. Connect **Nous-Smart-Socket** to the wifi of Remote-Kit at ssid: ```remote-kit-000``` and key: ```remote-kit-000```
1. Connect **XY-WFUSB-Switch** to the wifi of Remote-Kit at ssid: ```remote-kit-000``` and key: ```remote-kit-000```
1. Check if all accessories shows up with their ip addresses on the overview page of Remote-Kit-OpenWRT.

## Preparation of Port-Forwarding for accessing various accessories on WAN-Side(corporate network)
By default, Remote-Kit-Custom-Firmware of GL-MT300N-V2 includes following port forwarding rules so that accessories such as Camera/PiKVM/PoE-Switch/Nous-Smart-Socket/XY-WFUSB-SWITCH are accessible from the WAN side network of the GL-MT300N-V2 Pocket router, following are the ports accessible for different accessories
 * OpenWRT Interface of Pocket-Router: **http://remote-kit-000:80/**
 * Camera Stream is mapped at: **http://remote-kit-000:8080/**
 * Netgear PoE switch's web interface is mapped at: **http://remote-kit-000:8081/**
 * PiKVM's  webrtc interface is at: **https://remote-kit-000:4433/**
 * Nous-Smart-Socket's tasmota interface is at: **https://remote-kit-000:8082/**
 * XY-WFUSB's tasmota interface is at: **https://remote-kit-000:8083/**

To make these ports to work, ip address of the port forwarding needs to be changed to the actual ip of the accessory, this can be done by clicking the edit button under **OpenWRT-WebUI-->Network-->Firewall-->Port Forwards**.

## Remote Loading the first image to the sdcard of Pi-4-DUT
As shown in the following commands, LibreELEC image is downloaded to the usb-flash-drive(mounted as /dev/sda1 in Remote-Kit) and then Pi-4-DUT is powered down and then downloaded image is written to the micro-sdcard using SDWire's wrapper script ```sd-wire-load-image.sh```
```
[me@my-pc ~]$ ssh root@remote-kit-000  #pw: remote-kit-000
root@remote-kit-000:~# wget -O /mnt/sda1/libreelec-image.img.gz https://releases.libreelec.tv/LibreELEC-RPi4.aarch64-11.95.1.img.gz
root@remote-kit-000:~# ntgrrc login --password Remotekit000 --address GS308EP #login to poe-switch
root@remote-kit-000:~# ntgrrc poe set -p 1 --power disable --address GS308EP #power-OFF DUT before writing image to micro-sdcard
root@remote-kit-000:~# sd-wire-load-image.sh /mnt/sda1/libreelec-image.img.gz
root@remote-kit-000:~# ntgrrc login --password Remotekit000 --address GS308EP #login-again incase of session timeout
root@remote-kit-000:~# ntgrrc poe set -p 1 --power enable --address GS308EP #power-ON Pi-4-DUT
root@remote-kit-000:~# #LibreElec will now boot on Pi-4-DUT - you can see this booting from a remote location using PiKVM's WebUI.
```

## Image size limitations:
Due to limited memory of the pocket-router, avoid uncompressing large **.zip/.xz/.gz** images directly on the pocket-router's flash-drive(or SDWire's sdcard). Instead un-compress the image on your local machine, and then copy the un-compressed binary to the ntfs-formatted flash-drive of the Remote-Kit using scp.
In example above, libreelec-image.img.gz is smaller in size(<600MB uncompressed) hence it is directly un-compressed on the sd-card by ```sd-wire-load-image.sh``` script)
Directly uncompressing the [**2024-03-15-raspios-bookworm-arm64.img.xz**](https://downloads.raspberrypi.com/raspios_arm64/images/raspios_arm64-2024-03-15/2024-03-15-raspios-bookworm-arm64.img.xz) on the Remote-Kit will fail due to 2.4GB of raw image size - when in doubt, follow these 3 steps,
1. Download and uncompress the .zip or .xz or .gz image on your local PC
2. Using scp, copy the uncompressed image to remote-kit-000:/mnt/sda1/
3. Using ssh, login to remote-kit-000 and invoke ```sd-wire-load-image.sh /mnt/sda1/uncompressed-image.img``` command

## Controlling the power of DUT
```
$ ssh root@remote-kit-000  #pw: remote-kit-000
$ ntgrrc login --password Remotekit000 --address GS308EP #login to poe-switch, if required, change address GS308EP with actual ip of the poe-switch
$ ntgrrc poe set -p 1 --power enable --address GS308EP #DUT connected to Port-1 of PoE-Switch will be turned ON
$ ntgrrc poe set -p 1 --power disable --address GS308EP #DUT connected to Port-1 of PoE-Switch will be turned OFF
```

## Accessing the Serial-Terminal of the DUT
```
$ ssh root@remote-kit-000  #pw: remote-kit-000
$ screen -R serial /dev/ttyUSB0 115200 #change device-node and baud as per your setup
```

## DUT's Ethernet cable plug/unplug via remote command
```
$ ssh root@remote-kit-000  #pw: remote-kit-000
$ ntgrrc login --password Remotekit000 --address GS308EP
$ ntgrrc port set -p 1 -s 'Disable' --address GS308EP #unplug
$ ntgrrc port set -p 1 -s 'Auto' --address GS308EP #plug
```

## Measure Power consumption of DUT's via POE-Switch api
```
$ ssh root@remote-kit-000  #pw: remote-kit-000
$ ntgrrc login --password Remotekit000 --address GS308EP
$ ntgrrc poe status  --address GS308EP #print status in table format
$ ntgrrc poe status  --address GS308EP -f json #print in json format
```
