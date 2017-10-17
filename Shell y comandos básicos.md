# Shell y Comandos Básicos de Linux:

Todo usuario nuevo, antes de hacer cualquier otra cosa en Linux, debe entender como funciona la Shell y como hacer uso de esta en Linux. La Shell nos permite ejecutar comandos para realizar casi cualquier función en el sistema, es la forma que tenemos de hablar directamente con el sistema operativo sin necesidad de utilizar la GUI (Interfaz Gráfica de Usuario).

Un exelente comando para comezar a utilizar la shell es "uname", este nos muestra que sistema operativo estamos usando.

	$ uname
	Linux


Haciendo uso de la opción -a podemos encontrar información adicional.

	$uname -a
	Linux parrot 4.11.0-parrot6-amd64 #1 SMP Parrot 4.11.6-1parrot6 (2017-06-28) x86_64 GNU/Linux


La opción "-a" nos aporta más información, incluyendo la versión actual del Kernel Linux que está sienso usado, el hostname, la arquitectura del sistema. El comando uname -a es un exelente comando para comenzar a trabajar con la Shell de Linux, ya que nos permite obtener información sobre el sistema operativo que estamos usando. 

La Shell nos permite ejecutar comandos Internos y Externos, es importante diferenciar cada uno de estos dos tipos.

Los comandos Internos son aquellos que están integrados en la Shell (Built into shell), estos comandos internos nos permiten realizar tareas comunes como:


Mostrar el directorio actual:

	$ pwd


Cambiar de directorio:

	$ cd /ruta/del/nuevo/directorio


Mostrar un texto en pantalla:

	$ echo "Texto que queremos mostrar"


Tiempo de ejecución de un comando:

	$ time pwd		# Esto nos indica el tiempo de ejecución del comando pwd


Establecer obciones:

	$ set --help


El comando set muestra una gran variedad de obciones relacionadas con las operaciones de la shell. Estas obciones son muy similares a las variables de entorno pero no son la misma cosa.


Cerrar la Shell:

	$ exit


Los comandos exit y logout terminan la shell. El comando exit, termina cualquier shell, mientras que el comando logout sólo termina las shell de inicio de sesión. Estas shell de inicio de sesión son aquellas que se inician en el inicio de una sesión en modo texto.

	$ logout


Se puede comprobar facilmente cuando un comando es Interno o Externo utilizando el comando "type" antes del comando que queremos comprobar.

	$ type cd

	cd is a shell builtin

	$ type bash

	bash is /bin/bash


Algunos comandos internos estan duplicados por comandos externos que hacen exactamente la misma función, estos comandos externos no siempre estan instalados en todos los sistemas. Podemos chequear cuales de estos comandos internos estan duplicados por comandos externos usando la obción "-a" al ejecutar el comando "type".

	$ type -a pwd

	pwd is a shell builtin

	pwd is /bin/pwd


En la ejecución anterior podemos ver como exite una instalación externa del comando pwd en ParrotSec. Es importante mencionar que cuando un comando externo esta instalado, el comando interno tiene prioridad. Si queremos ejecutar el comando externo en lugar del interno, debemos especificar el path del comando externo, ejemplo:

	$ /bin/pwd

	/home/user


Estos son algunos conceptos básicos que deben aprender antes de profundizar en el uso de la Shell y de comandos en GNU/Linux. Es importante tener conocimiento no solo de las operaciones que se pueden realizar con un comando sino también de su origen y de como funciona este.

## Uso de su y sudo

Los comandos su y sudo a menudo suelen ser confundidos, digamos que tienen una pequeña relación ya que ambos, de una forma u otra son para escalar privilegios en un sistema Linux o para ejecutar comandos como super usuario, pero en realidad son muy distintos, realizan funciones distintas y el uso de ambos es totalmente distinto. En este capitulo vamos a explicar el uso de cada uno de ellos.

## Comando su

El comando su es al acronimo para Switch User (Cambio de Usuario), como su nombre nos indica, este comando nos permite cambiar de usuario sin necesidad de cerrar sesión e iniciar sesión nuevamente con el usuario al que queremos cambiar. Para explicar el uso del comando su, vamos a usar "whoami" (este comando nos muestra el usuario actual con el que estamos trabajando en el sistema) y pwd que lo vimos en ejemplos anteriores.

	$ whoami
	user
	$ pwd
	/home/user


