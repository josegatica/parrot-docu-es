# APT (Gestion de software Parrot)

Veremos en este capítulo una introducción al gestor de paquetes APT para Parrot.
Un programa es una serie de instrucciones. Estas instrucciones viene en archivos de texto, llamados fuentes. Para que funcionen en nuestros sistemas se necesita pasarlos a lenguaje máquina. A este paso se le llama compilación. La compilación genera uno o varios archivos, entendibles por el sistema, que se denominan binarios.

Actualmente no es necesario que el usuario compile las fuentes de cada programa. Los desarrolladores se encargan de compilarlos por nosotros y generar los respectivos binarios. Como un programa puede llevar, no sólo el ejecutable, sino otra serie de ficheros, los desarrolladores "empaquetan" dicho software en un archivo llamado paquete. Dos son los más famosos, paquetes RPM y paquetes DEB. RPM fue desarrollada por Red Hat y DEB por debian. Parrot utiliza el formato DEB.

Para compilar algunos programas son necesarias librerías y otros programas. Si intentasemos compilar un programa que tuviese dependendencias con otras librerías y otros programas, anteriormente a su compilación deberiamos instalar dichas "dependencias". Igualmente, si queremos instalar un binario necesitaremos tener instaladas las dependencias necesarias para su correcto funcionamiento.

Para gestionar estas dependencias y la instalación de los "paquetes", se han creado gestores de paquetes. Existen numerosos gestores de paquetes, algunos gráficos y otros en línea de comando. En este capítulo veremos uno de los más famosos, creados por los desarrolladores debian, y el utilizado por Parrot...APT.

Las funciones principales de un gestor de paquetes debe ser:

- Busqueda de software
- Instalación de software
- Actualización de software
- Actualización de sistema
- Gestión de dependencias
- Eliminación de software


El gestor de paquetes debe comprobar en una ubicación dada (puede ser un directorio local o una dirección de red), la disponibilidad de dicho software. A estas ubicaciones se les llaman repositorios. El sistema mantiene archivos de configuración para comprobar la ubicación de sus reposittorios.

Comenzemos...


## Lista de repositorios

Pese a que en Parrot, no es necesario(ni recomendado) añadir repositorios nuevos ni modificar los existentes, veremos donde podemos configurar estos.
En el sistema de ficheros, encontramos en la ruta "/etc/sources.list.d", el archivo parrot.list. El contenido de este debería ser:

	## stable repository
	deb http://deb.parrotsec.org/parrot stable main contrib non-free
	#deb-src http://archive.parrotsec.org/parrot stable contrib non-free

Con esto nos aseguramos tener la lista de repositrios correcta. En esta ubicación, los desarrolladores de parrot, mantienen los paquetes actualizados.

También puede ver el documento de "Lista de espejos (Mirrors)".


## Gestor de paquetes (APT)

El gestor de paquetes de parrot es apt. Este gestor se encarga de instalar paquetes, comprobar dependencias, actualizar sistema, entre otras cosas. 
Veamos que podemos hacer con él. Veremos las opciones mas comunes pero podemos ver varias páginas man (apt, apt-get, apt-cache, dpkg)

- Buscar un paquete o cadena de texto:
	
		# apt search <cadena_texto>

- Mostrar información del paquete:

	$ apt show <paquete>

- Mostrar dependencias de un paquete:

	$ apt depends <paquete>

- Mostrar los nombres de todos los paquetes instalados en el sistema:
	
	$ apt list --installed

- Instalar un paquete:
 
	$ apt install <paquete>

- Desinstalar un paquete:

	$ apt remove <paquete>

- Eliminar un paquete incluidos sus ficheros de configuración:

	$ apt purge <paquete>

- Eliminar de forma automática aquellos paquetes que no se estén utilizando:

	$ apt autoremove

- Actualizar información de los repositorios:
	
	$ apt update

- Actualizar un paquete a la última versión disponible en el repositorio:

	$ apt upgrade <paquete>

- Actualizar el sistema. Actualizará todos los paquetes que dispongan de una versión superior:

	$ apt upgrade

- Actualizar la distribución completa. Actualizará nuestro sistema a la siguiente versión disponible:

	$ apt dist-upgrade

- Limpiar cachés, paquetes descargados, etc:

	$ apt clean   
	$ apt autoclean


Estos son sólo unos ejemplos. Si requiere más información debería comprobar la página del manual (man 8 apt).
