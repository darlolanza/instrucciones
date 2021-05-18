*Instalacion de FreeDMR desde backup en Github

#####Recomendacion ejecutar todo con usuario root 

```terminal
cd /
apt-get update && apt-get upgrade -y
```

```terminal
apt-get install git python-pip python3-pip python-dev python3-dev libffi-dev libssl-dev -y

cd /opt

git clone https://github.com/hacknix/FreeDMR.git

cd /FreeDMR

chmod 0755 install.sh
./install.sh

apt-get update

apt-get install software-properties-common gnupg2 -y

apt-get upgrade -y
```

```terminal
apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8

add-apt-repository 'deb [arch=amd64] http://mariadb.mirror.liquidtelecom.com/repo/10.4/debian buster main'

apt-get update

apt-get install mariadb-server mariadb-client -y

mysql_secure_installation
```

<b>Precionamos ENTER y seguimos las instrucciones de la instalación de mariaDB.</b>

			NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
			      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

			In order to log into MariaDB to secure it, we'll need the current
			password for the root user. If you've just installed MariaDB, and
			haven't set the root password yet, you should just press enter here.

			Enter current password for root (enter for none): 
			OK, successfully used password, moving on...

			Setting the root password or using the unix_socket ensures that nobody
			can log into the MariaDB root user without the proper authorisation.

			You already have your root account protected, so you can safely answer 'n'.

			Switch to unix_socket authentication [Y/n] y
			Enabled successfully!
			Reloading privilege tables..
			 ... Success!


			You already have your root account protected, so you can safely answer 'n'.

			Change the root password? [Y/n] y
			New password:  
			Re-enter new password: 
			Password updated successfully!
			Reloading privilege tables..
			 ... Success!


			By default, a MariaDB installation has an anonymous user, allowing anyone
			to log into MariaDB without having to have a user account created for
			them.  This is intended only for testing, and to make the installation
			go a bit smoother.  You should remove them before moving into a
			production environment.

			Remove anonymous users? [Y/n] y
			 ... Success!

			Normally, root should only be allowed to connect from 'localhost'.  This
			ensures that someone cannot guess at the root password from the network.

			Disallow root login remotely? [Y/n] n
			 ... Success!

			By default, MariaDB comes with a database named 'test' that anyone can
			access.  This is also intended only for testing, and should be removed
			before moving into a production environment.

			Remove test database and access to it? [Y/n] y
			 - Dropping test database...
			 ... Success!
			 - Removing privileges on test database...
			 ... Success!

			Reloading the privilege tables will ensure that all changes made so far
			will take effect immediately.

			Reload privilege tables now? [Y/n] y
			 ... Success!

			Cleaning up...

			All done!  If you've completed all of the above steps, your MariaDB
			installation should now be secure.

			Thanks for using MariaDB!

```terminal
mysql -u root -p

mypassword
```
<b>Dentro de la base de datos ingresamos una a una las siguientes lineas.</b>

```terminal
CREATE DATABASE hblink;

USE hblink

CREATE USER 'hblink'@'localhost' IDENTIFIED BY 'mypassword';

GRANT ALL PRIVILEGES ON hblink.*TO 'hblink'@'localhost';
```
<b>Luego copiamos el siguiente codigo para crear la tabla de los Hotspot, TENER EN CUENTA QUE FINALIZA CON PUNTO Y COMA.</b>

```terminal
CREATE TABLE `repeaters` (
  `CALLSIGN` varchar(8) NOT NULL,
  `MODE` varchar(8) NOT NULL DEFAULT 'MASTER',
  `ENABLED` tinyint(1) NOT NULL DEFAULT 1,
  `_REPEAT` tinyint(1) NOT NULL DEFAULT 1,
  `MAX_PEERS` int(11) NOT NULL DEFAULT 10,
  `EXPORT_AMBE` tinyint(1) NOT NULL DEFAULT 0,
  `IP` varchar(255) NOT NULL,
  `PORT` int(11) NOT NULL,
  `PASSPHRASE` varchar(255) NOT NULL DEFAULT 'passw0rd',
  `GROUP_HANGTIME` int(11) NOT NULL DEFAULT 5,
  `USE_ACL` tinyint(1) NOT NULL DEFAULT 1,
  `REG_ACL` varchar(255) NOT NULL DEFAULT 'DENY:1',
  `SUB_ACL` varchar(255) NOT NULL DEFAULT 'DENY:1',
  `TGID_TS1_ACL` varchar(255) NOT NULL DEFAULT 'PERMIT:ALL',
  `TGID_TS2_ACL` varchar(255) NOT NULL DEFAULT 'PERMIT:ALL',
  `DEFAULT_UA_TIMER` int(11) NOT NULL DEFAULT 10,
  `SINGLE_MODE` tinyint(1) NOT NULL DEFAULT 1,
  `VOICE_IDENT` tinyint(1) NOT NULL DEFAULT 1,
  `TS1_STATIC` varchar(255) NOT NULL,
  `TS2_STATIC` varchar(255) NOT NULL,
  `DEFAULT_REFLECTOR` int(11) NOT NULL DEFAULT 0
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```
<b>Ahora agregamos el contenido a la tabla con los datos de los Hotspot.</b>

