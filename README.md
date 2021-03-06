This code is no longer being developed - see the new Arduino Library (https://github.com/hardtechnology/efergy_v2)
-----

# EfergyMQTT
Efergy MQTT is a simple Efergy Data protocol to MQTT IoT code base designed to run on the ESP8266

The Efergy Power meters run at 433MHz (in AU) with FSK protocol. Using the DATA_OUT pad inside the Efergy E2 Classic or Elite Classic Receiver this code will allow you to capture this data and send it to an MQTT server of your choice. It will also output information on the Serial port of the ESP8266 for diagnostics.


# What's Supported
* Configuration of MQTT Server including TCP Port, username, password
* MQTT Will Topic for Online status of EfergyMQTT
* Configuration of WiFi Settings
* Reporting of Transmitter Battery Status (retained message)
* Notification of Transmitters that have their 'Link' button pressed recently
* Transmitter Online status (retained message)
* Transmitter lost packet
* Power usage in both mA (milliAmps) and Watts
* Reporting of ESP8266 VCC on startup


# Programming the ESP8266 Module
This code is creating in the Arudino IDE version 1.8.1. You will need to seperately install the required libraries and your ESP8266 board configuration.

We need the 'ESP8266 Community' boards to be added - follow this: https://learn.sparkfun.com/tutorials/esp8266-thing-hookup-guide/installing-the-esp8266-arduino-addon

Required Libraries (add them through the Arduino Libaries Manager)
* ArduinoJSON 5.8.0
* ESP8266 1.0.0
* WiFiManager 0.12.0
* PubSubClient 2.6.0

# Configuration
Once programmed, it will boot up in AP mode. This may take 1-2 minutes upon first boot as the file system is being prepared. You can then connect to the 'EfergyMQTT' access point with passphrase 'TTQMygrefE'. Once connected, browse to a random HTTP website and you will be redirected to the configuration page. File out your MQTT server information and the voltage of your power supply and click Save. The ESP will then restart and connect to your configured WiFi and MQTT Server.

To change the configuration the ESP needs to boot without being able to connect to your configured Access Point. Turning off the AP, or disabling the MAC address of the ESP in your AP will allow access to the configuration next time the ESP restarts.

NOTE: To see the Debugging output on the Serial port, set your baud rate to 74880 (ESP8266 default Baud Rate).


# Hooking it up
This is designed to run on an Adafruit Huzzah or Wemos D1 mini which will fit inside the battery compartment of the Efergy Receiver unit. Due to the additional power draw of the ESP module, the Efergy receiver will need a 5V power supply. This can be tapped into along with the DATA out of the RF receivier in the unit to operate the ESP8266 module.The DATA Out from the receiver should be hooked up to pin 16 on the ESP8266 module.

# Controlling
The following MQTT Topics can be used to control the module:

%MQTTClientname%/CONFIG/RESET
* Value: 'RESET' - will restart the ESP8266 module
* Value: 'CONFIG' - will restart and launch the configuration AP Mode

%MQTTClientname%/CONFIG/VCC
* Value: n/a - will reply on %MQTTClientname%/VCC with the current VCC Voltage

%MQTTClientname%/CONFIG/EFERGYVOLTAGE
* Value: <volts> - will set the voltage used for Wattage calculations
* NOTE: The configured value set in config mode will re-apply after a restart

%MQTTClientname%/CONFIG/MILLIAMP
* Value: 0/1 - Enable or disable the MilliAmp output
* NOTE: The configured value set in config mode will re-apply after a restart

%MQTTClientname%/CONFIG/VERSION
* Value: n/a - will reply on %MQTTClientname%/VERSION with the current Software Voltage

%MQTTClientname%/CONFIG/FILTER/TYPE
* Value: 'blacklist', 'whitelist' or 'disabled' - Transmitter ID's added to filter will act in this way
* NOTE: All received Transmitters will still show on Serial line for debugging.

%MQTTClientname%/CONFIG/FILTER/ADD
* Value: Transmitter ID number to add to Filter list

%MQTTClientname%/CONFIG/FILTER/REMOVE
* Value: Transmitter ID number to add to Filter list
