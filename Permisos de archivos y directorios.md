## Permisos de Archivos y Directorios

Anteriormente mencionamos que en Linux, todos los archivos del sistema pertenecen a un usuario y un grupo. El dueño de un archivo es el usuario que lo ha creado y el grupo principal de este archivo es el grupo del usuario que lo creo. Por ejemplo, en capitulos anteriores trabajamos con el usuario "parrot", si este usuario crea un archivo, el usuario "parrot" y el grupo por defecto del usuario parrot, van a ser los propietarios de este nuevo archivo, o sea que el archivo pertenece al usuario parro y al grupo por defecto del usuario parrot. Por ello a menudo necesitamos hacer uso del comando "sudo" para poder leer, modificar o ejecutar algunos archivos y programas del sistema o realizar cambios en los permisos de los archivos en cuetión.

Vamos a analizar la salida del comando "ls -l"

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ # ls -l archivo.txt 
	-rw-rw-r-- 1 parrot hackers    0    oct 16 12:32 archivo.txt
	drwxr-xr-x 3 parrot hackers  4096   oct 15 16:25 scripts

La salida del comando "ls -l" nos indica si es un archivo (-) o directorio (d), los permisos del archivo/directorio (rw-rw-r--), el siguiente campo (indica el número de ficheros/directorios) usuario y grupo al que pertenece (parrot hackers), tamaño (0), última fecha de modificación (oct 16 12:32) y nombre (archivo.txt y scripts). Vamos a detenernos en los campos, permisos, usuario y grupo, vamos a centrarnos en el primer campo (permisos del archivo). En Linux, la gestión de los permisos que los usuarios y los grupos de usuarios tienen sobre los archivos y las carpetas, se realiza mediante un sencillo esquema de tres tipos de permisos:


- Permiso de lectura, representado por la letra r.

- Permiso de escritura, representado por la letra w.

- Permiso de ejecución, representado por la letra x.


El significado de estos permisos es diferente para archivos y para carpetas, a continuación vamos a explicar cada uno de los casos.


En el caso de archivo.txt, tiene permisos de lectura para:

	Propietario	Grupo	Resto de usuarios
	r w -           r w -	r - -

Esto quiere decir que todos los usuarios del sistema tienen permisos para leer este archivo, pero solo el propietario del archivo y los usuarios que sean miembros del grupo propietario podran realizar modificaciones en este archivo.

Para calcular el valor de un permiso nos basaremos en la suma de sus valores decimales según la siguiente correspondencia:
   
	 --------------------------
	|Permiso	|r | w | x |
	|---------------|--|---|---|
	|Valor decimal	|4 | 2 | 1 |
	 --------------------------

O sea, el valor decimal para el permiso de lectura es 4, el valor decimal para permiso de escritura es 2 y el valor decimal para permiso de ejecución es 1. Por lo tanto los posibles valores para un permiso son los siguientes:

   	 -----------------
  	|Permisos | Valor |
  	|-----------------|
  	|  rwx    |   7   |
  	|---------|-------|
 	|  rw-    |   6   |
 	|---------|-------|
 	|  r-x    |   5   |
 	|---------|-------|
 	|  r--    |   4   |
 	|---------|-------|
 	|  -wx    |   3   |
 	|---------|-------|
 	|  -w-    |   2   |
 	|---------|-------|
 	|  --r    |   1   |
 	|---------|-------|
 	|  ---    |   0   |
 	 -----------------

Por lo tanto llegamos a la siguiente conclusion:
 
  	 ------------------------
  	| Permiso     |   Valor  |
  	|-------------|----------|
  	| rwx rwx rwx |   777    |
  	|-------------|----------|
  	| rwx r-x r-- |   754    |
  	|-------------|----------|
  	| r-x r-- --- |   540    |
  	 ------------------------

Teniendo claro esto, podemos pasar al uso de "chmod", el cual nos sirbe para administrar los permisos de archivos y carpetas.

## Uso de chmod

Sintaxis básica de chmod:

	$ chmod [modo] [permisos] [fichero o directorio]

