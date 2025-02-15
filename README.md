# T2UNanoDriver-Kali

This guide provides the necessary steps to make the **TP-Link T2U Nano** wireless adapter work on **Kali Linux**. Follow the instructions below to install the required driver for your device.

## 1. Identify the USB Device

First, check if the device is properly recognized by running:

```bash
lsusb
```

Ensure that your device appears in the output.

## 2. Update System and Install Required Packages

Update your system and install necessary packages using the following commands:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y dkms bc rsync git
sudo apt update
```

## 3. Download the Driver Source Code

Clone the driver files from GitHub into the **Downloads** directory:

```bash
cd ~/Downloads
git clone https://github.com/morrownr/8821au-20210708
cd 8821au-20210708
```

## 4. Compile and Install the Driver

To compile and install the driver, follow these steps:

```bash
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 8821au
```

## 5. Restart the System

Once the driver is successfully installed, restart the system:

```bash
reboot
```

### If You Encounter Issues with Rebooting

If you experience issues while rebooting, it may be due to a display manager problem. Open a new terminal and run:

```bash
sudo systemctl restart gdm  # For GNOME  
sudo systemctl restart lightdm  # For LightDM  
```

Then restart again:

```bash
reboot
```
