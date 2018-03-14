# Introducción

Quizás se haya cansado de Microsoft Windows. Puede que ya esté aburrido de los innumerables virus de su sistema operativo. Quizás simplemente quiera conocer ese sistema del que todo el mundo habla, pero no se atreve a dar el salto.

Sea la razón por la que sea, puede que usted necesite convivir en un entorno en el que haya, o bien sistemas windows en la red, o mantenga un sistema con arranque dual y necesite acceder a sus particiones Windows.

No se preocupe, estamos aquí para ayudarle. En este capítulo aprenderá a montar particiones Windows en su sistema ParrotSec, así como a utilizar recursos compartidos entre sistemas Microsoft y su flamante ParrotSec OS.


# Montando particiones Windows en Parrot

Supongamos que tenemos un sistema con arranque dual. En un disco tengo mi sistema Parrot y en el otro Windows.

Primeramente, comprobemos con "fdisk" los discos y particiones de nuestro sistema. En mi caso utilizo el gestor de volúmenes lógico(LVM), con lo que puede que la salida de este comando no sea exactamente igual a la suya. También he modificado un poco la salida del comando, para que se vea más claro los diferentes discos y sus particiones:

	┌─[root@parrot]─[~]
	└──╼ #fdisk -l|grep sd
	----Disco sdb-----
	Disk /dev/sdb: 10 GiB, 10737418240 bytes, 20971520 sectors
	/dev/sdb1  *       63 20948759 20948697  10G  7 HPFS/NTFS/exFAT

	----Disco sdc-----
	Disk /dev/sdc: 8 GiB, 8589934592 bytes, 16777216 sectors
	/dev/sdc1          2048  4196351  4194304   2G 8e Linux LVM
	/dev/sdc2       4196352 16777215 12580864   6G 8e Linux LVM

	----Disco sda-----
	Disk /dev/sda: 30 GiB, 32212254720 bytes, 62914560 sectors
	/dev/sda1  *      2048   499711   497664  243M 83 Linux
	/dev/sda2       501758 62912511 62410754 29.8G  5 Extended
	/dev/sda5       501760 62912511 62410752 29.8G 8e Linux LVM


Podemos observar que mi sistema está compuesto por 3 discos (sda, sdb, y sdc).
En cada disco tenemos diferentes particiones. Para sdb, una partición llamada sdb1, para sdc dos particiones sdc1 y sdc2,.. Ahora mismo nos interesa ver las particiones (líneas) que contienen la palabra FAT o NTFS. Vemos que hay un disco que dispone de una partición con esa característica. El disco es el sdb y la partición la sdb1. Pués bien, esa es la partición Windows que deberemos montar en algún lugar de nuestro arbol de directorios.

Para poder trabajar con particiones del tipo NTFS necesitaremos tener instalado el paquete ntfs-3g. En mi caso ya estaba instalado:

	┌─[root@parrot]─[~]
	└──╼ #apt-get install ntfs-3g
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	ntfs-3g is already the newest version (1:2016.2.22AR.2-2).
	0 upgraded, 0 newly installed, 0 to remove and 2 not upgraded.

Ahora ya, tan sólo nos queda seleccionar un punto de montaje (la ruta en la que accederemos a nuestro dispositivo). En mi caso, seleccionaré /mnt. Usted seleccione otra ruta si lo prefiere. NOTA: Monte su dispositivo en un directorio vacío.

	┌─[root@parrot]─[~]
	└──╼ #mount /dev/sdb1 /mnt/


Comprobemos que se ha montado correctamente, bien con el comando "mount" o "df":

	┌─[root@parrot]─[~]
	└──╼ #mount|grep mnt
	/dev/sdb1 on /mnt type fuseblk (rw,relatime,user_id=0,group_id=0,allow_other,blksize=4096)


	┌─[root@parrot]─[~]
	└──╼ #df|grep mnt
	/dev/sdb1                    10474348  2667300   7807048  26% /mnt


