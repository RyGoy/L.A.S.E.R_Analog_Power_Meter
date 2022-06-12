# L.A.S.E.R_Analog_Power_Meter
L.A.S.E.R_Analog_Power_Meter (Scientech 360001)
# L.A.S.E.R. Analog Power Meter
(C) INGENIUM HOLDINGS INCORPORATED UNDER A CREATIVE COMMONS 3.0 UNPORTED LICENSE; BY-ND-NC

### Our Goal
We wanted to experiment with L.A.S.E.R. technology for displays and/or manufacturing; Of course safety was in the front of our minds. The goal of this project was to use an analog sensor to test untrusted diodes for their actual output values/potential.

**WARNING AND/OR DISCLAIMER!** 
EYE DAMAGE IS IMMEDIATE AND IREVERSIBLE, IF **YOU** DO NOT HAVE EXPERIENCE WITH ELECTRICITY AND/OR L.A.S.E.R. SAFETEY PRECAUTIONS THEN DO NOT USE ANY INFORMATION IN THIS DOCUMENT AND/OR ATTEMPT TO CONSTRUCT SUCH A DEVICE.

---

### Materials Required
- Raspbery Pi (We used a Pi 400)
- ADS1115 ADC Chip
- Jumper wires
- Breadboard
- Any analog sensor, ours is the Scientech 360001.
  
  We received specifications to calibrate this sensor from the manufacturer and don't have a technical manual.

---

  ### Prepare the Pi

We are using Debian Bullseye with the Pi Desktop (lite version), begin by bringing up a terminal and running:
>sudo apt-get update
>sudo apt-get upgrade -y
>sudo raspi-config

Enable SPI and I2C in the Raspberry Pi Config menu > Interface Options

Install [Adafruit_Blinka](https://github.com/adafruit/Adafruit_Blinka)

>sudo pip3 install adafruit-blinka

Then install [Adafruit_CircuitPython_ADS1x15](https://github.com/adafruit/Adafruit_CircuitPython_ADS1x15)

>sudo pip3 install adafruit-circuitpython-ads1x15
>pip3 install adafruit-ads1x15

### Make Pin Connections
**ADS -->>-- RPI**

SDApin ->>- SDApin(3)

SCLpin ->>- SCLpin(5)

GNDpin ->>- GNDpin(6)

VDDpin ->>- 3.3vpin(1) [connect last]

**ADS -->>-- AnaDevice**

VDDpin ->>- Analog 1

A0pin -->>- Analog 2

Using a breadboard makes this part easier

![image](https://user-images.githubusercontent.com/94795740/173250764-8c0513af-c37a-446c-aea5-13e216a35377.png)

https://www.engineersgarage.com/raspberry-pi-ads1015-ads1115-analog-sensor-interfacing-ir-sensor-interfacing/

### Create a Python Script to Print The Sensor Outputs to Screen

create a .py file with nano:
>sudo nano lasermeter.py

---

Paste this code into the .py file and save.

import time

import board

import busio

import adafruit_ads1x15.ads1115 as ADS

from adafruit_ads1x15.analog_in import AnalogIn

##### # Create the I2C bus
i2c = busio.I2C(board.SCL, board.SDA)

##### # Create the ADC object using the I2C bus
ads = ADS.ADS1115(i2c)

##### # Create single-ended input on channel 0
chan = AnalogIn(ads, ADS.P0)

##### # Create differential input between channel 0 and 1
##### # chan = AnalogIn(ads, ADS.P0, ADS.P1)

print("{:>5}\t{:>5}".format('raw', 'v'))

while True:
    print("{:>5}\t{:>5.3f}".format(chan.value, chan.voltage))
    time.sleep(0.5)

>Ctrl X to exit, Y to save.

---

>sudo reboot

Run the script
>sudo python lasermeter.py

You should see the values printing lines indicating the measurments. 

### Calibration
We're now waiting for proper eye protection and equipment, addtions to the project coming soon.


