## Introducción

Esta es una guía muy básica del funcionamiento de una red. Nos servirá para poder entender más adelante cómo configurar nuestro sistema ParrotSec OS.
En un futuro se espera de usted, lector, que investigue y estudie por su cuenta más (mucho más).

Actualmente, gran cantidad de los dispositivos que nos rodean está conectados a Internet o al menos a nuestra red de casa u oficina. Nuestra SmartTV, teléfonos móviles, smartwatches, tablets, PC, cafetera(?),...

Es por ello, que conviene conocer a grosso modo, el funcionamiento de nuestras redes, que significan todas esas siglas y palabras que nos solicitan cuando debemos configurar nuestros dispositivos.

## Dirección IP

En este documento vamos a tratar la versión 4 de IP (IPv4), aunque se está extendiendo cada vez más la versión 6 (IPv6). Pese a este incremento en el número de configuraciones IPv6, aún no es común dicha configuración en nuestras casas por lo que trataremos de explicar IPv4.

Todos los sistemas que necesiten comunicarse entre ellos, deben disponer de al menos una dirección IP. Esta dirección debe ser única y no puede repetirse en una misma red. En caso de que esto ocurra, los dos dispositivos quedarán anulados hasta que se resuelva el conflicto. Estas direcciones IPs son asignadas a las tarjetas de red o wifi.

Una dirección IP (v4) está compuesta por cuatro bloques de números decimales, separados por puntos. Cada bloque de números puede ir del 0 al 255. 

Ejemplos:

	* 192.168.0.1
	* 1.2.3.4
	* 195.255.1.37


Existen direcciones ip públicas y privadas. Las direcciones IP públicas son las que se utilizan para poder interconectar dispositivos por Internet, mientras que las privadas son utilizadas dentro de una organización, empresa o en nuestras propias casas.  

Pondré un ejemplo para intentar explicar esto (los datos son inventados y los pondré sólo a modo de ejemplo). Supongamos nuestro router de casa, nuestro PC, una impresora y nuestra página favorita de internet.

	PC, impresora, router: Disponen de ip privada (pertenecen a la red de nuestra casa)
	Router, Página WEB: Disponen de ip pública (necesitan una ip pública para comunicarse)

Puede observar que el router mantiene dos IP: una pública para poder acceder a los servicios externos que nos brinda Internet, y otra privada para interconectarse con los sistemas de nuestra red de casa.
	
	IP Pública
	----------------------------------
	Página WEB: 	104.24.124.114
	Router: 	85.83.4.127


	IP Privada
	----------------------------------
	Router: 	192.168.0.1
	PC: 		192.168.0.10
	Impresora: 	192.168.0.12


Podríamos hacer una analogía entre lo mostrado anteriormente y un sistema telefónico. Todos los números de teléfono son distintos (IP pública), pero tenemos centralitas (routers) que son capaces de direccionar una llamada (enrutar) a diferentes extensiones (IP privada). Estas extensiones, fijándonos únicamente en ellas, se pueden repetir en diferentes empresas u hogares, pero nunca dentro de ellas.


## Máscara de red

La máscara de red se utiliza en las redes privadas para indicar el rango de una subred.

Las IP privadas deben estar en los rangos indicados a continuación:

	De 10.0.0.0 a 10.255.255.255
	172.16.0.0 a 172.31.255.255
	192.168.0.0 a 192.168.255.255


Las siguientes IPs no deberían configurarse de modo manual:

	169.254.0.0 a 169.254.255.255 
	224.0.0.0 to 224.0.0.255
	224.0.1.0 to 224.0.1.255
	224.0.2.0 to 224.0.255.255
	224.3.0.0 to 224.4.255.255
	232.0.0.0 to 232.255.255.255
	233.0.0.0 to 233.255.255.255
	233.252.0.0 to 233.255.255.255
	234.0.0.0 to 234.255.255.255
	239.0.0.0 to 239.255.255.255


Cualquier otra dirección IP se considerará Pública. Las direcciones IP públicas, generalmente, vendrán asignadas por nuestro proveedor de Internet, por lo que no deberemos preocuparnos por ellas (al menos no de momento).

Vuelva a mirar el rango de direcciones IP privadas. Como hemos indicado la máscara de red especifica la subred a la que pertenece un sistema.
Es decir, con este valor indicamos a nuestro equipo si debe enviar un dato (paquete) a un equipo de nuestra subred o no. Dicho de otra forma, dada una dirección IP y una máscara de red, sabemos qué parte de dicha dirección IP es el valor de la subred y cuál es el número correspondiente al host. Con la máscara también indicamos el número máximo de hosts que pueden configurarse en una subred. Otra forma de indicar la máscara es con su ICDR

Tabla de máscaras posibles:

	Decimal 	CIDR 	Nº hosts
	--------------- ----	--------
	255.255.255.255	/32
	255.255.255.254	/31
	255.255.255.252	/30 	2
	255.255.255.248	/29 	6
	255.255.255.240	/28 	14
	255.255.255.224	/27 	30
	255.255.255.192	/26 	62
	255.255.255.128	/25 	126
	255.255.255.0 	/24 	254 	
	255.255.254.0 	/23 	510
	255.255.252.0 	/22 	1022
	255.255.248.0 	/21 	2046
	255.255.240.0 	/20 	4094
	255.255.224.0 	/19 	8190
	255.255.192.0 	/18 	16382
	255.255.128.0 	/17 	32766
	255.255.0.0 	/16 	65534 	
	255.254.0.0 	/15 	131070
	255.252.0.0 	/14 	262142
	255.248.0.0 	/13 	524286
	255.240.0.0 	/12 	1048574
 	255.224.0.0 	/11 	2097150
	255.192.0.0 	/10 	4194302
 	255.128.0.0 	/9 	8388606
 	255.0.0.0 	/8 	16777214 	
 	254.0.0.0 	/7 	33554430
 	252.0.0.0 	/6 	67108862
 	248.0.0.0 	/5 	134217726
 	240.0.0.0 	/4 	268435454
 	224.0.0.0 	/3 	536870910
	192.0.0.0 	/2 	1073741822
 	128.0.0.0 	/1 	2147483646
	0.0.0.0 	/0 	4294967294


