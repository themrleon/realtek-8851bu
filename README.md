# Realtek 8851bu Driver
This is the Linux device driver for the AX900 USB WIFI + BT network card, like this one:  
<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/ff61c722-0286-436b-8e51-c91415ed9ee6" />  

> [!WARNING] 
> Some AX900 being sold on websites like Aliexpress may have misleading/wrong information about the chipset model  

<br/>

# Will this driver work for my device ?
Only **YOU** can find out! start by identifying the product id from your device. Unplug it from the USB port and run `lsusb`, ex:
```
$ lsusb 
Bus 001 Device 005: ID 258a:002a  
Bus 001 Device 004: ID 214b:7250  
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. SMC9514 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

Now plug it back and re-run `lsusb`, look at what changed, the new entry is your device, [**make sure it is NOT in disk mode before proceeding**](#usb-device-mode). Now copy the id after the colon, in this case `b851`:
```
$ lsusb 
Bus 001 Device 007: ID 0bda:b851 Realtek Semiconductor Corp.
Bus 001 Device 005: ID 258a:002a  
Bus 001 Device 004: ID 214b:7250  
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. SMC9514 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

Do a search on this repo files for that id, adding a `0x` prefix:
```
$ grep -r -s -i 0xb851 *
os_dep/linux/usb_intf.c:	{USB_DEVICE_AND_INTERFACE_INFO(USB_VENDER_ID_REALTEK, 0xB851, 0xff, 0xff, 0xff), .driver_info = RTL8851B},
phl/hal_g6/mac/mac_ax.c:#define PID_HW_DEF_8851BS	0xB851
```
If you see results like above, it means this driver will likely work for your device!

> [!TIP] 
> If your device id wasn't found in this repo files, you can try a Github global search using the product id, eventually you will find a repo that matches it and is also a driver  

<br/>

# USB device Mode
Some of these USB wifi dongles have 2 modes, on Windows you will see they behave like a usb flash disk, showing up as a storage device with some files like the Windows driver. If that is the case for you, then on Linux you have to manually switch its mode from storage to network:
```
$ sudo apt-get install usb-modeswitch
$ lsusb 
Bus 001 Device 007: ID 0bda:1a2b Realtek Semiconductor Corp.
$ sudo usb_modeswitch -K -v 0bda -p 1a2b
```
where `1a2b` is the product id from your device, if all goes well you should see it with the new product id:
```
$ lsusb 
Bus 001 Device 007: ID 0bda:b851 Realtek Semiconductor Corp.
```
> [!TIP] 
> If you don't have access to a Windows machine you can check `sudo dmesg` for clues of which mode your device is right after plugging it

<br/>
