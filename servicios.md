# Systemd

Vivimos una epoca en la que systemd se ha "apoderado" de la grn mayoría de sistemas linux. No queremos entrar en un debate de si systemd es bueno o malo.

Los desarrolladores de Parrot lo tienen claro: https://blog.parrotsec.org/debian-and-devuan/ 

De todas formas en caso de cambiar algo, será en un futuro. Actualmente el sistema cuenta con systemd, tal y  como tiene su hermana mayor Debian.
 
Aun así los desarrolladores han convertido los scripts service para que hagan un "hook" a systemd, y así podamos seguir utilizando nuestro querido "service" y "update-rc.d".

De todas formas hoy hablaremos de systemd y si se producen cambios en la distribución actualizaremos este documento.





## Introducción a systemd

El arranque del sistema y procesos son manejados por systemd. Este programa nos ofrece una forma para activar recursos, demonios y otros procesos, tanto en el arranque del sistema como durante su ejecución.
Los Demonios son procesos que esperan o corren en segundo plano realizando diversas tareas. Para recibir una conexión, el demonio utiliza un socket. Este socket puede ser creado por el propio demonio o puede ser separado de él y creado por otro proceso, como puede ser systemd, el cual aplica dicho socket al demonio cuando se establece una conexión desde un cliente.

Un servicio normalmente se refiere a uno o más demonios, pero arrancando o parando un servicio puede realizar un cambio al estado del sistema (por ejemplo la configuración de las interfaces de red), que no necesariamente dejará un proceso demonio corriendo.

Durante muchos años, el proceso ID 1 de Linux y sistemas Unix ha sido el proceso "init". Este proceso era el responsable de activar otros servicios en nuestros sistemas.

Los demonios usuados frecuentemente eran arrancados al inicio del sistema con "system V y scipts de inicio LSB". Menos frecuentemente los demonios se arrancaban bajo demanda como inetd o xinetd.

El sistema "System V", que como se ha dicho llebvaba con nosotros muchos (demasiados?) años tenia una serie de limitaciones. Es por esto que han surgido diferentes sistemas de arranque para intentar solucionar esto. Debian (y la gran mayoría de distribuciones) han escogido como método de arranque "systemd".


## systemctl y unidades systemd

El comando systemctl se utiliza para manejar diferentes tipos de objetos de systemd, llamados "units" (unidades).

Podemos ver una lista de los diferentes tipos de unidades que maneja systemd podemos utilizar la instrucción "systemctl -t help".

	┌─[root@parrot]─[~]
	└──╼ #systemctl -t help
	Available unit types:
	service
	socket
	busname
	target
	device
	mount
	automount
	swap
	timer
	path
	slice
	scope


Algunas unidades son:

- Unidades "service". Tienen una extensión ".service" y representan los servicios del sistema. Este tipo de unidad son usados para arrancar demonios accedidos frecuentemente, como puede ser un servidor web.
- Unidades "socket". Tienen la extensión ".socket" y representan la comunicación entre procesos (IPC).
- Unidades "path". Tienen la extensión ".path" y se utilizan para retrasar la activación de un servicio hasta que el Filesystem este activo.

Puede comprobar todas las unidades que hay en su sistema con la instrucción "systemctl list-unit-files". Compruebe como cada unidad tiene una extensión que nos indica que tipo de objeto es.

## Estados de Servicio

El estado de un servicio puede ser observado mediante "systemctl status nombre.tipo". Si no se indica el tipo de unidad, systemd devolverá el estado del servicio si es que existe alguno con ese nombre.


	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd.service
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-09 15:39:51 CEST; 6 days ago
	 Main PID: 1135 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─1135 /usr/sbin/sshd -D

	Oct 16 10:37:10 parrot systemd[1]: Reloading OpenBSD Secure Shell server.
	Oct 16 10:37:10 parrot sshd[1135]: Received SIGHUP; restarting.
	Oct 16 10:37:10 parrot systemd[1]: Reloaded OpenBSD Secure Shell server.
	Oct 16 10:37:10 parrot sshd[1135]: Server listening on 0.0.0.0 port 22.
	Oct 16 10:37:10 parrot sshd[1135]: Server listening on :: port 22.
	Oct 16 10:37:10 parrot systemd[1]: Reloading OpenBSD Secure Shell server.
	Oct 16 10:37:10 parrot sshd[1135]: Received SIGHUP; restarting.
	Oct 16 10:37:10 parrot systemd[1]: Reloaded OpenBSD Secure Shell server.
	Oct 16 10:37:10 parrot sshd[1135]: Server listening on 0.0.0.0 port 22.
	Oct 16 10:37:10 parrot sshd[1135]: Server listening on :: port 22.



	┌─[root@parrot]─[~]
	└──╼ #systemctl status apache2
	● apache2.service - The Apache HTTP Server
	   Loaded: loaded (/lib/systemd/system/apache2.service; disabled; vendor preset: enabled)
	   Active: inactive (dead)


