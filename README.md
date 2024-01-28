**This document assumes you are already running Klipper on a Rapberry Pi or BTT Pi (you can use other computers).  With that assumption, you also know how to access the command line for the computer you use and have intermmediate knowledge of administration of Klipper and other software.**

# Enabling CANbus communication with your printer toolhead
How to enable CANbus with RPi, Klipper, candleLight_fw, and katapult on BTT devices U2C and EBB42 

## Required software 
When enabling CANbus using a U2C and EBB42 you need the following software
- https://github.com/bigtreetech/candleLight_fw/tree/stm32g0_support
  - The branch required is the **stm32g0_support** branch which allows you to build budgetcan_fw (required for U2C v2.1)
 
- https://github.com/Arksine/katapult
  - Previously **CanBoot** this is what is installed on the EBB42 which enables flashing Klipper over CANbus
 
- https://github.com/Klipper3d/klipper
  - Klipper should already be running on one of the following
    - Raspberry Pi
    - Raspberry Pi CM4
    - Bigtreetech Pi
    - Bigtreetech CB1

- Assuming you are running **Debian GNU/Linux 11 (bullseye)** install required packages for candleLight
  - cmake
  - gcc-arm-none-eabi
  - git

## Required hardware
This document is written for the following hardware
- Raspberry Pi
- Bigtreetech U2C v2.1
- Bigtreetech EBB42 v1.2
- Bigtreetech SKR3 E3 mini V3.0
- Ender 3 V2 (This should work with other printers)


### Cloning repositories and building firmware
```
mkdir ~/git_files && cd ~/git_files
git clone https://github.com/Arksine/katapult
git clone https://github.com/bigtreetech/candleLight_fw.git
```

### Install required OS packages
```
sudo apt-get update && sudo apt-get -y upgrade
sudo apt-get install -y cmake gcc-arm-none-eabi git
```

### Building budgetcan_fw for the U2C ###
```
cd ~/git_files/candleLight_fw/
git checkout stm32g0_support
mkdir ~/git_files/candleLight_fw/build && cd ~/git_files/candleLight_fw/build
cmake .. -DCMAKE_TOOLCHAIN_FILE=../cmake/gcc-arm-none-eabi-8-2019-q3-update.cmake
make budgetcan_fw
```

#### Put the U2C into DFU mode to flash firmware
- Disconnect the USB-C cable (if connected) from the U2C device.
- Hold the **BOOT** button while plugging in the USB-C cable and you should see the **blue** and **green** led lights come on.
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/96051b65-cad2-4d58-a4d7-e392a68bdc8d" width=20% height=20%>

- Confirm that the device is found in DFU mode with either of the below commands.
  - The ```lsusb``` command will show the device in DFU
```
lsusb
```
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/97d393d3-6ba3-440b-b4ab-b8e7c4aba173" width=50% height=50%>

- If the device is **not** in DFU mode then it will show as below
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/91e9d30e-17d5-48c2-bde5-fea1bfe4ea49" width=50% height=50%>

#### Flash budgetcan_fw to the U2C
- Flash the previously built **budgetcan_fw** to the U2C
```
cd ~/git_files/candleLight_fw/build
make flash-budgetcan_fw
```
- The output should show something similar to this
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/e0da3f3f-e085-4519-8902-0b030e41d3da" width=50% height=50%>

- Once completed, the device should show up as a CAN device in ```lsusb```
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/2d70acb9-7b23-4d4b-9c41-964ca1b313e3" width=50% height=50%>

- The command below should show **Total 0 uuids found**
```
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/633e17d5-85a9-4bea-b97b-9302eab1274a" width=50% height=50%>

### Building Katapult and flashing the EBB42

#### Building Katapult (formerly CanBoot) for EBB42
- Go into the previously downloaded [Github repo for Katapult]([url](https://github.com/Arksine/katapult)https://github.com/Arksine/katapult) and open **menuconfig**
```
cd ~/git_files/katapult/
make menuconfig
```
- Configure Katapult for the EBB42 (v1.1 or v1.2) device
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/93e1c289-bc0d-4770-8247-635e65a331f2" width=50% height=50%>

- Quit (q) and when prompted to **Save configuration** select Yes (y)
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/6fb13c7d-8dbc-4124-bf3a-389d86cfcd30" width=50% height=50%>

#### Flashing Katapult to the EBB42
- Enable USB-C connection by adding a jumper next to the USB-C plug
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/28365f9e-da5c-4db0-96cf-acf5a18bea21" width=50% height=50%>

- Put the EBB42 into DFU similar to the U2C
  - Press the **BOOT** button while plugging in the USB-C connector and you should see **blue** and **green** led lights come on.
  - Just like the U2C, you will see the device in DFU mode with the command ```lsusb```
  <img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/97d393d3-6ba3-440b-b4ab-b8e7c4aba173" width=50% height=50%>

  - If the device is **not** in DFU mode then it will show as below
  <img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/91e9d30e-17d5-48c2-bde5-fea1bfe4ea49" width=50% height=50%>


  

