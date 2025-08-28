# Realtek 8851bu Driver
This is the Linux device driver for the AX900 USB Dual WIFI + BT5.3 network card, like this one:
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ff61c722-0286-436b-8e51-c91415ed9ee6" />

This includes unbranded AX900 USB WiFi 6 Bluetooth 5.3 Adapters sold on AliExpress and Amazon
Example: https://www.aliexpress.us/item/3256807263559115.html

### To compile for ARM64
If you want to compile the driver for ARM64 architecture, you need to modify the Makefile by changing the following lines:
```bash
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM64_PC = y
```
This will instruct the build process to target ARM64 architecture instead of the default x86 architecture.

### Dkms install: 
```bash
sudo dkms install .
```

### Manually install: 
```bash
make
```
```bash
sudo make install
```

You will have to reinstall for any kernel updates.
```bash
make clean
```
```bash
make
```
```bash
sudo make install
```

### Switch the USB Dongle Mode
By default, USB Wi-Fi dongles are in Driver CDROM Mode, which is incorrect for proper Wi-Fi usage. You need to switch the device to Wi-Fi Mode using usb-modeswitch.

Install usb-modeswitch:
```bash
sudo apt-get install usb-modeswitch
```

Use lsusb to find the Vendor ID and Product ID of your wireless dongle:
```bash
lsusb
```

Example output:
```
Bus 002 Device 003: ID 0bda:1a2b Realtek Semiconductor Corp. RTL8188GU 802.11n WLAN Adapter (Driver CDROM Mode)
```

In this example:
Vendor ID: 0bda
Product ID: 1a2b
Use usb-modeswitch to switch the device mode. Replace 0bda and 1a2b with your specific Vendor ID and Product ID:
```bash
sudo usb_modeswitch -K -v 0bda -p 1a2b
```

### Reload the Driver
After switching the mode, reload the driver with:
```bash
sudo modprobe 8851bu
```
### Verify Wi-Fi Device is Active
Check if the Wi-Fi interface is recognized:
```bash
iwconfig
```
If the device is still not active, check the kernel logs for any errors related to the driver:
```bash
sudo dmesg | grep 8851bu
```