Como modo, vamos a usar solamente la obción -R, este parametro, indica a chmod que se van a cambiar los permisos de modo recursivo, útil para cambiar los permisos de los archivos de un directorio. Veamos un ejemplo:

Tenemos esta carpeta de scripts, en la cual no todos los scripts tienen permisos de ejecución

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rw-r--r-- 1 parrot hackers  932 oct 18 01:06 ddos-detect.py
	-rwxr-xr-x 1 parrot hackers  235 oct 18 01:06 ping.sh
	-rwxr-xr-x 1 parrot hackers  780 oct 18 01:17 wireless-dos-ids.py
	-rw-r--r-- 1 parrot hackers 1587 oct 18 01:05 wireless-dos.py

Como se puede apreciar en a ejecución de "ls -l scripts/", algunos scripts tienen permisos de ejecución para todos los usurios del sistema (lo cual no es recomendable), mientras que otros no tienen permisos de ejección ni siquiera para el usuario propietario. Para corregir este error aplicamos los siguientes permisos:

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #chmod -R 770 scripts/
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrwx--- 1 parrot hackers  932 oct 18 01:06 ddos-detect.py
	-rwxrwx--- 1 parrot hackers  235 oct 18 01:06 ping.sh
	-rwxrwx--- 1 parrot hackers  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrwx--- 1 parrot hackers 1587 oct 18 01:05 wireless-dos.py

Ahora el usuario porpietario y los usuarios miembros del grupo propietario tienen permisos de lectura, escritura y ejecución, miemtras que los demas usuarios del sistema no tiene acceso a estos archivos.

Otra manera de añadir o quitar permisos, es utilizando estos modos:

- a --> indica que se aplicaran a todos

- u --> indica que se aplicara al usuario

- g --> indica que se aplicara al grupo

- o --> indica que se aplicara a otros

- + --> indica que se añade el permiso

- - --> indica que se quita el permiso

- r --> indica permiso de lectura

- w --> indica permiso de escritura

- x --> indica permiso de ejecución


La sintaxis básica para utilizar "chmod" con estos todos es la siguiente:

	# chmod [a|u|g|o] [+|-] [r|w|x]

O sea a quien se le aplica el permiso, añadir o quitar permiso y tipo de permiso que se va a añadir o quitar.

Estas serian posibles combinaciones:

- a+r Permisos de lectura para todos.

- +r Igual que antes, si no se indica nada se supone ‘a’.

- og-x Quita permiso de ejecución a todos menos al usuario.

- u+rwx Da todos los permisos al usuario.

- o-rwx Quita los permisos a los otros.

Ejemplo de uso:

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #chmod -R og-x scripts/
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 parrot hackers  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 parrot hackers  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 parrot hackers  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 parrot hackers 1587 oct 18 01:05 wireless-dos.py

Si analizamos el resultado de la ejecución anterior, podemos notar como se han eliminado los permisos de ejecución para todos los usuarios del sistema, incluyendo los miembros del grupo propietario, excepto el usuario propietario, el cual conserva los permisos de lectura, escritura y ejecución.




## Uso del comando chown

Chown (change owner) es otra utilidad del sistema que nos permite realizar cambios en la propiedad de los archivos, se parece a "chmod" pero la función que raliza es distinta. Como su nombre lo indica es para cambiar el propietario de un archivo o carpeta.

Su sintáxis básica es la siguiente:

	$ chown [opciones] [propietario]:[grupo (obcional)] [archivos o directorios]

Opciones de chown:

- -R --> De manera recursiva cambia el propietario de los directorios junto con todos sus contenidos.

- -v o --verbose --> Se utiliza para mostrar una salida más descriptiva.

- --version --> Ver el número de versión del programa.

- -dereference --> Actúa sobre enlaces simbólicos en lugar de hacerlo sobre el destino.

- -h o --no-dereference --> En el caso de enlaces simbólicos, cambia el propietario del destino en lugar del propio enlace.