Como pueden ver estos comandos nos muestras que somos el usuario "user" y estamos en el directorio "/home/user", ahora si vamos a pasar al uso del comando "su".

Para cambiar al usuario "parrot" u otro usuario del sistema ejecutamos su seguido del nombre de usuario al que queremos cambiar:

	$ su parrot
	contraseña: <contraseña del usuario parrot>
	$ whoami
	parrot


como pueden ver hemos utilizado el comando "su" para cambiar hacia el usuario "parrot" sin necesidad de cerrar sesión, a partir de ahora todas las operaciones que realizamos en el sistema seran con los permisos del usuario "parrot". Parra regresar hacia nuestro usuario normal, basta con ejecutar el comando "exit".

	$ su parrot
	contraseña: <contraseña del usuario parrot>
	$ whoami
	parrot
	$ exit
	$ whoami
	user


Tambien podemos usar el comando "su" para cambiar hacia el usuario "root", no es necesario especificar el usuario "root" ya que si no se especifica ningún usuario, "su" toma por defecto al usuario "root".


	$ su
	contraseña: <contraseña del usuairo root>
	# whoami
	root
	# exit
	$ whoami
	user

notese como el prompt cambio el simbolo de $ por el simbolo #, esto se debe a que el simbolo $ representa a los usuarios normales del sistema y el simbolo de # representa al super usuario (root). Al igual para volver hacia nuestro usuario ejecutamos el comando exit.

En caso de querer cambiar hacia un usuario y a la vez cambiar hacia su carpeta personal y demás variables de entorno, agregamos un simbolo de menos (-) entre el comando su y el nombre del usuario, ejemplo:

	$ whoami
	user
	$ pwd
	/home/user
	$ su - parrot
	contraseña:
	$ whoami
	parrot
	$ pwd
	/home/parrot

Para un mayor entendimiento del comando su, le invitamos a chequear el manual, ejecutando desde el terminal:

	$ man su

Para salir del manual, presione la tecla "q".


## Comando sudo

Sudo es el acronimo para "Switch User DO" (Cambiar de Usuario y Hacer...), este comando nos permite cambiar al usuario root de una forma imperceptible y ejecutar comandos o acciones con los privilegios del usuario root de manera totalmente segura. En gran parte de las distribuciones Linux, tenemos el comando sudo instalado por defecto. Este comando no puede ser usado por todos los usuarios del sistema, existe un grupo llamado "sudoers users", los usuarios que pertenecen a este grupo son los unicos que estan autorizados a hacer uso de este comando, por lo general solo se suele configurar para usuarios administradores del sistema. El archivo de configuración se encuentra en /etc/sudoers.

Contenido del archivo sudoers en Parrot:

	# This file MUST be edited with the 'visudo' command as root.
	#
	# Please consider adding local content in /etc/sudoers.d/ instead of
	# directly modifying this file.
	#
	# See the man page for details on how to write a sudoers file.
	#
	Defaults	env_reset
	Defaults	mail_badpass
	Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
	# Host alias specification
	# User alias specification
	# Cmnd alias specification
	# User privilege specification
	root	ALL=(ALL:ALL) ALL
	# Allow members of group sudo to execute any command
	%sudo	ALL=(ALL:ALL) ALL
	# See sudoers(5) for more information on "#include" directives:
	#includedir /etc/sudoers.d


¿Qué quiere decir todo esto? 

Exlicado de forma sencilla:
 
- Todas las lineas precedidas por el simbolo de # son comentarios que indican cada sección del archivo de configuración, estos comentarios son ignorados por el sistema.

- La sección " # User privilege specification" la cual contiene "root	ALL=(ALL:ALL) ALL", nos indica que el usuario "root" tiene permisos para usar el comando sudo y editar la configuración del archivo "sudoers". En caso de querer agragar otro usuario a "sudoers", podemos agregar una linea similar a la del usuario "root" con el nombre del usuario deseado, ejemplo: "parrot	ALL=(ALL:ALL) ALL". En caso de que el usuario "parrot no este en el grupo "sudoers users" o "sudo", esta nueva linea le permitiria hacer uso del comando "sudo".