Podemos observar la salida de dos servicios. Uno sshd corriendo y otro apache2 parado.


Varias palabras claves podemos observar en la salida de estado de un servicio:

	Palabra			Descripción
	------------------------------------------------------------------------------------------------------------
	loaded			El fichero de configuración de la unidad ha sido porcesada
	active(runnig)		Arrancado con uno o mas procesos ejecutándose
	active(exited)		Completada correctamente la configuración
	active(waiting)		Ejecutádose pero esperando un evento
	inactive		No ejecutandose
	enabled			Se ejecutará en el arranque del sistema
	disabled		No se ejecutará en el arranque del sistema
	static			No puede ser activado, pero puede ser iniciado por una unidad activa automáticamente



## Listando unidades con systemctl
Consulta el estado de todas las unidades   

	┌─[root@parrot]─[~]
	└──╼ #systemctl 

Consulta el estado de los servicios arrancados

	┌─[root@parrot]─[~]
	└──╼ #systemctl --type=service

Consulta el estado de un servicio

	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd 

Aunque si observamos la salida de la opción "status", podemos llegar a saber si un servicio debe o no arrancarse en el inicio y si está activo, tenemos también varias opciones para verlo más facilmente:

	┌─[root@parrot]─[~]
	└──╼ #systemctl is-active apache2
	inactive
	┌─[✗]─[root@parrot]─[~]
	└──╼ #systemctl is-enabled apache2
	disabled
	┌─[✗]─[root@parrot]─[~]
	└──╼ #systemctl is-enabled sshd
	enabled

Comprobar servicios fallidos

	┌─[✗]─[root@parrot]─[~]
	└──╼ #systemctl --failed --type=service

## Arrancando y parando demonios del sistema
Arrancar, parar, recargar y verificar el estado son tareas comunes cuando administramos un sistema.

Para ver el estado de un servicio:

	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-09 15:39:51 CEST; 6 days ago
	 Main PID: 1135 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─1135 /usr/sbin/sshd -D


Para comprobar que el proceso está corriendo:

	┌─[root@parrot]─[~]
	└──╼ #ps -up 1135
	USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
	root      1135  0.0  0.0  71972  5440 ?        Ss   Oct09   0:00 /usr/sbin/sshd -D

Parar el servicio y verificar su estado:

	┌─[root@parrot]─[~]
	└──╼ #systemctl stop sshd
	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: inactive (dead) since Mon 2017-10-16 12:16:30 CEST; 5s ago
	  Process: 1135 ExecStart=/usr/sbin/sshd -D $SSHD_OPTS (code=exited, status=0/SUCCESS)
	 Main PID: 1135 (code=exited, status=0/SUCCESS)

Arrancar el servicio y ver el estado. El ID del proceso ha cambiado:

	┌─[✗]─[root@parrot]─[~]
	└──╼ #systemctl start sshd
	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-16 12:18:14 CEST; 3s ago
	  Process: 5222 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
 	Main PID: 5223 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─5223 /usr/sbin/sshd -D


Rearrancar el servicio y comprobar su estado:

	┌─[root@parrot]─[~]
	└──╼ #systemctl restart sshd
	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-16 12:19:04 CEST; 2s ago
	  Process: 5230 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
	 Main PID: 5231 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─5231 /usr/sbin/sshd -D