```terminal
INSERT INTO `repeaters` (`CALLSIGN`, `MODE`, `ENABLED`, `_REPEAT`, `MAX_PEERS`, `EXPORT_AMBE`, `IP`, `PORT`, `PASSPHRASE`, `GROUP_HANGTIME`, `USE_ACL`, `REG_ACL`, `SUB_ACL`, `TGID_TS1_ACL`, `TGID_TS2_ACL`, `DEFAULT_UA_TIMER`, `SINGLE_MODE`, `VOICE_IDENT`, `TS1_STATIC`, `TS2_STATIC`, `DEFAULT_REFLECTOR`) VALUES
('DMO-000', 'MASTER', 1, 1, 1, 0, '', 54000, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 0, '', '', 0),
('DMO-001', 'MASTER', 1, 1, 1, 0, '', 54001, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 0, '', '', 0),
('DMO-002', 'MASTER', 1, 1, 1, 0, '', 54002, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-003', 'MASTER', 1, 1, 1, 0, '', 54003, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-004', 'MASTER', 1, 1, 1, 0, '', 54004, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-005', 'MASTER', 1, 1, 1, 0, '', 54005, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-006', 'MASTER', 1, 1, 1, 0, '', 54006, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-007', 'MASTER', 1, 1, 1, 0, '', 54007, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-008', 'MASTER', 1, 1, 1, 0, '', 54008, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-009', 'MASTER', 1, 1, 1, 0, '', 54009, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-010', 'MASTER', 1, 1, 1, 0, '', 54010, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-011', 'MASTER', 1, 1, 1, 0, '', 54011, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-012', 'MASTER', 1, 1, 1, 0, '', 54012, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-013', 'MASTER', 1, 1, 1, 0, '', 54013, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-014', 'MASTER', 1, 1, 1, 0, '', 54014, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-015', 'MASTER', 1, 1, 1, 0, '', 54015, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-016', 'MASTER', 1, 1, 1, 0, '', 54016, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-017', 'MASTER', 1, 1, 1, 0, '', 54017, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-018', 'MASTER', 1, 1, 1, 0, '', 54018, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-019', 'MASTER', 1, 1, 1, 0, '', 54019, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-020', 'MASTER', 1, 1, 1, 0, '', 54020, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-021', 'MASTER', 1, 1, 1, 0, '', 54021, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-022', 'MASTER', 1, 1, 1, 0, '', 54022, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-023', 'MASTER', 1, 1, 1, 0, '', 54023, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-024', 'MASTER', 1, 1, 1, 0, '', 54024, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-025', 'MASTER', 1, 1, 1, 0, '', 54025, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-026', 'MASTER', 1, 1, 1, 0, '', 54026, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-027', 'MASTER', 1, 1, 1, 0, '', 54027, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-028', 'MASTER', 1, 1, 1, 0, '', 54028, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-029', 'MASTER', 1, 1, 1, 0, '', 54029, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-030', 'MASTER', 1, 1, 1, 0, '', 54030, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-031', 'MASTER', 1, 1, 1, 0, '', 54031, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-032', 'MASTER', 1, 1, 1, 0, '', 54032, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-033', 'MASTER', 1, 1, 1, 0, '', 54033, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-034', 'MASTER', 1, 1, 1, 0, '', 54034, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-035', 'MASTER', 1, 1, 1, 0, '', 54035, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-036', 'MASTER', 1, 1, 1, 0, '', 54036, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-037', 'MASTER', 1, 1, 1, 0, '', 54037, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-038', 'MASTER', 1, 1, 1, 0, '', 54038, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-039', 'MASTER', 1, 1, 1, 0, '', 54039, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-040', 'MASTER', 1, 1, 1, 0, '', 54040, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-041', 'MASTER', 1, 1, 1, 0, '', 54041, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-042', 'MASTER', 1, 1, 1, 0, '', 54042, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-043', 'MASTER', 1, 1, 1, 0, '', 54043, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-044', 'MASTER', 1, 1, 1, 0, '', 54044, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-045', 'MASTER', 1, 1, 1, 0, '', 54045, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-046', 'MASTER', 1, 1, 1, 0, '', 54046, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-047', 'MASTER', 1, 1, 1, 0, '', 54047, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-048', 'MASTER', 1, 1, 1, 0, '', 54048, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-049', 'MASTER', 1, 1, 1, 0, '', 54049, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-050', 'MASTER', 1, 1, 1, 0, '', 54050, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-051', 'MASTER', 1, 1, 1, 0, '', 54051, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-052', 'MASTER', 1, 1, 1, 0, '', 54052, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-053', 'MASTER', 1, 1, 1, 0, '', 54053, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-054', 'MASTER', 1, 1, 1, 0, '', 54054, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-055', 'MASTER', 1, 1, 1, 0, '', 54055, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-056', 'MASTER', 1, 1, 1, 0, '', 54056, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-057', 'MASTER', 1, 1, 1, 0, '', 54057, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-058', 'MASTER', 1, 1, 1, 0, '', 54058, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-059', 'MASTER', 1, 1, 1, 0, '', 54059, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-060', 'MASTER', 1, 1, 1, 0, '', 54060, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-061', 'MASTER', 1, 1, 1, 0, '', 54061, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-062', 'MASTER', 1, 1, 1, 0, '', 54062, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-063', 'MASTER', 1, 1, 1, 0, '', 54063, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-064', 'MASTER', 1, 1, 1, 0, '', 54064, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-065', 'MASTER', 1, 1, 1, 0, '', 54065, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-066', 'MASTER', 1, 1, 1, 0, '', 54066, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-067', 'MASTER', 1, 1, 1, 0, '', 54067, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-068', 'MASTER', 1, 1, 1, 0, '', 54068, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-069', 'MASTER', 1, 1, 1, 0, '', 54069, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-070', 'MASTER', 1, 1, 1, 0, '', 54070, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-071', 'MASTER', 1, 1, 1, 0, '', 54071, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-072', 'MASTER', 1, 1, 1, 0, '', 54072, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-073', 'MASTER', 1, 1, 1, 0, '', 54073, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-074', 'MASTER', 1, 1, 1, 0, '', 54074, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-075', 'MASTER', 1, 1, 1, 0, '', 54075, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-076', 'MASTER', 1, 1, 1, 0, '', 54076, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-077', 'MASTER', 1, 1, 1, 0, '', 54077, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-078', 'MASTER', 1, 1, 1, 0, '', 54078, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-079', 'MASTER', 1, 1, 1, 0, '', 54079, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-080', 'MASTER', 1, 1, 1, 0, '', 54080, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-081', 'MASTER', 1, 1, 1, 0, '', 54081, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-082', 'MASTER', 1, 1, 1, 0, '', 54082, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-083', 'MASTER', 1, 1, 1, 0, '', 54083, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-084', 'MASTER', 1, 1, 1, 0, '', 54084, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-085', 'MASTER', 1, 1, 1, 0, '', 54085, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-086', 'MASTER', 1, 1, 1, 0, '', 54086, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-087', 'MASTER', 1, 1, 1, 0, '', 54087, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-088', 'MASTER', 1, 1, 1, 0, '', 54088, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-089', 'MASTER', 1, 1, 1, 0, '', 54089, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-090', 'MASTER', 1, 1, 1, 0, '', 54090, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-091', 'MASTER', 1, 1, 1, 0, '', 54091, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-092', 'MASTER', 1, 1, 1, 0, '', 54092, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-093', 'MASTER', 1, 1, 1, 0, '', 54093, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-094', 'MASTER', 1, 1, 1, 0, '', 54094, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-095', 'MASTER', 1, 1, 1, 0, '', 54095, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-096', 'MASTER', 1, 1, 1, 0, '', 54096, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-097', 'MASTER', 1, 1, 1, 0, '', 54097, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-098', 'MASTER', 1, 1, 1, 0, '', 54098, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-099', 'MASTER', 1, 1, 1, 0, '', 54099, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0),
('DMO-100', 'MASTER', 1, 1, 1, 0, '', 54100, 'passw0rd', 5, 1, 'DENY:1', 'DENY:1', 'PERMIT:ALL', 'PERMIT:ALL', 15, 1, 1, '', '', 0);
```
<b>Y ahora si salimos de la base de datos.</b>
```terminal
quit
```
<b>Creamos y copiamos los siguientes archivos.</b>

```terminal
mkdir config

cp FreeDMR-SAMPLE.cfg config/FreeDMR.cfg

cp rules_SAMPLE.py config/rules.py

cp /opt/FreeDMR/systemd-scripts/freedmr.service /lib/systemd/system/
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







