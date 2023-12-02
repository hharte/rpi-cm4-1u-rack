# Raspberry Pi Compute Module 4 1U Rackmount Enclosure


# Introduction

This project consists of a [Mini-ITX](https://en.wikipedia.org/wiki/Mini-ITX) adapter and I/O plate to mount the [Raspberry Pi Compute Module 4 I/O board](https://www.raspberrypi.com/products/compute-module-4-io-board/) into the [iStarUSA D-118V2-ITX-DT 1U rackmount enclosure](https://www.amazon.com/iStarUSA-Compact-Desktop-mini-ITX-D-118V2-ITX-DT/dp/B0053YKPLM/).  Neither the I/O plate nor the adapter are strictly ITX-compliant; however they are a good fit for the iStarUSA enclosure.

The I/O plate includes openings for all CM4 I/O board connectors; however, the user may wish to remove the 12VDC barrel connector and/or the MicroSD card opening, depending on the use case.

[FreeCAD 0.20](https://www.freecad.org/) `FCstd` files are provided for easy modification.

![alt_text](https://raw.githubusercontent.com/hharte/rpi-cm4-1u-rack/main/photos/rpi_cm4_1u_rear.jpg "image_tooltip")

The `.stl` files for 3D-printing are also available on [Thingiverse](https://www.thingiverse.com/thing:5500278).


# Parts / Tools Required


## Required



* Four M2.5 screws.
* Four Female-Male M2.5 hex standoffs, 5 mm in length, with threads protruding 3 mm.
* M2.5 tap (alternatively, you can drill the four holes for the M2.5 standoffs slightly for an interference fit with the standoffs.)

Standard motherboard screws are used to attach the I/O plate to the iStarUSA chassis.


## Optional

1U Power Supply: [FSP Mini ITX Solution/Flex ATX 300W](https://www.amazon.com/dp/B08J2NDBWY) and an [ATX bridge adapter](https://www.amazon.com/SDTC-Tech-Starter-Without-Motherboard/dp/B07X9SVB5K).

One or two 40mm fans: [Noctura NF-A4x20](https://www.amazon.com/dp/B071W93333)

[PCIe Extender Cable](https://www.amazon.com/dp/B07TBLRZYJ)

If using an ITX/ATX power supply, it is necessary to buy or build an [ATX bridge adapter](https://www.amazon.com/SDTC-Tech-Starter-Without-Motherboard/dp/B07X9SVB5K).


# 3D Printing

Print the I/O plate with the rear face downward (stiffener ribs facing up) and use 100% infill.

The ITX adapter plate can be printed either side facing down, but I prefer to flip it over to have the nicer finish on the top.  Again 100% infill is recommended.

iStarUSA D-118V2-ITX-DT (from Monoprice [[link](https://www.monoprice.com/product?p_id=41762)]) 

Supports a full-height PCIe card.  Does not properly support a half-height PCIe card.  Modification is needed.


# Enclosure LEDs


### Blue Power LED

Splice a 510 ohm ⅛ watt resistor in line with the power LED (+) lead.  Plug the power LED (+) lead into CM4 J8 expansion header pin 2 (5V.)  Plug the power LED (-)  into expansion header pin 4 (GND.)


### Red HDD LED

The red HDD LED is controlled by software using GPIO22.

Splice a 330 ohm ⅛ watt resistor in line with the HDD LED (+) lead (blue.)  Plug the HDD LED (+) lead into expansion header J8 pin 15 (GPIO22.)  Plug the HDD LED (-)  into expansion header pin 17 (3.3V.)


#### Configure and turn off HDD LED

echo 22 > /sys/class/gpio/export

echo "out" > /sys/class/gpio/gpio22/direction 

echo "1" > /sys/class/gpio/gpio22/value 


#### Turn on HDD LED

echo "0" > /sys/class/gpio/gpio22/value 


# Reset Switch

The enclosure’s reset switch is connected to J1 pins 1 and 2 to disable eMMC boot when pressed.  This allows the CM4’s internal eMMC storage device to be mounted as a USB disk, if this switch is held while the system is powered on.  See [Jeff Geerling’s blog post for details](https://www.jeffgeerling.com/blog/2020/how-flash-raspberry-pi-os-compute-module-4-emmc-usbboot).


# Fans

The CM4 I/O board supports a PWM-controlled fan.  The enclosure has room for two 20mm fans.  The [Noctura NF-A4x20](https://www.amazon.com/dp/B071W93333) fans used in this project come with a Y-cable to allow two fans to be connected to the CM4 I/O board’s single fan connector.  They also come with mounting hardware and are very quiet.

To enable the fan driver when running a recent version of Raspberry Pi OS, add the following ([reference](https://github.com/raspberrypi/linux/issues/4632#issuecomment-1138400686)) to `/boot/config.txt`:


```
dtoverlay=i2c-fan,emc2301
```


To check the current fan speed, run the following as root:


```
cat /sys/class/hwmon/hwmon2/fan1_input
```


To check the current CPU temperature, run the following as root:


```
cat /sys/class/thermal/thermal_zone0/temp
```


The Noctura 


# Front Panel USB 2.0

Connect the front panel USB cables to the 2x5 header at J14 on the CM4 I/O board.  Desolder or bend pin 10 out of the way so it does not interfere with the key plug in the front panel cable connector.


# PCIe


## Bracket

The iStarUSA enclosure supports a full-height PCIe bracket.  It will not work with a half-height PCIe card without modification.


## Power

A 12VDC power supply is required when using the PCIe slot on the CM4 I/O board.


## Compatible PCIe devices

See [this list of PCIe devices](https://pipci.jeffgeerling.com/) and their current operational status with the CM4.


# Photos


## I/O Plate

![alt_text](https://raw.githubusercontent.com/hharte/rpi-cm4-1u-rack/main/photos/rpi_cm4_1u_io_plate.jpg "image_tooltip")


## Interior View

![alt_text](https://raw.githubusercontent.com/hharte/rpi-cm4-1u-rack/main/photos/rpi_cm4_1u_inside.jpg "image_tooltip")


## Front Panel USB Connection

Note: Pin 10 is bent out of the way to allow the enclosure’s front panel USB header to be installed.

![alt_text](https://raw.githubusercontent.com/hharte/rpi-cm4-1u-rack/main/photos/rpi_cm4_1u_fp_usb.jpg "image_tooltip")


# References

Raspberry Pi CM4 I/O Board ([manual](https://datasheets.raspberrypi.com/cm4io/cm4io-datasheet.pdf))

[iStarUSA D-118V2 case on Amazon](https://www.amazon.com/iStarUSA-Compact-Desktop-mini-ITX-D-118V2-ITX-DT/dp/B0053YKPLM/)

[RackChoice 1U on Amazon](https://www.amazon.com/dp/B0B3MG34D1)
