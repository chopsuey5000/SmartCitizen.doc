Getting Started
=====

### Adding a Smart Citizen Kit

![Smart Citizen kit 1.1](img/sck_1.1_3.jpg)

Thanks! The Smart Citizen Team wants to thank you for being here, for purchasing a kit, joining the platform taking part on this adventure.

To join the Smart Citizen community we are going to show you how to add your SCK kit to the platform.

For this, go to <a href="https://smartcitizen.me/" target="_blank">smartcitizen.me</a> through your web browser(choose Firefox or CHROME). Click Register in the upper menu. Fill the form fields and click register.

Once you’ve completed the registration you’ll be in your dashboard. To add your SCK kit, click on ADD SMART CITIZEN KIT.

Fill all the fields, under the labels SET A NEW KIT, SET UP KIT GEOLOCATION, DESCRIBE YOUR KIT, then click on SAVE SENSOR button.

Now, your SCK kit has been registered. Switch on your SCK kit and click on CONFIGURE button to continue the process. 

If is the first time you register a kit you should install the extension of Codebender for your browser in order to configure the  SCK kit. 

Once you’ve installed the extension, return to smart citizen site. You’ll notice that the content has changed. Click on START PROCESS button. Wait some seconds while Codebender plugin does its tasks. 

When it has finished you’ll see a form to setup your wifi connection and data update interval parameters. Fill the fields with your ssid, encryption mode and password phrase. Then, click on SYNC button. Wait some seconds while your kit it’s been configured.

When the sync has finished, don’t forget to click on the REGISTER KIT button on the bottom to add de MAC ADDRESS. Now reset your kit in order the changes take effect. 

Back to your dashboard. Wait two minutes and reload the page, pressing ctrl+R or cmd+R depending on your operating system, to see if some data has been uploaded to the platform. To check this take a look at the value in Last Update: field.

### Manual set up: The Serial Way 

In this tutorial you will configure your Smart Citizen Kit (hereafter SCK) using serial communication. By using serial communication, you will register your Wi-Fi settings into the SCK and save the SCK’s MAC address in our server.
 
The SCK, like most Arduino chips, has the ability to communicate through serial protocol (when plugged with a proper USB cable). The SCK uses the WiFly module to communicate with your Wi-Fi router. Anyway, through serial communication you will be able to send the commands directly with this module to set your Wi-Fi settings and extract the MAC address used by our server to verify your identity.
 
Note that this tutorial works for both SCK v1.0 (from the Goteo crowdfunding campaign) and SCK v1.1 (from the KickStarter crowdfunding campaign), independently of the firmware version used.
 
All the steps in this tutorial have been tested with Arduino 1.0.5. We strongly recommend not to use the Beta version of Arduino IDE as we encountered issues with drivers and serial communication.

STEP 1: Configuring the Wi-Fi settings

- Open Arduino IDE.
- Connect your SCK via USB.
- From the Tools > Board menu, choose the right USB port (generally the last one).
- From the Tools > Serial port menu, select the right board. This is Leonardo for SCK v1.0 (Goteo) or LilyPad for SCK v1.1 (Kickstarter).
- Open the serial monitor window in the Arduino IDE (button at the top-right of the main window).
- Set the options to "115200 baud" and “No line return” (drop-down menu at the bottom-right of the monitor window).
- Wake up the module and activate the Wi-Fi command mode:
*$$$*

- Change to “Carriage return” option (drop-down menu at the bottom-right of the monitor window)

- Add a new SSID to memory by typing:

*set wlan ssid XXX**

- Add a new phrase to memory (optional, password for WPA1 & WPA2):*

*set wlan phrase XXX**

- Add a new key to memory  (optional, password for WEP & WEP64):*

*set wlan key XXX*

- Add an authentication method into memory (replace XXX by “0” for Open,  “1” for WEP, “2” for WPA1, “4” for WPA2, “8” for WEP64):

*set wlan auth XXX*

- Add an antenna type into memory (replace XXX by “0” to use the internal antenna or “1” if you use an external antenna):

*set wlan ext_antenna XXX*

- Get the MAC address of the kit

*get mac*