- --reference --> Cambia el propietario de un archivo, tomando como referencia el propietario del otro.

Ejemplos de uso:

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 parrot parrot  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 parrot parrot  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 parrot parrot  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 parrot parrot 1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #chown -R root:root scripts/
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 root root  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 root root  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 root root  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 root root 1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #

En el ejemplo anterior, podemos observar como ha cambiado el usuario y grupo propietario de todos los archivos que se encuentran en el directorio scripts. Veamos un ejempo en el que solo vamos a cambiar el usuario propietario.

	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 root root  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 root root  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 root root  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 root root 1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #chown -R parrot scripts/
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 parrot root  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 parrot root  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 parrot root  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 parrot root 1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #

En el ejemplo anterior, se puede apreciar como el usuario propietario de todos los archivos dentro del directorio scripts cambio a parrot.



## Uso del comando chgrp

El comando chgrp, se utiliza para cambiar el grupo al cual pertenece un archivo o directorio. Su sntaxis básica es la siguiente:

	$ chgrp [opciones] [archivo(s)] o [directorio(s)]

Opciones.

- -R --> De manera recursiva cambia el grupo al cual pertenecen los directorios junto con todos sus contenidos.

- -v (o --verbose) --> Se utiliza para mostrar una salida más descriptiva.

- --version --> Ver el número de versión del programa.

- --dereference	--> Actúa sobre enlaces simbólicos en lugar de hacerlo sobre el destino.

- -h (o --no-dereference) --> En el caso de enlaces simbólicos cambia el grupo del destino en lugar del propio enlace.

- --reference --> Cambia el grupo de un archivo tomando como referencia el propietario de otro.

Practicamente son las mismas obciones de "chown", con la diferencia de que "chgrp" solo cambia el grupo propietario de archivos y/o directorios, conservando el usuario propietario.



- Ejemplo de uso de chgrp:

		┌─[root@parrot-armhf]─[/home/parrot]
		└──╼ #ls -l scripts/
		total 16
		-rwxrw---- 1 parrot root  932 oct 18 01:06 ddos-detect.py
		-rwxrw---- 1 parrot root  235 oct 18 01:06 ping.sh
		-rwxrw---- 1 parrot root  780 oct 18 01:17 wireless-dos-ids.py
		-rwxrw---- 1 parrot root 1587 oct 18 01:05 wireless-dos.py
		┌─[root@parrot-armhf]─[/home/parrot]
		└──╼ #chgrp -R parrot scripts/
		┌─[root@parrot-armhf]─[/home/parrot]
		└──╼ #ls -l scripts/
		total 16
		-rwxrw---- 1 parrot parrot  932 oct 18 01:06 ddos-detect.py
		-rwxrw---- 1 parrot parrot  235 oct 18 01:06 ping.sh
		-rwxrw---- 1 parrot parrot  780 oct 18 01:17 wireless-dos-ids.py
		-rwxrw---- 1 parrot parrot 1587 oct 18 01:05 wireless-dos.py
		┌─[root@parrot-armhf]─[/home/parrot]
		└──╼ #

En el ejemplo anterior, se puede apreciar como el grupo propietario de todos los archivos dentro del directorio scripts cambio a parrot.


	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 parrot parrot  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 parrot parrot  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 parrot parrot  780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 parrot parrot 1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #chgrp root scripts/wireless-dos.py scripts/wireless-dos-ids.py 
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #ls -l scripts/
	total 16
	-rwxrw---- 1 parrot parrot  932 oct 18 01:06 ddos-detect.py
	-rwxrw---- 1 parrot parrot  235 oct 18 01:06 ping.sh
	-rwxrw---- 1 parrot root    780 oct 18 01:17 wireless-dos-ids.py
	-rwxrw---- 1 parrot root   1587 oct 18 01:05 wireless-dos.py
	┌─[root@parrot-armhf]─[/home/parrot]
	└──╼ #

En el ejemplo anterior, se puede apreciar como el grupo propietario de los archivos wireless-dos-ids.py y wireless-dos.py cambio de parrot a root.
