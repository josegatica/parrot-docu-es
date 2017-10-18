## Arranque de un Sistema Linux.

El proceso de arranque en todo sistema de computadores comienza desde la BIOS y Linux no es la excepción. En este capítulo vamos a hablar sobre el proceso de arranque de un sistema Linux, vamos a ver qué sucede en nuestro sistema desde que pulsamos el botón de encendido hasta que el sistema operativo está completamente cargado. También vamos a ver las distintas fases por las cuales pasa nuestro sistema en todo el proceso de arranque incluyendo archivos y comandos involucrados. Básicamente existen cuatro fases de arranque en un sistema Linux:

Fase 1: Hardware y BIOS

Fase 2: BootLoader o Cargador de Arranque

Fase 3: Kernel o Nucleo del SO

Fase 4: Init

A continuación vamos a ver cada una de estas fases de arranque y cómo funcionan.

## Fase 1: Hardware y BIOS

  El proceso de arranque comienza desde que presionamos el botón de encendido en nuestro ordenador. En esta fase el sistema se inicia pasando el control a la BIOS.
	
  La BIOS es un pequeño programa que se encuentra grabado en la memoria de la placa (Motherboard), este programa guarda la configuración de nuestro sistema, es el encargado de realizar el POST, Power on Self Test o Auto Prueba de Encendido (Es el proceso de verificación de los componentes de un sistema computacional, se encarga de configurar y diagnosticar el estado del hardware). Esta memoria donde se encuentra la BIOS, se mantiene continuamente alimentada por la batería de la placa para mantener la configuración.

 La BIOS se encarga de realizar las siguientes tareas:
	
  - Verificar la Integridad del código de la BIOS.
	
  - Determina por qué se ejecuta el POST (arranque en frío, reset, error, standby, hibernación, etc.)
	
  - Busca, dimensiona y verifica la memoria del sistema (RAM y ROM)
	
  - Proporciona la interfaz de usuario para configurar parámetros del sistema (Velocidad de CPU, orden de arranque, tunning y overclocking, entre otras configuraciones particulares de otros fabricantes)
	
  - Identifica, organiza y selecciona los dispositivos de arranque disponibles.
	
  - Comienza el proceso de arranque del sistema, llamado Bootloader.

  Una ves que la BIOS realiza todas las pruebas necesarias y chequea la configuración correspondiente del sistema, si todo esta bien, pasa el control del sistema al Bootloader o Cargador de Arranque.

## Fase 2: Bootloader

  El objetivo del Bootloader es cargar parte del Núcleo (Kernel) del sistema operativo y ejecutarlo. En esta fase el bootloader toma el control del sistema computacional y se encarga de cargar el resto del sistema operativo. Existen varios tipos de Bootloaders y estos pueden ser cargados desde varias unidades de almacenamiento.

Ubicaciones de los Bootloaders (Cargadores de arranque):
	
  - En un disquete (actualmente es una opción obsoleta).
	
  - En el disco duro: a menudo se encuentra ubicado en el primer sector de una partición del disco duro, en el sector de arranque global MBR (Master Boot Record) o en moderno sistema de particiones GUID Globally-Unique Identifier (GPT) que es el estandar EFI (Extensible Firmware Interface) propuesto por Intel para reemplazar el viejo BIOS (GPT sustituye al MBR usado con el BIOS en ordenadores y portátiles modernos).
	
  - También podemos encontrar el Bootloader en un CD-ROM o DVD-ROM
	
  - Existen algunos tipos de Bootloaders que se pueden cargar desde la red como es el caso de LinuxBios (una alternativa Open Source que tiene como objetivos sustituir la BIOS normal con una Bios con una pequeña inicialización de Hardware y un kernel de Linux comprimido, evitar el uso de Bootloaders, entre otras...)




Tipos de Bootloaders en Linux
	
  - LILO: The LInux LOader
	
  - GRUB: GRand Unifying Bootloader

	Ambos son capaces de cargar tanto sistemas Linux como otros sistemas operativos y suelen estar ubicados en el MBR del disco duro.

  LILO: es un Bootloader rudimentario de una sola etapa, no entiende de sistemas operativos ni de sistemas de ficheros. Lilo lee datos desde el disco utilizando llamadas nativas de la BIOS que indican directamente los ficheros que se necesitan, estos ficheros son almacenados a través de un fichero mapa que se almacena en el sector de arranque.

  Funcionamiento de LILO: El firmware carga el sector de arranque de LILO y lo ejecuta, luego LILO carga su fichero mapa por medio de llamadas de la BIOS, el cual muestra el prompt de opciones a cargar. El usuario selecciona el kernel que desea arrancar y LILO carga el kernel seleccionado por medio de llamadas de la BIOS y utilizando los parámetros de ubicación en el fichero mapa. Por último, LILO ejecuta el kernel indicando donde esta el root fs (el sistema de archivos de raiz) y si es necesario el ramdisk.

Ficheros de LILO:
	