- Copy/Save the answer for further use.

- Exit and go back to the normal operational mode

*exit*
 
*Note: You have to replace XXX with your value, filling any space with the dollar ($) character .


STEP 2: Registering your kit online

- Go to the configuration page of your kit: http://smartcitizen.me/devices/configure/XXX (do not forget to replace XXX with your kit's ID)

- In the MAC address text input, paste the MAC address given by the command *get mac*. It should be something like: *00:06:66:21:17:33*

- Click on "Register the kit"
 
You are now done with the manual configuration of your SCK. Wait for a few minutes to see your data coming on the server and being displayed on the web page. You can also check that everything is ok by looking at the Arduino serial monitor. Debug messages coming from your SCK should look like this:


If you want to explore further options with the WiFly module, a full list of commands is available at:
https://github.com/fablabbcn/Smart-Citizen-Kit/blob/master/sck-commands.md

If you encounter any issue, please share your problem on the forum:
http://forum.smartcitizen.me/



### Manual set up: The Compilation Way

This tutorial will drive you toward a manual way of setting your Smart Citizen Kit (SCK) by editing directly the source code. As the code is Open Source, one way of setting the Wi-Fi of your SCK is to download the latest firmware, edit some lines of code, recompile it and upload it to the kit. 
 
One advantage of this system is that it gives you the opportunity to register multiple Wi-Fi networks at the same time. This is useful if your SCK is traveling from one location to another where the Wi-Fi credentials are known. The downside of this method is that you can not extract the MAC address of your kit, therefore making it this way is not suitable for the first setup of your kit. Please, refer to other tutorials available.
 

#### STEP 1: Getting the Firmware

You can download the latest firmware on our Github : 
https://github.com/fablabbcn/Smart-Citizen-Kit/releases
As you may know, the hardware and software are based on the arduino project. we will use the Arduino IDE to edit the firmware and upload it to the kit. This tutorial have been tested with arduino 1.0.5. Get the Arduino IDE at http://arduino.cc/en/Main/Software.

Open the file Smart-Citizen-Kit/sck_beta_v0_8_5/sck_beta_v0_X_X.ino

#### STEP 2: Editing the code

If you want to set the network configuration manually, you should go to the SCKBase tab and modify the lines you see below:
 
```Arduino
#define networks 0
#if (networks > 0)
char* mySSID[networks]      = { 
  "Red1"        , "Red2"        , "Red3"             };
char* myPassword[networks]  = { 
  "Pass1"      , "Pass2"       , "Pass3"            };
char* wifiEncript[networks] = { 
  WPA2         , WPA2          , WPA2               };
char* antennaExt[networks]  = { 
  INT_ANT      , INT_ANT       , INT_ANT            }; //EXT_ANT
#endif
 ```
 
The easiest way would be to write "#define redes X" (where X is the number of WI-FI networks you are going to use),  add the name of your network in "RedX" and the corresponding password in "PassX". You could also choose the encryption mode that fits with your network's configuration (OPEN, WEP, WPA1, WPA2, WEP64) or the type of antenna you are using (*INT_ANT* for internal antenna (default) or *EXT_ANT* for external antenna).
 
If you register only one wifi credential, you should obtain something like:
 
 ```Arduino 
#define networks 1
#if (networks > 0)
char* mySSID[networks]      = { 
  "MyWifiSSID"    };
char* myPassword[networks]  = { 
  "MyPassword"    };
char* wifiEncript[networks] = { 
  WPA2            };
char* antennaExt[networks]  = { 
  INT_ANT         }; //EXT_ANT
#endif
 ```
 
#### STEP 3: Registering the kit in the database

After you've uploaded your own script, don't forget to register the kit in our database and save there your kit mac address. To find this mac address, you can use the serial command "get mac". by following the tutorial Manual Setup, the Serial way. 

Alternatively, have a look at the wifi module on the board and read the serial number  under the bar code (something like "131G0006662116E4" on kit v1.0 or "0006662116E4" on kit v.1.1). 

The mac adress is the last 12 digit of this serial, separated by colon every two number. You should obtain something similar to "00:06:66:21:16:E4".

