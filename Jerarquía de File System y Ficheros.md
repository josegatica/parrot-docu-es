JERARQUÍA DE FILESYSTEM Y FICHEROS


El principal problema de todo usuario migrante a GNU/Linux es la estructura de archivos y ficheros, ya que comúnmente en Windows se tiene la costumbre de mencionar directorios como “C:/users/User/Desktop”,  lo que no pasa en la distribución GNU/Linux, para facilitar la “migración” de  los usuarios hacia GNU/Linux, a continuación se explicará de manera detallada la jerarquía de filesystems que contiene el sistema ya mencionado. Encuadrados en dos tipos básicos Estáticos/Dinámicos y Compartibles/Restringidos en los que se organiza todo el árbol de directorios de Linux.


ALGUNAS CARACTERÍSTICAS DEL SISTEMA DE ARCHIVOS DE LINUX


* Basado en un árbol jerárquico de directorios.
* Estandarizado en 1993 por el proyecto FHS (Filesystem Hierarchy Standard).
* Todo en Linux es un archivo.
* Organización del sistema de archivos según FHS.


CLASIFICACIÓN TIPOLÓGICA GNU/LINUX


* ESTÁTICOS: Contiene archivos que sólo root puede cambiar. Binarios, bibliotecas, documentación, etc. Sin embargo éstos están disponibles para sólo lectura por cualquier otro usuario del sistema. Algunos de los directorios son: /bin, /boot, /opt, /sbin, /usr, /proc, etc.


* DINÁMICOS: Como su nombre lo dice, en esta categoría se encuentran los archivos que pueden ser modificados, (algunos de ellos solo por root). De estos tipos de directorios se suele hacer copias de seguridad más a menudo. Algunos de los directorios son: /var/mail, /var/spool, /var/run, /home, etc.


* COMPARTIBLES: Son directorios que se pueden compartir a través de red.


* RESTRINGIDOS: Contiene directorios que no se pueden compartir, sólo pueden ser visualizados y/o modificados por root. Algunos de los directorios son: /etc, /var/lock, /var/run, etc.


Todos los directorios Linux/Unix cuelgan del denominado directorio raíz “/” podría a primera vista hacerse una comparación con el directorio “C:\” de Windows, sin embargo existen notables diferencias entre ellos: 
* Del directorio “/” de Linux cuelgan todos los dispositivos y las distintas particiones de  todos los discos y unidades usados por el sistema; las particiones, discos y dispositivos en general son “montados” en Linux en un directorio especial que está siempre por debajo del “directorio raíz”; ocurre lo contrario en los sistemas Windows ya que éstos nombran  a cada partición y dispositivo por una letra distinta C:, D:. E:.


Al mencionar “montar una unidad”  nos referimos a que ésta ya sea partición o dispositivo comienza a formar parte de nuestro sistema y por tanto, es posible trabajar con él. En Linux podemos montar y desmontar particiones a nuestro antojo en el momento que queramos usarlas o que dejen de ser accesibles a nuestro sistema.


Para entender la jerarquía de los Ficheros en  Linux a continuación daremos  una breve explicación de ALGUNOS de ellos:




DIRECTORIO
	DEFINICIÓN/CONTENIDO
	/
	Directorio Raíz o Principal, el que contiene toda la estructura de directorios y ficheros del sistema, en otras palabras, todo lo que existe en Linux está en algún punto por debajo de este directorio.
	/bin
	Contiene los binarios esenciales de los sistemas Linux/Unix, accesibles para todos los usuarios. Al referirnos a estos archivos como “binarios” nos estamos refiriendo a ejecutables ya compilados.
	/boot
	Contiene el Kernel (núcleo del sistema), el initrd,  el grub, etc.  Son archivos utilizados durante el arranque del sistema.
	/dev
	Agrupa los dispositivos esenciales, almacenamiento, teclado, terminales, sonido, video, cd, dvd, impresoras, etc. Me atrevería a decir que aqui se guarda
	/etc
	Archivos de configuración del sistema, nombre del host, red,  usuarios, programas, etc.