Vemos que ambos comandos nos han devuelto una respuesta. Ya tan sólo tenemos que acceder a dicho directorio y trabajar con él. 

	┌─[root@parrot]─[~]
	└──╼ #cd /mnt/
	┌─[root@parrot]─[/mnt]
	└──╼ #ls -al
	total 1573213
	drwxrwxrwx  1 root root       4096 Oct  8 21:55  .
	drwxr-xr-x 22 root root       4096 Oct 17 21:31  ..
	drwxrwxrwx  1 root root       4096 Feb 15  2015  4fr33
	drwxrwxrwx  1 root root       4096 Oct  8 21:54 'Archivos de programa'
	-rwxrwxrwx  1 root root          0 Oct  9 04:42  AUTOEXEC.BAT
	-rwxrwxrwx  1 root root       4952 Aug 13  2004  Bootfont.bin
	-rwxrwxrwx  1 root root        211 Oct  9 04:39  boot.ini
	-rwxrwxrwx  1 root root          0 Oct  9 04:42  CONFIG.SYS
	drwxrwxrwx  1 root root       4096 Oct  9 04:43 'Documents and Settings'
	-rwxrwxrwx  1 root root          0 Oct  9 04:42  IO.SYS
	-rwxrwxrwx  1 root root          0 Oct  9 04:42  MSDOS.SYS
	-rwxrwxrwx  1 root root      47564 Apr 14  2008  NTDETECT.COM
	-rwxrwxrwx  1 root root     251168 Apr 14  2008  ntldr
	-rwxrwxrwx  1 root root 1610612736 Oct 21  2017  pagefile.sys
	-rwxrwxrwx  1 root root        802 Feb 17  2015  readme.txt
	drwxrwxrwx  1 root root       4096 Oct  9 04:43 'System Volume Information'
	drwxrwxrwx  1 root root      16384 Oct  8 21:52  WINDOW



NOTA: Está usted accediendo a la partición Windows. Tenga cuidado a la hora de borrar o eliminar archivos "vitales", ya que podrían dejar inutilizado su sistema Microsoft.

Para desmontarlo tan sólo debemos ejecutar "umount", situándonos fuera del punto de montaje:

	┌─[root@parrot]─[/mnt/Documents and Settings/josu/Escritorio]
	└──╼ #cd /
	┌─[root@parrot]─[/]
	└──╼ #umount /mnt

Para que al arrancar ParrotSec, esta partición se monte automáticamente, debemos editar el archivo "/etc/fstab" y añadir una línea al final como la siguiente:

	/dev/sdb1	/mnt	ntfs	defaults	0 0

Donde /dev/sdb1 es la partición y /mnt es el punto de montaje.




# Compartiendo recursos

## Desde windows a Parrot

Tenemos varias formas de compartir recursos entre sistemas windows y GNU/Linux. Una de ellas es mediante el protocolo SMB. Este protocolo es utilizado entre los sistemas Windows para compartir directorios. En GNU/Linux existe una implementación de dicho protocolo contenida en la suite SAMBA.


Veamos como compartimos un recurso desde windows a nuestra querida Parrot.

Primeramente, compartamos un recurso en un sistema Windows. La forma de hacer esto dependerá de la versión que utilicemos. Compruebe los manuales de su versión:

NOTA: Esto es un windows XP