In both cases, you have to pass by the configuration page of your kit, and fill the mac address input field. Then press the register your kit button.
 
Wait for some minutes and you should see data comming to the website !
 
If you have any question or are looking for more information, have a look at our forum :
http://forum.smartcitizen.me


### Attaching the solar panel
### Sensor calibration
### Connecting from an iPhone


The Sck Command Line
=====
### Basic SCK commands

- $$$ Wake up the module and activate the Wi-Fi
- set wlan ssid XXX\r Add a new SSID to memory
- set wlan phrase XXX\r Add a new phrase to memory
- set wlan key XXX\r Add a new key to memmory
- set wlan auth XXX\r Add an authentication method into memory
- set wlan ext_antenna XXX\r Add an antenna type into memmory
- get mac\r Get the MAC address of the kit
- exit\r Go back to normal operational mode

### Special SCK commands

- get time update\r Retrieve the sensor update interval
- set time update XXX\r Update the sensor update interval
- get number updates\r Retrieve the max number of bulk updates allowed
- set number updates XXX\r Update the max number of bulk updates allowed
- get apikey\r Retrieve the kit APIKEY
- set apikey XXX\r Update the kit APIKEY
- get wlan ssid\r Retrieve the SSID saved on the kit
- get wlan phrase\r Retrieve the phrase and KEY saved on the kit
- get wlan auth\r Retrieve the authentication methods saved on the kit
- get wlan ext_antenna\r Retrieve the antenna types saved on the kit
- clear nets\r Remove all saved Wi-Fi configuration information
- exit\r Goes back to normal operational mode
- data\r Retrieves sensor readings stored in memory

### Basic WiFly commands

Hardware
=====
### Inside the SCK

![Main Board](img/main_board.jpg)

#### Main Board

The main board contains the basic functionality like sensor I/O to read de sensor values, communicate with the platform through the wifi module, manage the power and battery charging.

![Main Board CPU](img/main_board_cpu.jpg)

**CPU** 

Both versions of the SCK (1.0 and 1.1) are using the same CPU, ATMEGA32U4(Arduino Leonardo). With the difference that the 1.0 works at 5V and 16MHZ and the 1.1 works at 3.3V and 8MHZ. In the 1.1 version we’ve improved the power consumption. 

This CPU has native USB and an UART TTL port allowing us to connect directly with the WIFI module.

<a href="http://www.atmel.com/images/doc7766.pdf" target="_blank">ATMEGA32U4 datasheet</a>

![usb connectors](img/usb_connectors.png)

**USB CONNECTOR**

The 1.0 version uses a Mini USB connector and 1.1 version uses a Micro USB.

![wifly module](img/main_board_wifly.jpg)

**WIFI MODULE**

The RN-131 module is a standalone, embedded wireless 802.11 b/g networking module. With its small form factor and extremely low power consumption, the RN-131 fits perfectly for the SCK wireless communication requirements.

Main features:

- Qualified 2.4-GHz IEEE 802.11b/g transceiver
- Ultra-low power: 4 uA sleep, 40 mA Rx, 210 mA Tx
- High throughput, 1 Mbps sustained data rate with TCP/IP and WPA2
- Small, compact surface-mount module
- On-board ceramic chip antenna and U.FL connector for external antenna
- 8-Mbit flash memory and 128-KB RAM
- UART hardware interface
- 10 general-purpose digital I/O pins
- 8 analog sensor interfaces
- Real-time clock for wakeup and time stamping
- Accepts 3.3-V regulated or 2 to 3 V battery
- Supports ad hoc and infrastructure networking modes
- On board ECOS -OS, TCP/IP stacks
- Wi-Fi Alliance certified for WPA2-PSK
- FCC/CE/ICS certified and RoHS compliant.
- Industrial (RN-131G) and commercial (RN-131C) grade temperature options

<a href="http://ww1.microchip.com/downloads/en/DeviceDoc/rn-131-ds-v3.2r.pdf" target="_blank">WIFLY module - RN-131 datasheet</a>


![MCP1725](img/main_board_MCP1725.jpg)

**BATTERY POWERING**

