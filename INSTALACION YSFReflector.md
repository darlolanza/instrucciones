```
cd ~
git clone https://github.com/g4klx/YSFClients.git
cd ~/YSFClients/YSFReflector
make clean all
mkdir /ysfreflector
cp ~/YSFClients/YSFReflector/YSFReflector /ysfreflector
cp ~/YSFClients/YSFReflector/YSFReflector.ini /ysfreflector
enregistrer le USFReflector sur le site :
https://register.ysfreflector.de
modifier le fichier YSFReflector.ini
```
```
groupadd mmdvm
useradd mmdvm -g mmdvm -s /sbin/nologin
mkdir -p /var/log/YSFReflector
chown mmdvm: /var/log/YSFReflector
```
```
vi /etc/systemd/system/ysfreflector.service
```
```
[Unit]
Description=YSFReflector

[Service]
Type=forking
ExecStart=/ysfreflector/YSFReflector /ysfreflector/YSFReflector.ini

[Install]
WantedBy=multi-user.target
Alias=ysfreflector.service
``` 

Installation YSF2DMR
```
git clone https://github.com/juribeparada/MMDVM_CM.git
cd ~/MMDVM_CM/YSF2DMR/
make
cd ~/MMDVM_CM/YSF2DMR/
mkdir /ysf2dmr
cp YSF2DMR /ysf2dmr/
cp YSF2DMR.ini /ysf2dmr/
cp DMRIds.dat /ysf2dmr/
cp XLXHosts.txt /ysf2dmr/
vi /ysf2dmr/YSF2DMR.ini
```
Dashboard
```
git clone https://github.com/dg9vh/YSFReflector-Dashboard.git
cp -R ~/YSFReflector-Dashboard/* /var/www/ysf
mkdir /var/www/ysf/config
```
```
vi /var/www/ysf/config/config.php
```
```
<?php 
# This is an auto-generated config-file! 
# Be careful, when manual editing this! 
date_default_timezone_set('UTC'); 
define("YSFREFLECTORLOGPATH", "/var/log/YSFReflector"); 
define("YSFREFLECTORLOGPREFIX", "YSFReflector"); 
define("YSFREFLECTORINIPATH", "/ysfreflector/"); 
define("YSFREFLECTORINIFILENAME", "YSFReflector.ini"); 
define("YSFREFLECTORPATH", "/ysfreflector/"); 
define("TIMEZONE", "UTC"); 
define("LOGO", ""); 
define("REFRESHAFTER", "60"); 
define("SHOWOLDMHEARD", "60"); 
define("TEMPERATUREHIGHLEVEL", "60"); 
?-->
```
```
mkdir /root/reflector-install-files

mkdir /root/reflector-install-files/ysfdash

mv /var/www/ysf/setup.php /root/reflector-install-files/ysfdash/original-setup.php

systemctl daemon-reload
service ysfreflector start
```






OTRO MAS

echo iniciando instalacion

cd /opt
git clone https://github.com/iu5jae/pYSFReflector.git
cd pYSFReflector/
#cp YSFReflector /usr/local/sbin/
#cp YSFReflector.ini /usr/local/etc/

cp YSFReflector /usr/local/bin/
chmod +x /usr/local/bin/YSFReflector
mkdir /etc/YSFReflector

cp YSFReflector.ini /etc/YSFReflector/
cd /etc/YSFReflector/
sudo sed -i 's/FilePath=\/var\/log/FilePath=\/var\/log\/YSFReflector/' YSFReflector.ini
sudo sed -i 's/42395/42000/' YSFReflector.ini
cd /opt/pYSFReflector/
cp deny.db /usr/local/etc/
chmod +x /usr/local/bin/YSFReflector
cd systemd/
cp YSFReflector.service /lib/systemd/system

sudo groupadd mmdvm
sudo useradd mmdvm -g mmdvm -s /sbin/nologin
mkdir /var/log/YSFReflector
sudo chown -R mmdvm:mmdvm /var/log/YSFReflector

###############################

mkdir /opt/YSF2DMR

cd /opt
git clone https://github.com/juribeparada/MMDVM_CM.git
sudo cp -r /opt/MMDVM_CM/YSF2DMR /opt/
cd YSF2DMR
sudo make
sudo make install