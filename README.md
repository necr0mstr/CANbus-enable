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
  - lsusb command will show the device in DFU
```
lsusb
```
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/97d393d3-6ba3-440b-b4ab-b8e7c4aba173" width=50% height=50%>
- If the device is **not** in DFU mode then it will show as below
<img src="https://github.com/necr0mstr/CANbus-enable/assets/58074694/91e9d30e-17d5-48c2-bde5-fea1bfe4ea49" width=50% height=50%>






  

