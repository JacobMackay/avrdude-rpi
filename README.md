Avrdude for Raspberry Pi on Ubuntu
==================================
Modern Ubuntu Raspberry Pi kernels no longer use RPI.GPIO, and I was having trouble with the original avrdude, so I modified the original to make it work :)
This is a fork of https://github.com/SpellFoundry/avrdude-rpi

FYI I'm using to flash grbl onto an arduino as part of the protoneer-rpi cnc because i didn't want to use the outdated Raspbian.

Instructions to update avrdude:
-------------
1) Get the avr tools
```sudo apt install avrdude avr-libc gcc-avr make unzip python3-lgpio```
2) Get the avrdude-rpi code
```
cd ~/src # or your favourite place for random git repos
git clone https://github.com/JacobMackay/avrdude-rpi.git
cd avrdude-rpi
```
3) Copy both files into your /usr/bin directory, then rename the original avrdude to avrdude-original and symlink avrdude-autoreset to become avrdude.
```
sudo cp autoreset /usr/bin
sudo cp avrdude-autoreset /usr/bin
sudo mv /usr/bin/avrdude /usr/bin/avrdude-original
sudo ln -s /usr/bin/avrdude-autoreset /usr/bin/avrdude
```    

Modify the autoreset script to use the pin that you wired up to the reset pin.  See the line in autoreset where we do "pin = 17" and change the 17 to your gpio pin number (not board number).

Now when you run avrdude from anywhere (including via arduino's normal UI) it will flag dtr when it is about to upload hex data.

Instructions to flash grbl: Protoneer Raspberry Pi [Ubuntu]
---------------------------
1) Get the grbl source code:
```
cd ~/src # or your favourite place for random git repos
git clone https://github.com/grbl/grbl.git
```
2) Modify the config if desired
```
nano ~/src/grbl/grbl/config.h
```
3) Compile grbl
```
make clean
make grbl.hex
```
4) Flash the new grbl
```
sudo avrdude -v -p atmega328p -c arduino -P /dev/serial0 -b115200 -D -U flash:w:grbl.hex:i
```
5) Done! (I hope, this worked for me, so that's nice)

_____________________
