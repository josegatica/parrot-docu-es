Redes :::se trata de un conjunto de ordenadores conectados entre sí. Internet es una red de redes

Las llamadas redes de área local también denominadas con el acrónimo LAN (Local Area Network), que son las que abarcan una zona no demasiado grande, tales como un par de ordenadores domésticos, los ordenadores de un aula o centro, los ordenadores de una empresa, etc. en las cuales las conexiones se realizan mediante cables.
Las redes de área amplia o WAN (de Wide Area Network), que abarcan una región más extensa (uno o varios países, por ejemplo), y en las que los enlaces se establecen generalmente por medio de líneas telefónicas o líneas dedicadas de alta velocidad, por ejemplo: de fibra óptica, mediante satélites, etc

Las redes se comunican por protocolos, los protocolos de comunicación TCP/IP. Estas siglas corresponden a los dos protocolos que se han combinado para conseguir el conjunto de reglas que permiten la comunicación en Internet: Transmission Control Protocol e Internet Protocol. 


La información se transmite fragmentada en paquetes: sería algo similar a las piezas de un puzzle que se recomponen cuando llegan a su destino. Eso explica que cuando navegas por la web las páginas se vayan visualizando de forma fragmentada, normalmente primero el texto y luego las imágenes.

Cada paquete es autoenrutable, esto es, busca su camino para viajar hasta su destino. Ese es el motivo por el que dos ordenadores situados uno junto a otro en la misma red y solicitando a la vez la misma página web no la reciben simultáneamente, ya que mientras unos paquetes han podido tomar una "autopista" despejada otros pueden haberse encontrado con una retención o haber tomado una "carretera secundaria". Esto fue lo que dio lugar a que los anglosajones hicieran un juego fonético con las siglas World Wide Web (Red de amplitud mundial) diciendo que en muchos casos se trataba de la World Wait Web (Red de la espera mundial)

También está UDP.


Un protocolo es un método estándar que permite la comunicación entre procesos (que potencialmente se ejecutan en diferentes equipos), es decir, es un conjunto de reglas y procedimientos que deben respetarse para el envío y la recepción de datos a través de una red. 

En Internet, los protocolos utilizados pertenecen a una sucesión de protocolos o a un conjunto de protocolos relacionados entre sí. Este conjunto de protocolos se denomina TCP/IP, entre otros como:

HTTP, FTP, ARP, ICMP, IP, TCP, UDP, SMTP, Telnet,

HTTP:El propósito del protocolo HTTP es permitir la transferencia de archivos (principalmente, en formato HTML) entre un navegador (el cliente) y un servidor web (denominado, entre otros, httpd en equipos UNIX) localizado mediante una cadena de caracteres denominada dirección URL

FTP: el  objetivo del protocolo FTP es permitir el intercambio de archivos entre equipos remotos, de una manera eficaz e independientemente del sistema de archivos utilizado en cada equipo. 

El protocolo FTP está incluido dentro del modelo cliente-servidor, es decir, un equipo envía órdenes (el cliente) y el otro espera solicitudes para llevar a cabo acciones (el servidor).
Durante una conexión FTP, se encuentran abiertos dos canales de transmisión: un canal de comandos (canal de control)

ARP: El protocolo ARP tiene un papel clave entre los protocolos de capa de Internet relacionados con el protocolo TCP/IP, ya que permite que se conozca la dirección física de una tarjeta de interfaz de red correspondiente a una dirección IP Por eso se llama Protocolo de Resolución de Dirección
Para que las direcciones físicas se puedan conectar con las direcciones lógicas, el protocolo ARP interroga a los equipos de la red para averiguar sus direcciones físicas y luego crea una tabla de búsqueda entre las direcciones lógicas y físicas en una memoria cache

El protocolo RARP (Protocolo de Resolución de Dirección Inversa) es mucho menos utilizado. Es un tipo de directorio inverso de direcciones lógicas y físicas.
En realidad, el protocolo RARP se usa esencialmente para las estaciones de trabajo sin discos duros que desean conocer su dirección física.

El protocolo RARP le permite a la estación de trabajo averiguar su dirección IP desde una tabla de búsqueda entre las direcciones MAC (direcciones físicas) y las direcciones IP alojadas por una pasarela ubicada en la misma red de área local (LAN)

ICMP (Protocolo de mensajes de control de Internet): es un protocolo que permite administrar información relacionada con errores de los equipos en red.

Los mensajes de error ICMP se envían a través de la red en forma de datagramas, como cualquier otro dato. Por lo tanto, los mismos mensajes de error pueden contener errores. 

PING. Este comando, que permite evaluar la red, envía un datagrama a un destino y solicita que regrese. 
"Un datagrama es un fragmento de paquete (análogo a un telegrama) que es enviado con la suficiente información para que la red pueda simplemente encaminar el fragmento hacia el equipo terminal de datos receptor, de manera independiente a los fragmentos restantes"

Protocolo IP (Internet Protocol). Este protocolo utiliza direcciones numéricas denominadas direcciones IP compuestas por cuatro números enteros (4 bytes) entre 0 y 255, y escritos en el formato xxx.xxx.xxx.xxx. Por ejemplo, 194.153.205.26 es una dirección IP

TCP (que significa Protocolo de Control de Transmisión): es uno de los principales protocolos de la capa de transporte del modelo TCP/IP. En el nivel de aplicación, posibilita la administración de datos que vienen del nivel más bajo del modelo, o van hacia él, (es decir, el protocolo IP). Cuando se proporcionan los datos al protocolo IP, los agrupa en datagramas IP, fijando el campo del protocolo en 6 (para que sepa con anticipación que el protocolo es TCP). TCP es un protocolo orientado a conexión, es decir, que permite que dos máquinas que están comunicadas controlen el estado de la transmisión.
UDP
El protocolo UDP (Protocolo de datagrama de usuario) es un protocolo no orientado a conexión.

Existen miles de puertos (codificados en 16 bits, es decir que se cuenta con 65.536 posibilidades). Es por ello que la IANA (Internet Assigned Numbers Authority [Agencia de Asignación de Números de Internet]) desarrolló una aplicación estándar para ayudar con las configuraciones de red.

///////////////
Los puertos del 0 al 1.023 son los puertos conocidos o reservados. En términos generales, están reservados para procesos del sistema (daemons) o programas ejecutados por usuarios privilegiados. Sin embargo, un administrador de red puede conectar servicios con puertos de su elección. Los puertos del 1.024 al 49.151 son los puertos registrados. Los puertos del 49.152 al 65.535 son los puertos dinámicos y/o privados

Puertos conocidos y más usados 

21:	FTP
23: Telnet
25: SMTP
53: Sistema de nombre de dominio
63: Whois
70: Gopher
79: Finger
80: HTTP
110: POP3
119: NNTP
