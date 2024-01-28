**This document assumes you are already running Klipper on a Rapberry Pi or BTT Pi (you can use other computers).  With that assumption, you also know how to access the command line for the computer you use and have intermmediate knowledge of administration of Klipper and other software.**

# CANbus-enable
How to enable CANbus with RPi, Klipper, candleLight_fw, and katapult on BTT devices U2C and EBB42 

# Required software 
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

