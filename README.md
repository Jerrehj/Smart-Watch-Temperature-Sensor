# Smart-Watch-Temperature-Sensor



<h4><b>Step 1: Acquireing Parts<b><h4>
  
  Parts needed 
  - x4 Male-male connectors
  - x1 36-pin 0.1" female header
  - x1 Short feather 12-pin male header
  - x1 Micro HDMI to HDMI adapter (if not using the method in the instruction guide) 
  - x1 TMP006 tempreature breakout sensor
  - x1 Raspberry pi zero w starter kit
  - x1 PCB
  - x1 Soldering iron 
  - x1 Sponge 
  - x1 Roll of lead-free solder

<h4><b>Step 2: Solderring and Assembly<b><h4>
  
Let's begin with the TMP006 sensor. You should solder the headers to your sensor first so you can test it start by soldering placing your  headers on to a breadboard then plance your sensor on top. solder each pin for a good connection using your soldering iron and solder 
you should have something that looks like whats below:
{completed TMP006}
next you'll want to solder some sockets to the raspberry pi 
{Completed Raspberry pi}
and finally soder sokects for the top of your PCB and headers for the bottom the idea is to get the headers of the sensor to connect to the socket of the PCB and it's headers to connnect to the pi
{completed PCB}
<h4><b>Step 3: Unit Testing<b><h4>
  
next a unit test you'll need four male-male conectors of diffrent colours which you'll conncet 
- PI 3V3-> Sensor VCC
- PI GND-> Sensor GND
- PI SDA-> Sensor SDA
- PI SCL-> Sensor SCL
see pipout out below  

{pinout}
{completed unit test}
<h4><b>Step 4: Getting Tempreature <b><h4>
  
to get tempreature your going to need your sd card. download rasbian form the raspberyy pi website{insert site here}, unzip and move the files onto your pi you'll want to download a few services on your you laptop if you are using one (the method used in these instructions) theses include: 
- Bonjour print services
- Ethcer 
- 7zip
- Notepad++

<h4>To setup these services to use a laptop<h4>

- Open "config.txt" file with Notepad++
- Scroll all the way down to the bottom
- Type "dtoverlay=dwc2" in the last line and then add an extra line after that
- Save and close the file
- Open "cmdline.txt" file with Notepad++
- Look for the section after "rootwait"
- Type "modules-load=dwc2,g_ether" in this section
- Save and close the file
- Create a new file called "ssh"
- Eject the MicroSD Card and place it on your Raspberry Pi


<h4>Next You Should Start You Pi<h4> 

- Connect your Raspberry Pi using a MicroUSB cable connected to the port marked "USB".
- Open up "Putty"
- Type "pi@raspberrypi.local" in the IP Address box
- Use your credentials pass: "raspberry"


<h4>Then Connect It To WIFI<h4>

- Type "sudo nano /etc/wpa_supplicant/wpa_supplicant.conf" Press "Enter" then go to the bottom of the text editor.
- Type in the following 

"network={

ssid="YOURNETWORKNAME"

psk="YOURNETWORKPASSWORD"

}"

- Press "Ctrl X"
- Press "Y"
- Press "Enter"
- Type "sudo wpa_cli reconfigure"

<h4>Getting Adafruit Libraries <h4>
The libraries nessary for this are located at https://circuitpython.org/libraries

You need to copy the following libraries out of the library bundle into your sd card:

- adafruit_tmp006.mpy
- adafruit_bus_device

h4>Enabling i2c and <h4>
  
  Type the following commands: 

- sudo apt-get install python-smbus
- sudo apt-get install i2c-tools

once done you can test you i2c address by typing "sudo i2cdetect -y 0"

<h4>Sensor Code<h4>
  this is the code you will be detecting acutal tempreature with 
  
<h4>"".py<h4>
import board
import busio
import adafruit_tmp006
 
i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_tmp006.TMP006(i2c) 

<h4>"".py<h4>
import time
import board
import busio
import adafruit_tmp006
 
# Define a function to convert celsius to fahrenheit.
def c_to_f(c):
    return c * 9.0 / 5.0 + 32.0
 
# Create library object using our Bus I2C port
i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_tmp006.TMP006(i2c)
 
# Initialize communication with the sensor, using the default 16 samples per conversion.
# This is the best accuracy but a little slower at reacting to changes.
# The first sample will be meaningless
while True:
    obj_temp = sensor.temperature
    print('Object temperature: {0:0.3F}*C / {1:0.3F}*F'.format(obj_temp, c_to_f(obj_temp)))
    time.sleep(5.0)    
    
<h4><b>Step 5: Building Your Case<b><h4> 
<h4><b>Step 6: Production Testing <b><h4>  
