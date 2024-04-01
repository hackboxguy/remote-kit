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

## Setup diagram for accessing and controlling the Device-Under-Test over the network
![Hardware Setup Diagram.](/images/hw-setup-diagram.png "Hardware Setup Diagram.")
