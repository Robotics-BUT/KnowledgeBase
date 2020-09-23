# CAN Bus

Setting up the CAN bus is based on [this tutorial](https://www.hackster.io/youness/how-to-connect-raspberry-pi-to-can-bus-b60235).

Kernel-wise the following needs to be added to the `/boot/config.txt`

```
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25
dtoverlay=spi0-hw-cs
```

This should work most of the time, however in recent Raspberry Pi firmware builds there happened to be a problem with this approach. A solution to the problem was described [here](https://www.raspberrypi.org/forums/viewtopic.php?t=255470).
The solution utilizes the `pinout` program which can be installed using `sudo apt-get install python3-gpiozero`. Using this program the BCM number is read and then used in the `/boot/config.txt` as follows.
```
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25
dtoverlay=spi-bcm{BCM_NUMBER}
```
For example:
```
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25
dtoverlay=spi-bcm2837
```

Reboot is required after for the `/boot/config.txt` to take place.

To debug problems with the SPI or the MCP2515 use `dmesg | grep spi`.