Normalmente se suele utilizar las máscaras de red:
	255.255.255.0
	255.255.0.0
	255.0.0.0

Y más habitualmente en nuestros hogares la 255.255.255.0.

De esta forma, y continuando con nuestro ejemplo, lo más lógico es que nuestro PC tenga:
	
	IP: 		192.168.0.10
	Máscara de red: 255.255.255.0

O con la notación ICDR:
	
	192.168.0.10/24

Así nuestro equipo podría comunicarse directamente con cualquier sistema de nuestra red cuya dirección IP comenzase por 192.168.0.

Si disponemos de un equipo que pertenezca a la subred 192.168.1, ya no se podría comunicar directamente y debería utilizar un gateway o puerta de enlace, que veremos a continuación.

Por otro lado, si queremos que las redes 192.168.0 y 192.168.1 se vean directamente, podríamos utilizar la máscara de red 255.255.0.0. En este caso, realmente, se estarían viendo todos los sistemas cuyas ips comenzasen por 192.168 sin importarnos los siguientes dos bloques de números.


## Gateway o Puerta de enlace

Cuando un sistema debe enviar un dato (paquete) a una red a la que no pertenece, lo cual sabe por su máscara de red visto anteriormente, buscará en su tabla de enrutamiento la dirección ip a la que debe enviar dicho dato para que salga al "exterior". Es decir, si nuestro PC quiere comunicarse con un sistema que no está en su red, debe conocer una ip por la que enviará este dato, dejando el control de envío a dicho sistema. 

En nuestro ejemplo sabemos que el router tiene una dirección IP 192.168.0.1 para nuestra red interna. Sólo disponemos de esta red en nuestro hogar. Por lo tanto, nuestra ruta por defecto o gateway deberá ser la dirección del router. Dicho de una forma coloquial, si enviamos un paquete a un sistema que no está en nuestra red, enviémoslo al router, que él sabrá lo que debe hacer con dicho paquete. Internamente el router cuando reciba el paquete y vea la dirección a la que va destinada, activará sus mecanismos para enviarla a través de su dirección pública al exterior, poniéndose en contacto con el sistema externo.


## DNS (Domain Name Server)

Como ya hemos dicho anteriormente, todos los sistemas conectados disponen de una dirección IP. Sería muy complicado que nos supiésemos de memoria todas las direcciones IP de todas las páginas a las que nos solemos conectar. Es por ello que existen servidores que son capaces de traducir un nombre a una dirección ip (y viceversa). Estos servidores se denominan DNS (Servidor de resolución de nombres).

Pongamos como ejemplo la navegación a una página web. Cuando escribimos en nuestro navegador el nombre de una página, el sistema, primeramente, preguntará a nuestro DNS cual es la dirección IP de la dirección que estamos solicitando. Nuestro DNS devolverá, a nuestro sistema, una dirección IP que se corresponderá con la página que estamos solicitando.

La herramienta "dig" nos permite comprobar las direcciones ips de un nombre de dominio.

	┌─[user@parrot]─[~]
	└──╼ $dig parrotsec.org
	
	; <<>> DiG 9.10.6-Debian <<>> parrotsec.org
	;; global options: +cmd
	;; Got answer:
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59479
	;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

	;; OPT PSEUDOSECTION:
	; EDNS: version: 0, flags:; udp: 4000
	;; QUESTION SECTION:
	;parrotsec.org.			IN	A

	;; ANSWER SECTION:
	parrotsec.org.		300	IN	A	104.24.125.114
	parrotsec.org.		300	IN	A	104.24.124.114

	;; Query time: 91 msec
	;; SERVER: 212.142.144.66#53(212.142.144.66)
	;; WHEN: Sun Nov 19 23:18:48 CET 2017
	;; MSG SIZE  rcvd: 74


Puede ver dos cosas importantes en este ejemplo. La sección "ANSWER SECTION" nos indica las direcciones IPs de parrotsec.org y la línea SERVER nos indica cuál fue el servidor DNS al que preguntamos.


Podemos configurar nuestros sistemas con más de un servidor DNS, por si el principal falla.


## DHCP (Dynamic Host Configuration Protocol)

En los puntos anteriores hemos definido varios datos que serán imprescindibles para configurar nuestros sistemas y que puedan conectarse:
	
	* Dirección IP
	* Máscara de Red
	* Gateway
	* DNS

Estos valores podemos configurarlos de modo manual, aunque también se pueden configurar de forma automática cuando conectemos el sistema a la red. Estos valores nos los puede proporcionar y configurar un servidor DHCP. Generalmente, los routers vienen con esta característica activada para que no tengamos que preocuparnos por nada.


## Nota Final

Tal como hemos indicado al principio, este documento es una breve (muy breve) introducción a redes, y una vez usted entienda el funcionamiento básico de una red debería investigar y estudiar de una forma más extensa el funcionamiento de esta.

A continuación, le proponemos enlaces que podrían interesarle en un futuro:

	* https://rfc-es.org/
	* https://www.rfc-editor.org/rfc-index.html
	* http://www.tldp.org/HOWTO/Networking-Overview-HOWTO.html
