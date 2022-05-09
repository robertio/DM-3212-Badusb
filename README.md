
# DM-3212

![image](https://user-images.githubusercontent.com/7502315/167411267-cb019812-e536-4b50-885d-2c4ccfe00244.png)


## Description 

Guide to flash ESP8266 in DM-3212. 

Descriptions like https://github.com/puckk/CJMCU-3212 DOES not works for me.

This is what works for me:


1: Put the DM-3212 as ESP programmer  -  This will connect Arduino serial to ESP serial.
    Run Arduino IDE -  step1.ino -upload https://github.com/puckk/CJMCU-3212/blob/master/step1.ino sketch 
    Set -FILE- Preferences - https://raw.githubusercontent.com/SpacehuhnTech/arduino/main/package_spacehuhn_index.json
    ![image](https://user-images.githubusercontent.com/7502315/167414866-57b22952-0794-4bd2-9701-8bb8c6404a92.png)

    Choose Board is Arduino Leonardo - 
        
    ![image](https://user-images.githubusercontent.com/7502315/167414322-47b374a2-56fe-4562-899b-a19e58edf1d8.png)


Unplug DM-3212 from usb

2. Shortcut the two pin. Plug it to USB while it is shortcutted. This will put ESP into Flash mode. 
    ![image](https://user-images.githubusercontent.com/7502315/167409876-4c3b91f8-1589-4791-81d2-dd999f3e6391.png)


3. Start ESP8266Flasher64.exe and Flash esp8266_wifi_duck_4mb.bin - this is the ESP image ESP networking, etc, like a "ESP firmware"
    Bin is here: https://github.com/spacehuhn/wifi_ducky/releases   
    Exe is from here: https://github.com/nodemcu/nodemcu-flasher/blob/master/Win64/Release/ESP8266Flasher.exe
    ESP blue led will blinking during Flashing.
    ![image](https://user-images.githubusercontent.com/7502315/167412778-79dc01eb-5bbd-4845-ad6e-89dfd2727b94.png)
    ![image](https://user-images.githubusercontent.com/7502315/167412846-e7c253d7-67c3-421e-8e2a-7ac3c9f5ccfb.png)
    ![image](https://user-images.githubusercontent.com/7502315/167413382-a39dc490-737a-4bb0-a334-1d03f1d45ab7.png)

    
4. Compile esp8266_wifi_duck.ino  (because esptool upload not works for me.)
    https://github.com/spacehuhn/wifi_ducky/blob/master/arduino_wifi_duck/arduino_wifi_duck.ino
    Set the board in Arduino IDE :
    ![image](https://user-images.githubusercontent.com/7502315/167409991-3e7b4fa8-1381-4090-8bf1-247619dc83d2.png)

    Set the clear wifi settings and sketch
    ![image](https://user-images.githubusercontent.com/7502315/167410257-53aafbaf-62c6-42de-a0c9-cc124e1e28dc.png)


    Start compile:
    ![image](https://user-images.githubusercontent.com/7502315/167410290-6b3f8dae-d86f-4fbd-b542-4ea04e71cd84.png)

    Bootom line -shows the settings- should look like this:
    ![image](https://user-images.githubusercontent.com/7502315/167410428-978a1371-657e-42dd-af4d-6e88bfd9508e.png)

Compilation will create esp8266_wifi_duck.ino.generic.bin 

5. Flash esp8266_wifi_duck.ino.generic.bin with ESP8266Flasher64.exe  - This will create Ducky webserver - 
    where you can config your ducky script.

6. repeat step 1 (to set back serial) - maybe this is not necessary

7 Attache to WIFI- SSID: WiFi Duck
    Password: quackquack

    Chrome:  http://192.168.4.1/ 

My first ducky script:
DELAY 30000
CTRL   ALT   DELETE
DELAY 2000
ENTER
DELAY 5000
STRING pin




END of set up DM-3212 


---------------------------------------------------------------------------------------------------------

This is the original page for reference only!!

For the guide we are going to use https://github.com/spacehuhn/wifi_ducky 

## Resources used

https://github.com/spacehuhn/wifi_ducky

https://github.com/espressif/esptool


## 1: Put the CJMCU as ESP programmer

The first thing to do is put the arduino as a programmer, so, in Arduino put 

  *Tools-> Board-> Arduino Leonardo*

  *Tools-> Port -> (the corresponding port)*

And upload this [sketch](https://github.com/puckk/CJMCU-3212/blob/master/step1.ino)


## 2: Upload the sketch on the ESP

download the 4m binary from [here](https://github.com/spacehuhn/wifi_ducky/releases)

Now, the Magic (or the solution that worked for me):

Disconnect the CJMCU.

Bridge the two metal circles on the top right before connect the CJMCU to the USB. (image)

![](https://i.imgur.com/5ght4Uu.jpg)

Once connected, run in esptool:

```
python esptool.py --trace   --baud 150200 --port /dev/ttyACM0 write_flash 0x00000 ../esp8266_wifi_duck.ino.generic.bin --flash_size 4MB --flash_mode dio --flash_freq 40m
```

Now, disconnect the bridge.


## 3: Upload the wifi ducky sketch

Now, upload https://github.com/spacehuhn/wifi_ducky/blob/master/arduino_wifi_duck/arduino_wifi_duck.ino

Ready, wifi_ducky is working =)

I hope this solution works for you, any problem ask me
