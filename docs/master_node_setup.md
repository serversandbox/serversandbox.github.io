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

you can always `sudo whoami` to check if account is root lol. linux mint comes with python3 installed by default on cinnamon

8. little ansible playbook for k3s (i could just write the install script, but there's nothing like fooling around to learn)

there's https://github.com/k3s-io/k3s-ansible for reference

[key terms](https://docs.ansible.com/ansible/latest/getting_started/index.html): 
* control node: where ansible is installed, where i run commands from. this is my mac.
* inventory: describe the things you wanna manage so ansible can use them ig? 
* managed node: in this case, where i want to put k3s

set up inventory.ini similar to the tutorial, then 

```
ansible-playbook -i inventory.ini install-k3s.yaml -K
BECOME password:

PLAY [Install and configure k3s single node cluster] ***********************************

TASK [Gathering Facts] *****************************************************************

<snip>

PLAY RECAP *****************************************************************************
192.168.50.100             : ok=12   changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

9. test if 6443 is accessible

```
macmint@macmint:~$ curl -k https://192.168.50.200:6443
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
```

10. use kubectl's built in merge to uh, merge configs so you can kubectl from outside

```
# check your external k3s config is pointing to its ip, this probably needs to be reworked eventually....

# run this within the repo directory

cp ~/.kube/config ~/.kube/config.backup

KUBECONFIG=k3s-config.yaml kubectl config view --flatten > ~/.kube/config.new

mv ~/.kube/config.new ~/.kube/config
```

it would be MUCH better if i'd renamed the k3s env to something else, instead of like "default", but another time

11. modify localhost etc/hosts file to point to pihole.local for dashboard >__>

12. try to update local device's dns servers first

```
16:52:14 in ~ using ☁️ terraform using ☁️  default/
➜ scutil --dns | grep nameserver
  nameserver[0] : 192.168.50.200
  nameserver[0] : 192.168.50.200

```

13. point router at dns server