![alt text](https://github.com/josegatica/parrot-docu-es/blob/master/images/migracion1.png "SMB")

![alt text](https://github.com/josegatica/parrot-docu-es/blob/master/images/migracion2.png "SMB")

![alt text](https://github.com/josegatica/parrot-docu-es/blob/master/images/migracion3.png "SMB")


Una vez compartido el recurso nos podremos conectar a él. En este caso, mi recurso compartido en windows XP tiene acceso total, por lo que puedo utilizar cualquier usuario con cualquier contraseña. La dirección ip de Windows XP, en mi caso, es 192.168.56.101. También podemos ver que el nombre del recurso que hemos compartido anteriormente se llama "shared".

Con smbclient podemos listar los recursos de un sistema:

	┌─[root@parrot]─[~]
	└──╼ #smbclient -L 192.168.56.101
	WARNING: The "syslog" option is deprecated
	Enter WORKGROUP\root's password: 
	OS=[Windows 5.1] Server=[Windows 2000 LAN Manager]

		Sharename       Type      Comment
		---------       ----      -------
		IPC$            IPC       IPC remota
		ADMIN$          Disk      Admin remota
		C$              Disk      Recurso predeterminado
		shared          Disk      
	OS=[Windows 5.1] Server=[Windows 2000 LAN Manager]

		Server               Comment
		---------            -------

		Workgroup            Master
		---------            -------

También con smbclient, podremos acceder a los recursos. Si usted conoce el comando ftp, no hará falta que expliquemos mucho más:

	┌─[root@parrot]─[/mnt]
	└──╼ #smbclient  //192.168.56.101/shared
	WARNING: The "syslog" option is deprecated
	Enter WORKGROUP\root's password: 
	OS=[Windows 5.1] Server=[Windows 2000 LAN Manager]
	smb: \> ls
  	.                                   D        0  Sat Oct 21 00:00:13 2017
  	..                                  D        0  Sat Oct 21 00:00:13 2017
  	Nuevo Archivo de sonido.wav         A       58  Sat Oct 21 00:00:13 2017
  	Nuevo Archivo WinRAR ZIP.zip        A       22  Fri Oct 20 23:59:58 2017
  	Nuevo Documento de texto.txt        A        0  Sat Oct 21 00:00:04 2017
  	Nuevo Imagen de mapa de bits.bmp      A        0  Fri Oct 20 23:59:53 2017

			2618587 blocks of size 4096. 1946923 blocks available
	smb: \> 

Con "ls" en el nuevo prompt de smbclient, hemos listado los 4 documentos de nuestro recurso compartido.



Si necesitamos acceder al recurso, tal y como lo haríamos a un recurso nfs (montándolo en nuestro arbol de directorios), necesitaremos el paquete "cifs-utils". Para ello ejecutamos "apt install cifs-utils":

	┌─[root@parrot]─[/root]
	└──╼ #apt install cifs-utils
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	Suggested packages:
	  keyutils winbind
	The following NEW packages will be installed:
	  cifs-utils
	0 upgraded, 1 newly installed, 0 to remove and 2 not upgraded.
	Need to get 75.8 kB of archives.
	After this operation, 236 kB of additional disk space will be used.
	Get:1 http://matojo.unizar.es/parrot parrot/main amd64 cifs-utils amd64 2:6.7-1 [75.8 kB]
	Fetched 75.8 kB in 0s (78.4 kB/s)   
	Selecting previously unselected package cifs-utils.
	(Reading database ... 490249 files and directories currently installed.)
	Preparing to unpack .../cifs-utils_2%3a6.7-1_amd64.deb ...
	Unpacking cifs-utils (2:6.7-1) ...
	Setting up cifs-utils (2:6.7-1) ...
	update-alternatives: using /usr/lib/x86_64-linux-gnu/cifs-utils/idmapwb.so to provide /etc/cifs-utils/idmap-plugin (idmap-plugin) in auto mode
	Processing triggers for man-db (2.7.6.1-2) ...

Una vez instalado, podremos montarlo en un punto de montaje de nuestro sistema:

	┌─[✗]─[root@parrot]─[~]
	└──╼ #mount -t cifs //192.168.56.101/shared /mnt -o user=guest,vers=1.0
	Password for guest@//192.168.56.101/shared:  
	─[root@parrot]─[~]
	└──╼ #cd /mnt/
	┌─[root@parrot]─[/mnt]
	└──╼ #ls -la
	total 5
	drwxr-xr-x  2 root root    0 Oct 21 00:00  .
	drwxr-xr-x 22 root root 4096 Oct 17 21:31  ..
	-rwxr-xr-x  1 root root   58 Oct 21 00:00 'Nuevo Archivo de sonido.wav'
	-rwxr-xr-x  1 root root   22 Oct 20 23:59 'Nuevo Archivo WinRAR ZIP.zip'
	-rwxr-xr-x  1 root root    0 Oct 21 00:00 'Nuevo Documento de texto.txt'
	-rwxr-xr-x  1 root root    0 Oct 20 23:59 'Nuevo Imagen de mapa de bits.bmp'


O incluso lo podremos configurar en el archivo "/etc/fstab", agregando la siguiente línea:

	//192.168.56.101/shared	/mnt	cifs	user=guest,vers=1.0   0 0


Donde, el primer campo es el servidor SMB junto con el nombre del recurso compartido, el segundo campo es nuestro punto de montaje, el tercero es el tipo de filesystem (en este caso CIFS, protocolo "similar" a SMB), y en sus opciones agregamos el usuario "guest" que utilizaremos para conectarnos y vers=1.0, ya que se trata de Windows XP y su versión de samba es esta. A continuación nos solicitará contraseña.

Si no queremos que solicite esta contraseña, y ya que nuestro recurso compartido es público, nuestra línea en /etc/fstab podría haber sido:

	//192.168.56.101/shared	/mnt	cifs	user=guest,password=,vers=1.0   0 0

Donde "password=" queda vacío.


	NOTA: si su versión de Windows es posterior a windows XP es más que probable que no deba indicar "vers=1.0".

Consulte la página de manual (man 8 mount.cifs) para más información.


## Desde Parrot a Windows

En esta parte, veremos cómo configurar nuestro sistema para ser accedido desde Windows. Para ello debemos instalar samba. Probablemente su sistema ParrotSec ya lo tenga instalado. 

	┌─[root@parrot]─[~]
	└──╼ #apt install samba

El fichero de configuración se encuentra bajo "/etc/samba". Añadiremos al final del fichero smb.conf, un recurso llamado "test", con una serie de opciones para que 
el recurso pueda ser accedido desde windows sin petición de usuario/contraseña y que se pueda escribir en dicho recurso (ATENCION!!!ESTO ES POTENCIALMENTE PELIGROSO SI LA RED NO ES SUYA O COMPARTE UNA RED CON MAS GENTE, YA QUE EL DIRECTORIO ES ESCRIBIBLE SIN CONTRASEÑA). Puede comprobar con "man 5 smb.conf", otras opciones.

Añadimos al final del fichero /etc/samba/smb.conf:

	[test]
	  comment = Test samba en ParrotSec
	  path = /srv/samba
	  browsable = yes
	  guest ok = yes
	  read only = no
	  create mask = 666
	  directory mask = 777
	  force user = test
	  force group = test

La variable path indica el directorio que queremos compartir.

Ejecutaremos "testparm" para comprobar que no hay errores en smb.conf:

	┌─[root@parrot]─[~]
	└──╼ #testparm 
	Load smb config files from /etc/samba/smb.conf
	rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
	WARNING: The "syslog" option is deprecated
	Processing section "[homes]"
	Processing section "[printers]"
	Processing section "[print$]"
	Processing section "[test]"
	Loaded services file OK.
	Server role: ROLE_STANDALONE

	Press enter to see a dump of your service definitions

	# Global parameters
	[global]
		log file = /var/log/samba/log.%m
		max log size = 1000
		syslog = 0
		panic action = /usr/share/samba/panic-action %d
		usershare allow guests = Yes
		map to guest = Bad User
		obey pam restrictions = Yes
		pam password change = Yes
		passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
		passwd program = /usr/bin/passwd %u
		server role = standalone server
		unix password sync = Yes
		dns proxy = No
		idmap config * : backend = tdb


	[homes]	
		comment = Home Directories
		browseable = No
		create mask = 0700
		directory mask = 0700
		valid users = %S


	[printers]
		comment = All Printers
		path = /var/spool/samba
		browseable = No
		printable = Yes
		create mask = 0700


	[print$]
		comment = Printer Drivers
		path = /var/lib/samba/printers


	[test]
		comment = Test samba en ParrotSec
		path = /srv/samba
		create mask = 0666
		directory mask = 0777
		force group = test
		force user = test
		guest ok = Yes
		read only = No


Ejecutamos smbpasswd, para añadir al usuario test a samba. Nos solicitará su contraseña samba. Para este ejemplo en concreto, no tiene importancia su contraseña ya que desde Windows no la pedirá.

	┌─[root@parrot]─[~]
	└──╼ #smbpasswd -a test
	New SMB password:
	Retype new SMB password:


Sólo queda comprobar que los permisos y propietario del directorio compartido son correctos:

	┌─[root@parrot]─[~]
	└──╼ #ls -lad /srv/samba
	drwxr-xr-x 2 test test 4096 Oct 22 22:02 /srv/samba

E iniciar el servicio samba:

	┌─[root@parrot]─[~]
	└──╼ #systemctl start smbd

Comprobemos que samba está en funcionamiento:

	┌─[root@parrot]─[~]
	└──╼ #systemctl status smbd
	● smbd.service - Samba SMB Daemon
	   Loaded: loaded (/lib/systemd/system/smbd.service; disabled; vendor preset: en
	   Active: active (running) since Sun 2017-10-22 22:00:58 CEST; 19min ago


Desde un sistema windows ya podría acceder a nuestro recurso, apuntando en un explorador, a la ip de nuestro servidor SMB. (\\ip_servidor_ParrotSec).



![alt text](https://github.com/josegatica/parrot-docu-es/blob/master/images/migracion4.png "SMB")