- La sección "# Allow members of group sudo to execute any command" que contiene "%sudo	ALL=(ALL:ALL) ALL", indica que los miembros del grupo "sudo" tienen permisos para usar este comando, modificar la configuracion de sudoers, etc. O sea que esta sección nos permite agregar a todos los miembros de un grupo especificado para que tengan permisos de usar el comando sudo. Ejemplo: "%administradores	ALL=(ALL:ALL) ALL", esta linea permite a todos los miembros del grupo "administradores" a usar el comando sudo, etc...

## Ejemplos de uso del comando sudo

En caso de querer instalar alguna aplicación desde los repositorios, o realizar alguna otra tarea que necesite permisos administrativos, nos encontramos con el siguiente error.

$ apt-get install chromium
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?

Nos dice que el permiso ha sido denegado y nos pregunta que si somos root, en cambio si usamos el comando sudo por delante:

	$ sudo apt-get install chromium
	[sudo] password for parrot: 
	Reading package lists... Done
	Building dependency tree       
	Reading state information... Done
	.............

... el comando sudo nos pregunta nuestra contraseña de usuario y luego ejecuta la orden siguiente con privilegios administradivos.

### Trabajando con ficheros desde la Shell

	$ ls
Lista ficheros de un directorio.
	
	$ ls -l
		
Lista tambien las propiedades y atributos.
	
	$ ls -la
		
Lista ficheros incluidos los ocultos del sistema.
	
	$ ls -la | more
		
Lista los ficheros de un directorio de forma paginada.
	
	$ ls -lh
	
Lista ficheros especificando la unidad de tamaño.
	
	$ ls -l | grep^d
	
Lista solo los directorios.

	$ cat -n fichero
	
Muestra el contenido de un fichero (-n lo numera).

	$ pr -t fichero
	
Muestra el contenido de un fichero de manera formateada

	$ more fichero
	$ less fichero
	
Muestran el contenido de un fichero de forma paginada.

	$ zcat fichero
	$ zmore fichero
	$ zless fichero
Muestran el contenido de un fichero comprimido (.gz).

	$ echo cadena
Muestra en pantalla el texto que le siga.

	$ grep 'cadena' archivo
Muestra las lineas de un archivo que contienen la cadena.

	$ stat fichero
Muestra el estado de un fichero.

	$ file fichero
Muestra de que tipo es un fichero.

	$ tail archivo
	
Muestra las últimas lineas de un archivo, 10 por defecto.
	
	$ tail -n 12 archivo
Muestra las últimas 12 lineas de un archivo.
	
	$ tail -f archivo
Muestra las últimas líneas del archivo, actualizándolo a medida que se van añadiendo. Útil para controlar logs.

	$ head archivo
Muestra las primeras 10 líneas de un archivo. Admite -n al igual que el comando tail.

	$ find /usr -name lilo -print
	
Busca todos los ficheros con el nombre lilo en /usr.

	$ find /home/user -name *.jpg -print
Busca todas las imagenes *.jpg en /home/user/

	$ pwd
Visualiza el directorio actual.

	$ history
Muestra el listado de comandos usados por el usuario.

	$ cd directorio
Cambia de directorio.

	$ cd ..
Vuelve al directorio anterior.

	$ cd /home/user/Documents
Cambia al directorio Documents indicando la ruta completa.

	$ cp -pR fichero /home/user/directorio/
Copia el fichero hacia directorio, conservando el nombre actual del fichero.

	-R Indica que se va a copiar un directorio recursivamente, salvo los ficheros especiales
	-p Incica que se va a copiar preservando permisos, propietario, grupo y fechas.

	
	$ mv ruta_fichero1 ruta_fichero2
Mueve y/o renombra ficheros o directorios.

	$ mkdir directorio
Crea u directorio.

	$ rmdir directorio
Borra un directorio vacío.

	$ rm archivo
Elimina archivos.

	$ rm -r directorio
Borra los ficheros de un directorio recursivamente.

	$ wc
Muestra el numero de palabras, líneas y caracteres de un archivo.

	$ touch fichero
Crea un fichero con la fecha actual.

	$ ifconfig
Mustra la configuración de los adaptadores de red

	
