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

note asus router fixes the first 3 bits, server is @ 200.

batteries are completely dead, so server needs to be moved otherwise the cat tends to disconnect the cable

7. ssh keygen

```
# on the mac, also name your key otherwise you kena
ssh-keygen -t ed25519 -C "macbook-to-mint-master"

# note filename: <users>/.ssh/ansible_key

ssh-copy-id <user@xx.xx.xx.200>
```

subsequent logins are just ssh <user@xx.xx.xx.200>, i can make an alias but i'd probably forget if i did

8. little ansible playbook for k3s (i could just write the install script, but there's nothing like fooling around to learn)

