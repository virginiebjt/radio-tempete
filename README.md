# radio-tempete
Play a webradio stream on a Raspberry Pi and print meta data on an OLED i2c screen

Hardware needed:
- Raspberry pi 3 or above (if you want bluetooth, otherwise any Raspberry will do)
- Micro SD card, min. 8go
- OLED display (the most common one is SSD1306, mine is SH1106), ex: https://www.ebay.fr/itm/1-3-White-OLED-LCD-Display-Module-IIC-I2C-Interface-128x64-3-5V-For-Arduino-/173560164703
- 4 dupont wire female to female (https://www.ebay.fr/itm/dupont-cable-liaison-pour-module-Arduino-male-ou-femelle-40-fils/171060862090?hash=item27d4059c8a:g:k6IAAMXQTT9Rv3uv&var=471169380327), usually comes with your oled display

We'll first stream the webradio to our device, and then set up the display.



## Play Radio Tempête on Raspberry Pi

Install Raspberry Pi OS (formely known as Raspbian) on your Raspberry Pi. Ideally, chose a desktop version for things to be easier. If you have a Raspberry Pi Zero, take Raspberry Pi Lite (without desktop version): you will have to do everything from the command line. Instructions here: https://www.raspberrypi.org/documentation/installation/installing-images/

Connect your Raspberry Pi to a keyboard, mouse and screen, or control it with your computer with SSH and VNC https://www.raspberrypi.org/documentation/remote-access/ssh/

Install Music Player Daemon. This will play any webradio stream. 
Run the following commands in your terminal
 
 ##### sudo apt-get update  
 sudo apt-get upgrade  
 sudo apt install mpc mpd
 
 Add your radio stream, in this case: https://listen.radioking.com/radio/372772/stream/422929 by running this command
 `mpc add https://listen.radioking.com/radio/372772/stream/422929`
 
 And play it:
  `mpc play`
 
 If everything's working well you should hear the music!

## Make the radio play on boot

You don't want to launch the script by yourself everytime you want to listen to music! So let's make the radio start when you boot your Raspberry Pi.

Write a bash script (name it however you want, I named it radiotempete) 
`sudo nano radiotempete.sh`

This will open a text editor in your terminal, with an empty page. Copy paste the following commands:
`#!/bin/bash  
mpc add https://listen.radioking.com/radio/372772/stream/422929  
mpc play  
mpc volume 50  
`
(I have set the volume to 50 instead of 100 because my radio was too loud, but it's optional).

Do Ctrl O and Ctrl X to save and quit.


Then, we'll be telling the Raspberry to start this bash script on startup.

Open your terminal and type
`sudo nano /etc/rc.local`

And copy paste this **before exit 0**
`# Radio tempete  
/home/pi/radiotempete.sh`

Save and quit (Ctrl O, Ctrl X).

Finally, test if it works: type sudo reboot in your terminal. The Rabsperry will restart, and should be playing Radio Tempête (or your station) straight away!

 