For powering the SCK, in both versions, we are using a 3.7v 2000 mAh li-on battery. 

In 1.0 version, SCK uses two different voltages, 3.3V and 5V to power the IC’s. 

To elevate to 5V we are using a step-up based on NCP1400, thus having a stable voltage at 5v and 100mA.

On the other hand, to regulate the voltage and to obtain 3.3v, the SCK uses the IC MAX604.

In 1.1 version, to simplify, the voltage of entire SCK was unified to 3.3V. The responsible to regulate the voltage from 3.7v to 3.3v is the MCP1725 IC.

<a href="http://www.onsemi.com/pub_link/Collateral/NCP1400A-D.PDF" target="_blank">NCP1400 datasheet</a><br>
<a href="http://www.solarbotics.net/library/datasheets/MAX604.pdf" target="_blank">MAX604 datasheet</a><br>
<a href="http://ww1.microchip.com/downloads/en/DeviceDoc/22026b.pdf" target="_blank">MCP1725 datasheet</a>


![MCP73831](img/main_board_MCP73831.jpg)

**BATTERY CHARGING**

For charging the battery there are two ways, USB or solar panel. To carry out the charging we are using MCP73831 IC. 

For the solar panel way, in 1.0 version the solar panel have to be 12v and 500mA. In 1.1 version, the solar panel can be more versatile in terms of amperage. 

<a href="http://ww1.microchip.com/downloads/en/DeviceDoc/20001984g.pdf" target="_blank">MCP73831 datasheet</a>

![LM2674](img/main_board_LM2674.jpg)

**SOLAR PANEL CHARGING**

As the solar panel produces 12v, depending on the sunlight conditions, we have to reduce the voltage to 5v to feed up the Vin of the MCP73831 charger IC. 

To carry out this task we are using the LM2674 IC, a very efficient IC, with a rate of 91% of performance.

<a href="http://www.ti.com/lit/ds/symlink/lm2674.pdf" target="_blank">LM2674 datasheet</a>

![DS1339Y-3+](img/main_board_DS1339Y-3+.jpg)

**RTC (REAL TIME CLOCK)**

The SCK has a real time clock for when the kit is offline. For this task we chose the DS1307 IC for the 1.0 version and the DS1339Y-3+ IC for the 1.1 version. Different IC due to the different voltages, 5V for the 1.0 version and 3.3V for the 1.1 version.

<a href="http://datasheets.maximintegrated.com/en/ds/DS1307.pdf" target="_blank">DS1307 datasheet</a><br>
<a href="http://datasheets.maximintegrated.com/en/ds/DS1339-DS1339U.pdf" target="_blank">DS1339Y-3+ datasheet</a>

![DM3CS](img/main_board_DM3CS.jpg)

**SD CARD READER**

The SD card is used to store the data captured by the sensors when the kit is offline. Then, when the kit is online, the data will be uploaded to the platform. 

To hold the SD card we are using the DM3CS holder. The SD card is powered at 3.3V and communicates with the CPU through SPI protocol.

<a href="http://www.mouser.com/ds/2/185/e60900232-38395.pdf" target="_blank">DM3CS datasheet</a>


![24LC256](img/main_board_24LC256.jpg)

**EEPROM MEMORY**

For the users that don’t have a SD card we’ve added an EEPROM memory to store the data when the SCK is offline.

We chose the 24LC256 IC that can store 32kBytes, it communicates with the CPU through I2C protocol.

<a href="http://ww1.microchip.com/downloads/en/DeviceDoc/20001203U.pdf" target="_blank">24LC256 datasheet</a>


**MAIN BOARD BASIC SENSORS**

The main board has some basic sensors:

- Measurement of the battery level
- Measurement of the solar panel level
- Measurement of the wireless networks detected

![Sensor Board](img/sensor_board.jpg)

#### SENSOR BOARD

The sensor board contains the necessary sensors for measuring the pollution parameters. This means NO2 and CO gases, sunlight, noise pollution, temperature, humidity. Also, the sensor board has an I2C bus, this allows to expand the SCK to other kind of sensors.

![MICS4514](img/sensor_board_MICS4514.jpg)

