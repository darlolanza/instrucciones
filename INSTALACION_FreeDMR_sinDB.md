# Instalacion de FreeDMR y HBMonv2 desde backup en Github

Recomendacion ejecutar todo con usuario root 
<em><b>las lineas separadas por un espacio ejecutar de a una, las que estan junta se pueden ejecutar de una vez.</b></em>

```terminal
cd /
apt-get update && apt-get upgrade -y
```

```terminal
apt-get install git python-pip python3-pip python-dev python3-dev libffi-dev libssl-dev -y

cd /opt

git clone https://github.com/hacknix/FreeDMR.git

cd FreeDMR

chmod 0755 install.sh
./install.sh

apt-get update

apt-get upgrade -y
```

<b>Creamos y copiamos los siguientes archivos.</b>

```terminal
mkdir config

cp FreeDMR-SAMPLE-commented.cfg config/FreeDMR.cfg

cp rules_SAMPLE.py config/rules.py

cp /opt/FreeDMR/systemd-scripts/freedmrrepeater.service /lib/systemd/system/freedmr.service
```

<b>Creamos el archivo encargado de correr la base de datos.</b>
Tiepeamos los siguiente y damos ENTER.
```terminal
nano /lib/systemd/system/proxy.service
```
Copiar este codigo y grabar el archivo con CTRL+X

```terminal
[Unit]

Description= Proxy Service 



After=syslog.target network.target



[Service]
User=root

WorkingDirectory=/opt/FreeDMR

ExecStart=/usr/bin/python3 hotspot_proxy_v2.py






[Install]
WantedBy=multi-user.target
```
<b>Ahora ejecutamos los comando para iniciar el servicio que utilizara la base de datos.</b>

```terminal
systemctl enable proxy.service
systemctl start proxy.service
systemctl status proxy.service
```
<b>Ahora ejecutamos el servicio del servidor FreeDMR.</b> 

```terminal
systemctl enable freedmr.service
systemctl start freedmr.service
systemctl status freedmr.service
```
<b>En este mosmento si vemos un mensaje de color verde <em>Active</em> hicimos todo correcto y el servidor esta funcionando correctamente.</b>

#####Instalacion de Hbmonitor

<em>Para instalar Apache en Debian 10 utilizaremos los paquetes que incluye la propia distribución en sus repositorios oficiales.</em>

```terminal
apt update && sudo apt -y upgrade
```
Hecho esto ya podemos instalar Apache. El paquete que necesitamos es apache2, así que lo instalamos mediante apt:

```terminal
apt -y install apache2
```
En unos instantes se descargará el paquete principal junto con todas sus dependencias. Una vez instalado todo el software, el nuevo servicio apache2 queda activado y en ejecución. Esto lo podemos comprobar mediante el comando ```systemctl status apache2```

Cada vez que hagamos cambios en la configuración de Apache estos no causarán efecto hasta que se recargue la configuración del servicio, lo que haremos con systemctl:

```terminal 
systemctl reload apache2
```
Ahora para instalar PHP en Debian 10 Buster

```terminal
apt update && sudo apt -y upgrade
```

```terminal
wget -O /etc/apt/trusted.gpg.d/php-sury.org.gpg https://packages.sury.org/php/apt.gpg
```

Creamos el archivo de configuración para el repositorio:

```terminal
nano /etc/apt/sources.list.d/php-sury.org.list
```

Con la siguiente línea como contenido:
```terminal
deb http://packages.sury.org/php/ buster main
```

Guardamos los cambios y actualizamos la información de las listas de paquetes:
```terminal
sudo apt update
```

Seguramente haya actualizaciones al activar este repositorio, así que actualizaremos los paquetes necesarios:

```terminal
sudo apt upgrade -y
```
Luego

```terminal
apt -y install libapache2-mod-php
```

```terminal
systemctl reload apache2
```
<b>Ahora procedemos a la instalacion del Monitor propiamente dicha.</b>

```terminal
cd /opt

git clone https://github.com/sp2ong/HBMonv2.git

cd HBMonv2

chmod +x install.sh
./install.sh

cp config_SAMPLE.py config.py
```

<b>Editamos el codigo del archivo config.py con nuestras preferencias.</b>

```terminal
nano config.py
```
<b>MUY IMPORTANTE, copiamos el contenido de la carpeta /opt/HBMonv2/ a /var/www/html</b>

```terminal
cp /opt/HBMonv2/html /var/www/html
```
Espero que este comando anterior funcione...

Luego

```terminal
cp utils/lastheard /etc/cron.daily/

chmod +x /etc/cron.daily/lastheard
```

<b>Finalmente copiamos y ejecutamos el servicio del Monitor.</b>

```terminal
cp utils/hbmon.service /lib/systemd/system/
```
```terminal
systemctl enable hbmon
systemctl start hbmon
systemctl status hbmon
```

Con amor LU9XRL.
lu9xrl@gmail.com

Extraido de los instructivos de cada autor de los servicios a instalar. El 90% del credito va para ellos.

FreeDMR is a DMR server, which is originally derived from the HBLink3 project. It aims to further extend the work of the Cortney Buffington, N0MJS and others. My thanks goes out to all who have made this project possible. (@hacknix dixit)

Python 3 implementation of N0MJS HBmonitor for HBlink https://github.com/kc1awv/hbmonitor3
Copyright (C) 2013-2018 Cortney T. Buffington, N0MJS n0mjs@me.com
This is version of HBMonitor V2 by SP2ONG 2019-2021








