# Systemd

Vivimos una epoca en la que systemd se ha "apoderado" de la grn mayoría de sistemas linux. No queremos entrar en un debate si systemd es bueno o malo.

Los desarrolladores de Parrot lo tienen claro: https://blog.parrotsec.org/debian-and-devuan/ 

De todas formas en caso de cambiar algo será en un futuro. Actualmente el sistema cuenta con systemd, tal y  como está en su hermana mayor Debian.
 
Aun así los desarrolladores aun convertido los scripts service para que hagan un "hook" a systemd, y así podamos seguir utilizando nuestro querido "service" y "update-rc.d".

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



