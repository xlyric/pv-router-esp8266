# Pv router V2.10 under VS code

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

I try to create the cheapest pv router, with a ESP8266 ( or ESP32 ) and only some resistances and cheap components 
it use an oled screen and an SCT013 clamp. 
the power supply is and old 12v AC with electrical coil, and you must open it to separate 12 DC  and AC voltage 
the 12DC is used to make the sync with the network tension AC , the led is used as a 1 bit numeric converter ( ON / OFF ) 

the web interface is used to configure and have a visual to the router 
main informations are sent to mqqt server for login and used by domotics or other domotic server. 

if there is injection from PV production to network, the pv router send command to external numeric dimmer for command an extra use ( heat boiler , water tank ... ) 
https://github.com/xlyric/PV-discharge-Dimmer-AC-Dimmer-KIT-Robotdyn


<img src="https://nsa40.casimages.com/img/2019/12/23/191223091410613885.png">

# Prerequiement : 
A circuit board has been created and is available ( tips ) 
<img src="https://nsa40.casimages.com/img/2019/09/05/190905103700235594.png">

<img src="https://nsa40.casimages.com/img/2019/08/22/190822020621726681.jpg">
                                                                           
<img src="https://nsa40.casimages.com/img/2019/08/22/190822020621896704.png">                                                                           

# the necessary components are: 
 - An ESP8266 ( Nodemcu or Wemos ) 
 - 1 SCT013

<img src="https://ae01.alicdn.com/kf/HTB1FVJSXEjrK1RkHFNRq6ySvpXaR/YHDC-30A-50A-100A-SCT013-Non-invasive-AC-Current-Sensor-Split-Core-Current-Transformer-New-sct013000.jpg_220x220xz.jpg">

 - 1k resistors, 
 - a LED ( 3V ) , 
 - a capacitor 150nf polypropylene , 
 - a 5v regulator, 
 - connectors, 
 - a femelle Jack 
<img src="https://ae01.alicdn.com/kf/HTB1f4P3aovrK1RjSszfq6xJNVXaj/2Pcs-Set-3-5MM-Audio-Jack-Socket-3-Pole-Black-Stereo-Solder-Panel-Mount-Gold-with.jpg_220x220xz.jpg">

 - a 12V connector
<img src="https://ae01.alicdn.com/kf/HTB1tgeJXsnrK1RkHFrdq6xCoFXa1/10Pcs-3A-12v-For-DC-Power-Supply-Jack-Socket-Female-Panel-Mount-Connector-5-5mm-2.jpg_220x220xz.jpg">

 - an oled Display 

<img src="https://ae01.alicdn.com/kf/HTB1uK6AX._rK1Rjy0Fcq6zEvVXac/0-96-inch-IIC-Serial-White-OLED-Display-Module-128X64-I2C-SSD1306-12864-LCD-Screen-Board.jpg_220x220xz.jpg">

 - and a recovery transformer. 

# preparing power: 
Open the transformer and remove the diode bridge ( do not broke it ) 

<img src ="https://nsa40.casimages.com/img/2019/06/14/190614104905615784.jpg">

plug the bridge at 12AC-out and 5-10DC-IN

<img src="https://nsa40.casimages.com/img/2019/06/14/190614104905866769.jpg">

plug 12V connector at 12AC - IN 
and female connector to SCT013 on board

<img src="https://nsa40.casimages.com/img/2019/06/14/190614104906116772.jpg">

Connect the Oled Display on the connector on board ( OLED ) 


# Use : 
change nothing on the program, make only modification on data/config.json if needed or upload the 2 bin files ( code and littlefs )
upload the program on the esp8266 and connect to the new wifi network. you can send the code by OTA. 

the pv routeur will send information on your Domoticz server. 

on the Oled display, the IP is on screen. 
you can use web explorer for seen mesures and tune your pv router, 
the oscilloscope mode is on 

by default, send to domotic server is off ( for test ) you can activate it on the web page and don't miss to save /get?save the configuration 

<img src="https://nsa40.casimages.com/img/2019/07/11/190711093838371624.png">

# Domoticz regulation if needed 
Dimmer_heater.lua is configuration file for send to the dimmer. 
Domoticz_pvrouter_script.lua is the main file for sending to the digital dimmer. 
put them in domoticz/scripts/lua folder on your raspberry.
configure the config.json file for activate domoticz mode ( "UseDomoticz":true )

#autonome mode 
configure the config.json file for activate autonome mode ( "autonome":true ) and change the dimmer  IP
the program will try to equilibrate power in the external numeric dimmer ( https://github.com/xlyric/PV-discharge-Dimmer-AC-Dimmer-KIT-Robotdyn )

the full documentation is in the file Documentation.pdf ( in French ) 

*** old ***
bin files are added for easy installation. 
python ~/esptool.py --port /dev/ttyUSB4 --baud 115200 write_flash 0x00000 file.bin
and for SPIFFS python ~/esptool.py --port /dev/ttyUSB4 --baud 115200 write_flash 0x200000 file.bin

and for windows : 
esptool.py --port com3 --baud 256000 write_flash 0x00000 pvrouter.nodemcu.bin 

esptool.py --port com3 --baud 256000 write_flash 0x00200000 spiffs.img 
 


OTA update 
https://steve.fi/Hardware/ota-upload/
https://github.com/esp8266/Arduino/blob/master/tools/espota.py
python espota.py -d  -i 10.0.0.106 -f file.bin

*** 

Special commands

exemple :
Ip:/get&cosphi=11

"send"; /// paramettre de retour sendmode
"cycle"; /// paramettre de retour cycle
"readtime"; /// paramettre de retour readtime
 "cosphi"; /// paramettre de retour cosphi
"save"; /// paramettre de retour cosphi
 "dimmer"; /// paramettre de retour cosphi
"server"; /// paramettre de retour server domotique
"idx"; /// paramettre de retour idx
"idxdimmer"; /// paramettre de retour idx
 "port"; /// paramettre de retour port server domotique
"delta"; /// paramettre retour delta
"deltaneg"; /// paramettre retour deltaneg
 "fuse"; /// paramettre retour fusible numérique
 "apiKey"; /// paramettre de retour apiKey
 "servermode"; /// paramettre de retour activation de mode server
 "POWER"; /// paramettre de retour activation de mode server
 "facteur"; /// paramettre retour delta
 "tmax"; /// paramettre retour delta
 "mqttserver"; /// paramettre retour mqttserver

BETA : 
/cosphi find up point
/puissance  indicate power of charge

