# Remote-Kit
Remote-Kit is a OpenWRT based custom firmware designed for the **GL-MT300N-V2** pocket router, specifically tailored to support a variety of tools and utilities aimed at facilitating **remote hardware bring-up, debugging, and testing activities**. This setup offers an array of remote control capabilities, including:

1. Serial terminal access to the Device-Under-Test (DUT) using the ```screen``` command on **Remote-Kit** pocket-router.
1. Ability to virtually unplug and re-plug USB accessories connected to your DUT(using Tasmota based [**XY-WFUSB**](https://templates.blakadder.com/sinilink_XY-WFUSB.html) dongle).
1. Capability to simulate Ethernet cable plug/unplug events for the DUT(using GS308EP PoE switch's api function via [**ntgrrc**](https://github.com/nitram509/ntgrrc) utility).
1. Utilization of [**PiKVM**](https://pikvm.org/) to remotely view and interact with your target hw using its HDMI-Output and USB Input(keyboard/mouse emulation).
1. Integration of a USB webcam for remote monitoring of your Target-HW setup.
1. Provision of a virtual microSD card [**SDWire**](https://shop.3mdeb.com/shop/open-source-hardware/sdwire/) for trying out different images on the DUT.
1. Control over the power ON/OFF functionality of the DUT.
1. Measurement of power consumption of the DUT for monitoring purposes.
1. FTP/TFTP server functionality for bootloaders to load images onto the RAM of the DUT(using Tasmota based XY-WFUSB dongle).

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

