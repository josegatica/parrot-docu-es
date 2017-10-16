# Shell y Comandos Básicos de Linux:

Todo usuario nuevo, antes de hacer cualquier otra cosa en Linux, debe entender como funciona la Shell y como hacer uso de esta en Linux. La Shell nos permite ejecutar comandos para realizar casi cualquier función en el sistema, es la forma que tenemos de hablar directamente con el sistema operativo sin necesidad de utilizar la GUI (Interfaz Gráfica de Usuario).

Un exelente comando para comezar a utilizar la shell es "uname", este nos muestra que sistema operativo estamos usando.

$ uname

Linux


Haciendo uso de la obcion -a podemos encontrar información adicional.

$uname -a

Linux parrot 4.11.0-parrot6-amd64 #1 SMP Parrot 4.11.6-1parrot6 (2017-06-28) x86_64 GNU/Linux


La obción "-a" nos aporta más información, incluyendo la version actual del Kernel Linux que esta sienso usado, el Hostname, la Arquitectura del sistema. El comando uname -a es un exelente comando para comenzar a trabajar con la Shell de Linux, ya que nos permite obtener información sobre el sistema operativo que estamos usando. La Shell nos permite ejecutar comandos Internos y Externos, es importante diferenciar cada uno de estos dos tipos.

Los comandos Internos son aquellos que están integrados en la Shell (Built into shell), estos comandos internos nos permiten realizar tareas comunes como:


Mostrar el directorio actual:

$ pwd


Cambiar de directorio:

$ cd /ruta/del/nuevo/directorio


Mostrar un texto en pantala:

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


Estos son algunos conceptos básicos que deben aprender antes de profundizar en el uso de la Shell y de comandos en GNU/Linux. Es importante tener conocimiento no solo de las operaciones que se pueden realizar con un comando sino tambien de su origen y de como funciona este.

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

# == Conclusión pendiente ==
