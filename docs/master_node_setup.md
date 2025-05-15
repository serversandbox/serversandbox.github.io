# set up

logging so i don't forget what i did; to eventually try to automate this

1. flash usb drive with linux mint cinnamon 22.1 using balena etcher

```brew install --cask balenaetcher```

2. alt + power button on macbook pro 2015 to show boot drives, select correct drive

3. start linux mint, then install from the desktop -- this let me skip needing a ethernet connection to install the wifi drivers; was also prompted to install codecs so i can actually view youtube videos. it's quite performant, amazing

4. install ssh

```
sudo apt update && sudo apt upgrade
sudo systemctl status ssh

sudo systemctl enable ssh
sudo systemctl start ssh
```

5. once installed, go to `power management` and change default "suspend" when in clamshell mode

6. dhcp reservation - this can be done thru asus gui. handy commands `ip addr` to check success and device mac. 

```
1: lo
loopback interface, virtual network device used by your system to communicate with itself (localhost, 127.0.0.1), every linux system has it.
2: wlp3s0
wireless network interface; the name wlp3s0 follows the modern "predictable network interface naming" scheme:
wl = wireless LAN
p3 = PCI bus 3
s0 = slot 0
3. eth0 
ethernet
```