Recargar un servicio sin llegar a pararlo, por ejemplo para que lea un cambio en su configuración. En este caso el ID de proceso no cambiará.

	┌─[root@parrot]─[~]
	└──╼ #systemctl reload sshd
	┌─[root@parrot]─[~]
	└──╼ #systemctl status sshd
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-16 12:19:04 CEST; 2min 8s ago
	  Process: 5241 ExecReload=/bin/kill -HUP $MAINPID (code=exited, status=0/SUCCESS)
	  Process: 5240 ExecReload=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
	  Process: 5230 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
	 Main PID: 5231 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─5231 /usr/sbin/sshd -D


## Habilitando y deshabilitando demonios del sistema en el arranque

Los servicios se arrancan al inicio del sistema cuando se crean links en los directorios correctos de systemd. Estos links se pueden crear y/o borrar con los siguientes comandos de systemctl.

Deshabilitar un servicio en el arranque y comprobar su estado:

	┌─[root@parrot]─[~]
	└──╼ #systemctl disable ssh
	Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
	Executing: /lib/systemd/systemd-sysv-install disable ssh
	insserv: warning: current start runlevel(s) (empty) of script `ssh' overrides LSB defaults (2 3 4 5).
	insserv: warning: current stop runlevel(s) (2 3 4 5) of script `ssh' overrides LSB defaults (empty).
	Removed /etc/systemd/system/sshd.service.
	┌─[root@parrot]─[~]
	└──╼ #systemctl status ssh
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; disabled; vendor preset: enabled)
	   Active: active (running) since Mon 2017-10-16 12:19:04 CEST; 7min ago
	 Main PID: 5231 (sshd)
	    Tasks: 1 (limit: 4915)
	   CGroup: /system.slice/ssh.service
	           └─5231 /usr/sbin/sshd -D

Habilitar un servicio en el arranque y comprobar su estado:

	┌─[root@parrot]─[~]
	└──╼ #systemctl enable ssh
	Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
	Executing: /lib/systemd/systemd-sysv-install enable ssh
	insserv: warning: current start runlevel(s) (empty) of script `ssh' overrides LSB defaults (2 3 4 5).
	insserv: warning: current stop runlevel(s) (2 3 4 5) of script `ssh' overrides LSB defaults (empty).
	Created symlink /etc/systemd/system/sshd.service → /lib/systemd/system/ssh.service.
	┌─[root@parrot]─[~]
	└──╼ #systemctl status ssh
	● ssh.service - OpenBSD Secure Shell server
	   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
	  Active: active (running) since Mon 2017-10-16 12:19:04 CEST; 8min ago
	Main PID: 5231 (sshd)
	   Tasks: 1 (limit: 4915)
	  CGroup: /system.slice/ssh.service
	          └─5231 /usr/sbin/sshd -D


## Resumen de comandos systemctl

	Comando:			Tarea:
	-----------------------------------------------------------------------
	systemctl status UNIT		Ver detalles de una unidad
	systemctl stop UNIT		Para un servicio
	systemctl start UNIT		Arranca un servicio
	systemctl restart UNIT		Reinicia un servicio
	systemctl reload UNIT		Recarga uyn servicio
	systemctl enable UNIT		Habilita un servicio en el arranque
	systemctl disable UNIT		Deshabilita un servicio en el arranque

## Referencias

Páginas man init(1), systemd(1), systemctl(1).


Existen muchas páginas man en nuestro sistema. En este caso la salida de apropos es abrumadora:

	┌─[root@parrot]─[~]
	└──╼ #apropos systemd
	30-systemd-environment-d-generator (8) - Load variables specified by environment.d
	deb-systemd-helper (1p) - subset of systemctl for machines not running systemd
	deb-systemd-invoke (1p) - wrapper around systemctl, respecting policy-rc.d
	init (1)             - systemd system and service manager
	journalctl (1)       - Query the systemd journal
	loginctl (1)         - Control the systemd login manager
	lvm2-activation-generator (8) - generator for systemd units to activate LVM2 volumes on boot
	pam_systemd (8)      - Register user sessions in the systemd login manager
	systemctl (1)        - Control the systemd system and service manager
	systemd (1)          - systemd system and service manager
	...
	...
	...