También contiene subdirectorios como /x11 aquí se puede configurar el sistema gráfico de nuestro Linux
	/home
	Es el directorio donde se guardan los usuarios y archivos de usuario del sistema, a excepción del usuario “root”, éste tiene su propio directorio /root, los demás cuelgan del directorio /home. Por ejemplo: /home/usuario
	/lib
	Contiene librerías (comúnmente llamadas bibliotecas) esenciales compartidas por los programas alojados dentro de los directorios /bin y /sbin, así como por el Kernel.
	/lost+found
	Es una carpeta en la que podemos encontrar todas las particiones. Cuando ocurre un error de hardware ajeno al sistema (comúnmente cortes de energía), cuando éste se reinicie notarás que se llamará al programa fsck para restaurar la integridad del sistema de ficheros. En esta carpeta encontraremos la información que se malguardó debido a la incidencia.
	/mnt
	Sistemas de ficheros  montados temporalmente (usb, disco duro, etc). Existen diferencias entre el uso de este, y el directorio /media, en la mayoría de distribuciones Linux, ésta comúnmente se utiliza para montar las particiones windows (en caso que tengamos un sistema dual boot).
	/media
	Contiene los puntos de montaje para dispositivos como unidades lectoras (CD, DVD). No confundir con el directorio /dev. Tanto /media como /mnt son los que contienen los puntos de montaje, no los dispositivos en sí.
	/opt
	Aquí  normalmente se almacenan los paquetes que instalas manualmente (sin ejecutar el comando apt install  en la terminal).
	/proc
	Sistema de ficheros virtual que documenta sucesos y estados del núcleo, contiene principalmente ficheros de texto, información de memoria, interrupciones, irq, etc.
	/root
	El /home del administrador, el único /home que no está incluído (por defecto) en el directorio ya mencionado .
	/sbin
	Binarios del sistema o administrativos, aquí se alojan los binarios que en su mayoría utiliza “root” .
	/srv
	Contiene información del sistema sobre ciertos servicios que ofrece (FTP, HTTP,...).
	/sys
	Contiene información sobre los dispositivos tal y como los ve el Kernel de Linux.
	/tmp
	Es un directorio donde se almacenan ficheros temporales, ya sean de instalación o ejecución de los programas. Cada vez que se inicia el sistema este directorio se limpia.
	/usr
	A diferencia de /sbin ó /bin contiene binarios que no son esenciales para el sistema, en este directorio se encuentra una sub jerarquía para datos compartidos de solo lectura. Este directorio puede ser compartido por múltiples ordenadores, pero no debe contener datos en el ordenador que los comparte.
	/usr/bin
	Binarios administrables de la mayoría de  aplicaciones de escritorio entre otras disponibles para todos los usuarios
	/usr/include
	Contiene archivos cabecera (headers files o inlclude files) son archivos que son aprovechados por otros archivos incluyéndolos en su contenido.
	/usr/lib
	Bibliotecas compartidas de los binarios
	/usr/local
	Datos locales del host. Es otro nivel dentro que ofrece una jerarquía parecida al propio directorio /usr.
	/usr/sbin
	Sistema de binarios no esenciales, por ejemplo demonios ejecutados normalmente durante el arranque.
	/usr/share
	Datos compartidos independientes de la arquitectura del sistema (imágenes, ficheros de texto, etc). Dentro de este directorio está gran parte de la documentación del sistema.
	/usr/src
	Contiene en su interior el código fuente de los programas, por ejemplo el kernel de Linux
	/var
	Contiene ficheros variables (logs,servidores http, ftp, colas de correo, etc).
	/var/cache
	Memoria cache de los datos de aplicaciones, en ocasiones el directorio /tmp tiene un uso parecido.
	/var/games
	Contiene los datos variables de los juegos.
	/var/lib
	Información sobre el estado actual de las aplicaciones, modificable por las propias aplicaciones.
	/var/lock
	Ficheros que se encargan de que un recurso sólo sea usado por una aplicación determinada que ha solicitado su exclusividad, hasta que ésta lo libere.
	/var/log
	Aquí se almacenan todo tipo de logs del sistema y aplicaciones.
	/var/mail
	Buzón de correos o mensajes de los usuarios. Cuando no se utiliza el cifrado, esta informacion suele almacenarse en la carpeta personal del usuario (/home/usuario).
	/var/opt
	Datos usados por los paquetes almacenados en /opt.
	/var/run
	Contiene información del sistema desde que se inició (usuarios logueados, demonios en ejecución, etc).
	/var/spool
	Datos esperando  que sean tratados por algún tipo de proceso (colas de impresión, correos  no leídos, etc).
	/var/tmp
	Archivos temporales que no se borran entre sesiones o reinicios del sistema, pero aun así pueden ser prescindibles.