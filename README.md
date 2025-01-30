# LibreSDR
This is an unofficial port of PlutoSDR onto LibreSDR a.k.a. ZynqSDR.              
There is not a whole lot of information from the manufacturer, but the device can be purchased on Aliexpress, eBay and other platforms.
The board schematics from one of the Aliexpress sellers is included, see zynqsdr_rev5.pdf
This set of patches was not tested for full functionality in every possible mode, probably contains bugs or incorrect interpretations of schematics and intended for fellow hackers to get started with low level software and FPGA modifications.

Prerequisites:
Prepare development environment for PlutoSDR v0.37, for details see 
https://github.com/analogdevicesinc/plutosdr-fw/tree/v0.37

This includes Vivado and Vitis 2021.2, please use only supported build host environment. 
Newer Vivado will not work due to specific dependencies in the HDL design 

How to build?

1. Clone this repository:
```sh
git clone git@github.com:day0wl/libresdr-fw.git
```
2. Change directory to the cloned source code and get original Pluto firmware code v.0.37 
```sh
cd  libresdr-fw
git clone --recursive --branch v0.37 https://github.com/analogdevicesinc/plutosdr-fw.git plutosdr-fw_0.37_libre
```
3. Apply patches
```sh
./apply.sh
```
4. Change directory to patched Pluto firmaware sources and build it
```sh
cd plutosdr-fw_0.37_libre  
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:/opt/Xilinx/Vitis/2021.2/gnu/aarch32/lin/gcc-arm-linux-gnueabi/bin
export VIVADO_SETTINGS=/opt/Xilinx/Vivado/2021.2/settings64.sh
export TARGET=libre
make
make sdimg
```
Make a cup of coffee, then probably one more.
Collect results in build directory and in addition a set of files for the sd card in build_sdimg

Usually LibreSDR is shipped with empty qspi flash and boots from included SD card.
Use any small capacity sd card, format it as FAT32 and copy content of the build_sdimg directory on it. Insert the sdcard into LibreSDR and it should boot from it.
Once it is running, the software can be updated using mounted drive, just like PlutoSDR.
This will populate QSPI flash and LibreSDR will be able to run without SD Card
USB OTG will act as normal PlutoSDR.
Serial console is available on the debug port as /dev/ttyUSB2 if you don’t have any other USB serial devices in the system. Set your terminal application to 115200N8
Gigabit Ethernet is enabled, IP address is set by default to 192.168.1.10

You can also use a full AD provided linux build, just replace BOOT.BIN and device tree file from this build and it should be able to mount the SD card as the root file system, This will aloow you to run any Linux applications or scripts directly on LibreSDR

From this point you are on your own, but pull requests and enhancements are welcome!

