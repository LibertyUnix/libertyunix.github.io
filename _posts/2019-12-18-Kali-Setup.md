---
layout: post
title: Setting Up Your Kali Box
---
## Kali - Setup Script
This scripts builds and updates our Kali machine to get us ready to complete our day to day tasks. With Kali moving towards a rolling distro it has become must more stable to use as a daily OS.

#### If you are to nervous to live day to day as root you can always add a user in Kali:
```bash
useradd -m user
passwd user
usermod -a -G sudo user
chsh -s /bin/bash user
echo "###############################"
echo        "Setting Kali up"
echo        "Go get a drink"
echo "#################################"
apt-get update
apt-get upgrade
apt-get dist-upgrade 
apt-get install kali-linux-all
apt-get install gqrx gqrx-sdr
##if GQRX fails uncomment the line below
#apt-get update --fix-missing
apt-get install hackrf libhackrf-dev libhackrf0
apt-get install cmake libusb-1.0-0-dev make gcc g++ libbluetooth-dev pkg-config libpcap-dev python-numpy python-pyside python-qt4
apt-get install ubertooth
apt-get install bluez bluez-test-scripts python-bluez python-dbus libsqlite3-dev
git clone https://github.com/pwnieexpress/blue_hydra.git
cd blue_hydra/
bundle install
sleep 3
cd ../
pip install urh
echo "####### All Set - Happy Hacking!#######"
```

## A Simple Scanning Script
```bash
#!/bin/bash
#Simple bash script to 1st ping all ips in a range, then discover services, 
#pipe output to a file for each services, then further explore these services

echo "Enter IP range: "
read ips
nmap -sn -vv -oG tmp_scan $ips
cat tmp_scan | grep "Up" | cut -d " " -f2 > discoveredhost.txt
rm tmp_scan
nmap -sS --top-ports=100 -v -iL discoveredhost.txt -oG servicesup -oX metasploit-$clientname.xml
clear
echo "[*] Cleaning up and organizing"
cat servicesup | grep "22/open" | cut -d " " -f2 > ssh.txt
cat servicesup | grep "25/open" | cut -d " " -f2 > smtp.txt
cat servicesup | grep "21/open" | cut -d " " -f2 > ftp.txt
cat servicesup | grep "80/open" | cut -d " " -f2 > http.txt
cat servicesup | grep "443/open" | cut -d " " -f2 > https.txt
cat servicesup | grep "445/open" | cut -d " " -f2 > smb.txt
cat servicesup | grep "3389/open"| cut -d " " -f2 > rdp.txt
echo "[*] Happy Hunting - Run active-recon.sh!"
```