**NO2 AND CO2 SENSORS**

To measure these two gases we chose <a href="http://www.e2v.com/" target="_blank">e2v</a> sensors. In particular, metal oxide sensors MICS5525 and MICS2710, for version 1.0. And MICS4514, for version 1.1, that contains both sensors in one.

Metal oxide sensors are based on oxide semiconductors. Their electrical conductivity is modulated due to the reaction between the semiconductor and the gases in the atmosphere.

<a href="http://www.e2v.com/shared/content/resources/File/sensors_datasheets/Metal_Oxide/mics-5525.pdf" target="_blank">MICS5525 datasheet</a><br>
<a href="http://www.cdiweb.com/datasheets/e2v/mics-2710.pdf" target="_blank">MICS2710 datasheet</a><br>
<a href="http://files.manylabs.org/datasheets/MICS-4514.pdf" target="_blank">MICS4514 datasheet</a>

![H1730FVC](img/sensor_board_BH1730FVC.jpg)

**LIGHT SENSOR**

The light sensor is a basic element to know the light pollution. In version 1.0, was used a LDR(light-dependent resistor) whose voltage varies depending on the light conditions.

For version 1.1, was used a photodiode BH1730FVC. This sensor contains an I2C bus that gives us directly the amount of lux of ambient and infrared light.

<a href="http://www.farnell.com/datasheets/1813319.pdf" target="_blank">BH1730FVC datasheet</a>

![Noise Sensor](img/sensor_board_noise_sensor.jpg)

**NOISE SENSOR**

The noise sensor is based on an electret microphone. For the version 1.0, the audio signal is passed through an operational amplifier configured as band pass filter.

For the version 1.1, the amplification step was modified adding a variable gain, allowing us to measure very low and very high signals.

![SHT21 Sensor](img/sensor_board_SHT21.jpg)

**TEMPERATURE AND HUMIDITY SENSOR**

To measure temperature and humidity was used a module that integrates both sensors. 

For version 1.0 was used the RHT22, it has one wire digital interface.

For version 1.1 was used the SHT21, it has I2C protocol and a more fast response between measures than the RHT22.

<a href="https://www.sparkfun.com/datasheets/Sensors/Temperature/DHT22.pdf" target="_blank">RHT22 datasheet</a><br>
<a href="http://www.sensirion.com/fileadmin/user_upload/customers/sensirion/Dokumente/Humidity/Sensirion_Humidity_SHT21_Datasheet_V4.pdf" target="_blank">SHT21 datasheet</a>

![ADXL345](img/sensor_board_ADXL345.jpg)

**3 AXIS ACCELEROMETEER**

In version 1.0 was detected that some measures vary depending on the orientation the SCK. 

For this, in version 1.1, was added an accelerometer(ADXL345) to detect the position and to compensate the measures depending on the orientation of the SCK.

The ADXL345 has I2C protocol to interface with.

<a href="http://www.analog.com/static/imported-files/data_sheets/ADXL345.pdf" target="_blank">ADXL345 datasheet</a>

![I2C Bus](img/sensor_board_i2c_bus.jpg)

**I2C EXPANSION BUS**

Due to the ease of the I2C protocol. We’ve included and I2C bus to provide to the community the opportunity of expanding the SCK.


### Enclosures
### Acrylic cases 


Software
====
### Inside the SC Platform



Faq
====
### How to store data in your own database?
### How to use the SD Card?
### Which LiPo batteries to use?
### Which solar panels to use?
### How to setup the SCK on Ubuntu?
### Actual accuracy of all the sensors? 
### What is the spec (battery type) for the button-cell for the RTC?
### Why is 50dB the microphone lowest value?
### How do I calibrate the sensors?
### Browsers compatibility


Troubleshooting
====
### Add an SSID with two words
### Serial port already in use
### No port available
### Unable to connect to the Board
### Unable to connect to the Internet
### No data received yet
### Error in connection
### Port is not appearing on the drop down
### Performed install but still no data
### Firmware update problem
### Sensors erratic behaviour
### No MAC address registered
### Collapsed USB port
### Broken LiPo battery wire




Apps
====
### iOS
### Android