- Ejemplo de /etc/lilo.conf

		boot=/dev/hda2
		root=/dev/hda2
		install=/boot/boot.b
		map=/boot/map
		vga=normal
		delay=20
		image=/vmlinuz
		label=Linux
		read-only
		other=/dev/hda1
		table=/dev/hda
		label=win
	
  - Para cargar la configuración hay que ejecutar el comando lilo.
	
  		$ lilo /etc/lilo.conf


  GRUB: es un Bootloader más avanzado y más moderno que LILO. Trabaja en dos o tres etapas (Stages) y tiene capacidad para cargar un kernel vía red. GRUB en cada etapa va cargando más elementos para arrancar, entiende de ficheros y permite especificar parámetros de forma dinámica en el arranque, no utiliza valores estáticos.
	
  Funcionamiento de GRUB: como se mencionó anteriormente, GRUB tiene dos o tres etapas, se dice que tiene dos o tres porque la segunda etapa es opcional. A continuación vamos a ver cada una de estas estapas.

  Etapa 1: El firmware carga el sector de arranque de GRUB en memoria.
	
  Etapa 1.5: Su objetivo es cargar el código que reconoce sistemas de ficheros y a partir de ahí carga la etapa 2 como un fichero.
	
  Etapa 2: GRUB muestra el menú con las opciones de boot que hayamos definido y un prompt donde podemos especificar ramdisk, kernels, etc. a cargar.
	
  Luego de estas etapas, GRUB ejecuta los comandos introducidos, las definidas por nosotros en el fichero de configuración (grub.conf, menu.lst, grub.cfg, en dependencia de la distribución) y comienza la carga del kernel.


Estas etapas y caracteristicas de GRUB demuestran su potencia y superioridad a LILO, es capaz de cargar ficheros y realizar tareas dinámicas en la fase de arranque del sistema, de ahí que es es Bootloader por exelencia en la gran mayoria de las distribuciones.

Ficheros de GRUB en Parrot:

	$ ls -la
	total 1359
	drwxr-xr-x 5 root root    1024 oct  3 21:36 .
	drwxr-xr-x 4 root root    1024 oct 12 22:34 ..
	drwxr-xr-x 2 root root    1024 oct  3 21:36 fonts
	-r--r--r-- 1 root root    6574 oct  3 21:36 grub.cfg
	-rw-r--r-- 1 root root    1024 oct  3 21:36 grubenv
	drwxr-xr-x 2 root root    9216 oct  3 21:36 i386-pc
	drwxr-xr-x 2 root root    1024 oct  3 21:36 locale
	-rw-r--r-- 1 root root 1362622 oct  3 21:24 unicode.pf2


Estos ficheros varían en dependencia de la distribución, en distribuciones basadas en Debian, suele verse así.

## Fase 3: Kernel

Breve descripción del Kernel Linux:
	
  - Arquitectura Monolítica.
	
  - Es un complejo programa compuesto de un gran número de subsistemas lógicos.
	
  - Gestionado directamente por Linus Trovadls.
	
  - Con capacidad de carga de Módulos.
	
  - Esta formado por una capa lógica pero internamente funciona con más.


En esta fase comienza la ejecución del kernel, descomprimiéndose a sí mismo. Luego comienza la inicialización de kernel y el chequeo y puesta en marcha de algunos de los dispositivos para los que se ha dado soporte.
	
  - Detecta la CPU y su velocidad.
	
  - Inicializa el display para mostrar información por pantalla.
	
  - Comprueba el bus PCI e identifica y crea una tabla con los periféricos conectados.
	
  - Inicializa el sistema de gestión de memoria virual, incluyendo el swapper (intercambiador o memoria de intercambio, swap).
	
  - Inicializa todos los periféricos compilados dentro del kernel, normalmente sólo se configuran así los periféricos necesarios para esta fase del arranque, el resto se configuran como módulos.
	
  - Monta el sistema de ficheros root (/).
	
  - A partir de aquí llama al proceso init que se ejecuta con un uid 0 y será el padre de todos los demás procesos.

## Fase 4: Init.

En estos momentos el kernel está cargado, tenemos gestión de memoria, una parte del harware está inicializado y tenemos un sistema de ficheros root. A partir de ahora, el resto de las operaciones se van a realizar directa o indirectamente por el proceso init. El proceso init lee del fichero /etc/inittab la configuración a utilizar y ejecuta el comando /etc/rc.sysinit, el cual realiza una inicialización básica del sistema. En función del runlevel ejecuta los comandos establecidos.

Hasta aquí hemos visto las cuatro Fases del proceso de arranque de un sistema Linux en un ordenador. Podemos concluir este capítulo con el siguiente resumen:

El proceso de arranque de un sistema Linux en un ordenador comienza desde que presionamos el boton de encendido, éste le da vida a nuestro hardware haciéndolo funcionar. Luego del encendido, el hardware es testeado por el POST de la BIOS, este hace un mapeo del hardware que tenemos en nuestro ordenador y lo prueba, si todo esta funcionando correctamente, continúa el proceso de arranque. La BIOS utiliza la configuración predeterminada por el fabricante de la placa de nuestro ordenador o una configuración modidificada por el usuario, luego da paso al Bootloader o Gestor de Arranque que tengamos instalado en la partición inicial de nuestro disco duro. El Bootloader es el encargado de mostrarnos las opciones de boot que configuramos previemente en la instalación del sistema, las opciones por defecto en una instalación reciente o las de un DVD de instalación o Live. Una vez que el usuario escoge una opción de boot, el Kernel es descomprimido y posteriormente se inicia. El Kernel realiza un pequeño chequeo de los dispositivos necesarios y a los cuales se le ha dado soporte, como es el caso de CPU, Display, memoria RAM y memoria virtual (swap) y otros dispositivos necesarios, el Kernel termina montando el sistema de ficheros root y por último inicia el proceso init. Init es el encargado de el resto de iniciar el resto de los procesos del sistema, iniciando así el login en modo texto o la interfaz gráfica en sistemas con GUI (Interfaz Gráfica de Usuario) y permitiéndonos hacer uso del sistema operativo.
