
Anonimato GNU/Linux con Proxychains

(Solo para motivos educativos)


Proxychains es un programa disponible solamente para GNU/Linux y Unix que nos permite crear cadenas de proxies, “ocultando” así nuestra IP pública real en todo tipo de conexiones (HTTP, FTP, SSH, etc…). Esto se traduce en que podemos navegar por Internet o realizar cualquier operación en la red de redes sin descubrir nuestra identidad real.

¿Cómo funciona esto? ¿Es realmente posible? ¿Podría ocultar mis pasos en Internet?

Para poder conocer la respuesta a estas preguntas es necesario tener una mínima noción de lo que es un proxy en la jerga informática.


¿Qué es un proxy?

Un proxy puede definirse como un ordenador o servidor en el cual está corriendo un servicio de proxy, es decir, un “programa” que permite a ese ordenador actuar de intermediario entre nuestro ordenador y el destino final. En este caso, Proxychains nos ofrece conectarnos a más de uno en cadena.

Por ejemplo, imaginemos que nuestra IP pública es 77.123.21.3 y que queremos conectarnos a 80.12.54.23. Podríamos usar una cadena de proxies para conectarnos anónimamente, como muestra el dibujo:


(imagen) https://goo.gl/nMZKNc


<sudo nano /etc/proxychains.conf>


Si no lo hemos modificado inicialmente veremos esta configuración:


(imagen) https://goo.gl/78pZx2


La mejor opción es descomentar “dynamic_chain”, es decir borrar el # antes de la línea y comentar # “strict_chain

(imagen) https://goo.gl/PdiFnz


Nos iremos al final del fichero y veremos esto


(imagen) https://goo.gl/xPvWAA


y añadiremos las direcciones de los proxies con el siguiente formato:

socks5 127.0.0.1 9050 y deberia de quedar de esta manera…


(imagen) https://goo.gl/PymMoG


Apretamos Ctrl+o (Confirmamos la modificacion del fichero con “Y” o “S” y luego volvemos a consola con Ctrl+x

Antes podriamos probar instalando la ultima version de Tor, sitienes algun problema lo instalamos de manera manual en consola de esta manera:

<wget https://dist.torproject.org/torbrowser/7.5a4/tor-browser-linux32-7.5a4_ar.tar.xz>

*(Aca puedes buscar las ultimas actualizaciones de manera manual)
https://dist.torproject.org/torbrowser/


pondremos en consola el siguiente comando para probar el status de Tor:

<service tor status>


si nos da el error “tor is not running” pondremos el siguiente comando:

<service tor start> 

<https://goo.gl/CFfNp6>

ya estamos ok para trabajar por consola.


Para ejecutar un programa con acceso a Internet usando nuestros proxies, usaremos el comando “proxychains”. Ejemplos:

<proxychains nmap>

<proxychains firefox>

<proxychains ping http//duckduckgo.com>



by Yakóv
