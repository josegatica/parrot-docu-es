Disclaimer: realizado para saciar la curiosidad, no para realizar acciones ilegales.

# FASE 0: Ocultación

> This is a repository to provide documentation on anonymity.

## Importancia

Nuestra vida avanza y la sociedad en cierto modo nos presiona para hacer uso de la tecnología. Actualmente resulta dificil conocer a un pariente cercano que no haga uso de un dispositivo móvil o de un ordenador. Dicha tecnología crece de forma exponencial, lo cual provoca que cada vez interactuemos más con ella. Es esta interación la que facilita la identificación y recolección de información sobre nuestros hábitos, gustos, opiniones, … Sobre todo nuestro ser.

La tecnología va adentrandose más y más en nuestras vidas hasta llegar al punto de situarse dentro de lo personal. Hemos permitido que puedan conocer nuestros hábitos, nuestros gustos, nuestros ideales e incluso nuestras opiniones.

Sin darnos cuenta hemos pasado de impedir que desconocidos puedan acceder a nuestros hogares a dejarles abiertas las puertas de par en par... Y todo porque no nos percatamos de su presencia. Internet es un gran amigo, pero un gran amigo insvisible a través del cual pueden controlar toda nuestra vida.

Puede que en tiempos pasados pudieramos refugiarnos en el descontrol y el caos… Los tiempos cambian, y al igual que las grandes corporaciones, las agencias y nuestro gran amigo y vecino el cracker se adapta, debemos hacerlo nosotros también.

Muchos paises ya han considerado el ciberespacio como la quinta dimensión en cuestión de riesgos para la seguridad nacional. Para quien no lo sepa, estan las dimensiones del mar, tierra, aire, espacio y ahora… El ciberespacio.

Si bien es cierto como comentan algunos si seguimos una ética correcta, todo es seguro. Si no tocas es que no tiene ningún fallo…

Dejemonos de ser unos ilusos. Dejemos de estar dormidos. Despertemos. Preocupemonos por nuestra seguridad. Por la seguridad de quienes nos rodean. 

Es el momento de marcar un antes y un después. Es el momento de demostrar que sabemos adaptarnos a esta sociedad sin ser deborados por ella. 


## Dirección MAC

### ¿Qué es?
La  dirección MAC es una identificador único a nivel mundial por cada dispositivo formado por 48 bits (6 bloques de dos caracteres hexadecimales (4 bits)) que permite identificar la totalidad de dispositivos de red tales como las tarjetas de red.

Normalmente los fabricantes una vez tienen montado el hardware, vease una tarjeta de red, graban en formato binario en una memoria ROM del dispositivo la dirección MAC. Dicha memoria es de solo lectura, lo cual imposibilita poder modificarla directamente en la ROM.

Según lo anterior no podemos modificarla. Cabe destacar que lo que no podemos modificar es directamente la MAC que tenemos almacenada en la ROM. Entonces… ¿qué podemos modificar?Al arrancar el sistema se carga una copia de la dirección MAC en nuestra memoria RAM. En este caso, lo que podemos hacer es modificar la dirección MAC que esta almacenada en la RAM. Dicha dirección será la usada en todas las acciones donde se precise su uso. 


### Estructura
Anteriormente habiamos mencionado que la dirección MAC es un identificador único a nivel mundial por cada dispositivo. Estos identificadores tienen una entidad que regula unas normas de marcado. Dicha entidad es el IEEE (Instituto de Ingenierí Eléctrica y Electrónica).

Una dirección MAC tiene el siguiente formato:
*40:AF:E5:F5:FE:50*

Debemos centrarnos principalmente en dos partes:

* Por un lado tenemos la primera parte, el lado izquierdo, el cual consta de 3 bloques. Dicho lado identifica quien es el fabricante del hardware. Esto quiere decir que si en nuestra red vemos una dirección MAC que empieza por *40:AF:E5* esta pertenecerá a Sawyer Technologies.

* La segunda parte, la de la derecha, consta también de 3 bloques. Esta parte del código es el número de serie que identifica el dispositivo fabricado. El fabricante puede usar el número de serie que quiera pero con la condición que únicamente puede usar el mismo número de serie una vez. Es decir, los 3 últimos bloques deben ser únicos para un único dispositivo. Si se fabricase otro no debe poder usar la terminación *F5:FE:50*.


### Usos de la dirección MAC
1.- La dirección Mac de una tarjeta de red la podemos utilizar para que nuestro Router asigne una IP estática a nuestro ordenador.

2.- Poder filtrar por MAC en nuestra red de tal forma que solo puedan acceder a ella las MACs que hayamos especificado.

3.- Como hemos mencionado antes la dirección Mac puede servir para identificar a un equipo o usuario dentro de una red informática. Usando Nmap (`nmap -sP <dirección IP>`) o NACC por ejemplo podemos obtener fácilmente las IP y las Mac Address de los equipos conectados a la red.

4.- Nuestro teléfono móvil con la red Wifi activada tiende a preguntar a los diferentes routers si estan activos. Dicha “pregunta” hace reflejar nuestra dirección MAC. Recordemos que nuestra dirección MAC es una dirección única.

5.- Hay proveedores de Internet que requieren Autenticación MAC para proporcionar internet a sus clientes.

6.- Si estamos en una red y somos los únicos con un dispositivo de la marca Apple, claramente se sabrá a quien pertenece dicho ordenador dentro de un establecimiento o local.


### Modificación de la dirección MAC
Hagamos pie en que la dirección MAC es una dirección única que se le da a nuestro dispositivo y que este dispositivo puede hacer uso de ella siendo visible para otros usuarios.

#### ¿Cómo se modifica una dirección MAC?
Hay varias formas y programas para realizar dicha modificación. El más comunmente usado es (https://github.com/alobbs/macchanger macchanger ), disponible en nuestra distribución Parrot. 

A continuación veremos una forma fácil de cómo se hace dicha modificación sin el uso de herramientas:

> Ejecutando ifconfig podremos ver nuestras diferentes redes. Por ejemplo, un nombre comunmente usado es el de la red ethernet eth0. En el lado izquierdo de la respuesta dada al comando ifconfig podremos ver los nombres.


Lo primero que deberemos hacer es deshabilitar la tarjeta de red a la cual vamos a cambiar la dirección MAC:
`ifconfig <nombre de red> down`

A continuación modificamos la MAC de dicha tarjeta por la que queramos:
`ifconfig <nombre de red> hw ether 00:00:00:00:00:00`

> Normalmente en lugar de *00:00:00:00:00:00* se suele hacer uso de direcciones MAC existentes provenientes de bases de datos que circulan por la red. No es muy dificil acceder a dichas bases de datos.

Finalmente volvemos a arrancar de nuevo la tarjeta de red que habiamos deshabilitado:
`ifconfig eth0 up`


Con estos sencillos pasos se habrá modificado la dirección MAC. Podremos comprobar el cambio ejecutando ifconfig. Si nos fijamos en la interfaz de red que se deseó cambiar, esta deberá tener una dirección MAC diferente a la original.

> En algunos lugares se puede considerar ilegal la modificación de la dirección MAC.


## Tunel ssh
El protocolo SSH (secure shell) se utiliza para “tunelizar” tráfico confidencial sobre Internet de una manera segura. 

Por ejemplo, un servidor de ficheros puede compartir archivos usando el protocolo SMB (Server Message Block), cuyos datos no viajan cifrados. Esto permitiría que una tercera parte, que tuviera acceso a la conexión (algo posible si las comunicaciones se realizan en Internet) pudiera examinar a conciencia el contenido de cada fichero trasmitido.

Para poder montar el sistema de archivo de forma segura, se establece una conexión mediante un túnel SSH que encamina todo el tráfico SMB al servidor de archivos dentro de una conexión cifrada SSH. Aunque el protocolo SMB sigue siendo inseguro, al viajar dentro de una conexión cifrada se impide el acceso al mismo.

Por ejemplo, para conectar con un servidor web de forma segura, utilizando SSH, haríamos que el cliente web, en vez de conectarse al servidor directamente, se conecte a un cliente SSH. El cliente SSH se conectaría con el servidor tunelizado, el cual a su vez se conectaría con el servidor web final. Lo atractivo de este sistema es que hemos añadido una capa de cifrado sin necesidad de alterar ni el cliente ni el servidor web.

Es decir, ¿cual es la idea? La idea es realizar una conexión que vaya cifrada haciendo que esta, aún siendo interceptada, sea segura impidiendo poder visualizar su contenido.

El tener una conexión cifrada impide que alguien pueda esnifar el tráfico y pueda visualizar el contenido de nuestras peticiones a la red (que pueda ver, por ejemplo, las páginas que visitamos). También impida que en caso de estar bajo un ataque man in the middle el atacante pueda modificar los paquetes (al estar cifrado un atacante se vería en una gran dificultad para realizar la modificación de los paquetes). Hay otros ataques de los cuales podemos sentirnos seguros haciendo uso de un tunel ssh como pueden ser ARP o DNS Spoofing

Para establecer un túnel SSH necesitaremos un ordenador fuera de nuestra Red local en que la seguridad está comprometida, que actúe como servidor SSH. 

En el momento que el cliente establezca comunicación con el servidor SSH se creará un Túnel de comunicación entre el cliente que está en la red local comprometida y el servidor SSH que está fuera de la red local comprometida.

Cuando el cliente introduzca una dirección en el navegador se enviará una petición http al servidor SSH con las siguientes particularidades:
	- La petición http viajará por el túnel SSH que hemos establecido.
	- Nuestra petición, y la totalidad de tráfico entrante y saliente estará completamente cifrado.

Los túneles SSH son una buena solución para asegurar la comunicación entre 2 máquinas y para fortificar protocolos débiles como por ejemplo HTTP, SMTP. FTP, Telnet, etc.

Montarse un servidor SSH propio es mucho más sencillo que montarse un servidor VPN o un servidor proxy. Recordemos que las cosas hechas por uno mismo son más seguras...


### Estableciendo un tunel ssh
El primer paso es disponer de un servidor al cual nos conectaremos por ssh. Si no disponemos de uno podemos crearnos uno nosotros mismos en nuestra propia casa ya sea haciendo uso de un ordenador personal que se quedará en casa o de una raspberry.

Deberemos asegurarnos de que nuestro ordenador dispone de un servidor ssh instalado. En caso de no tenerlo, bastaría con ejecutar por terminal: sudo apt-get install openssh-server openssh-client

> Se necesitan ambas partes instaladas en cada ordenador, tanto cliente como servidor.

Si queremos adaptar la configuración del servidor a nuestros requerimientos, se debe modificar el archivo sshd_config situado en la ruta /etc/ssh/sshd_config ( comando: `sudo pluma /etc/ssh/sshd_config` ).

Nuestro ordenador, el cual actuará de servidor, debe ser accesible desde fuera. Si no es accesible desde el exterior no podríamos crear el tunel ssh. En caso de disponer de una dirección IP fija no habría más problema que conocer la dirección IP. Anonsurf nos trae una herramienta que nos indica cual es nuestra dirección IP (“check IP”). Suele ser común que nuestro proveedor de red nos asigne una dirección IP dinámica.


Lo siguiente que deberemos hacer una vez tengamos seguro la dirección IP pública de nuestro servidor, redireccionar las peticiones del router a la dirección IP interna del servidor ssh.

Observamos la dirección IP interna del servidor. Podemos obtenerla haciendo uso del comando `sudo ifconfig`. 

> Dicha dirección IP también debe ser fija. Por lo normal mientras este conectado al router dicha dirección IP no cambiará.

A continuación deberemos acceder a la configuración del router. Normalmente vale con escribir 192.168.1.1 en el navegador. Dentro de la configuración del router deberemos buscar un apartado que ponga Virtual Servers. Accediendo a dicho apartado deberemos presionar el boton añadir (*Add en inglés*). Deberemos completar con la dirección IP interna, con la opción *Secure Shell Server (SSH)*, con el protocolo *TCP* y el puerto *22*.

Una vez esten todos los campos completados guardar los cambios.

> Cabe advertir que en caso de disponer de un firewall en el servidor, deberemos crear una regla que permita conexiones entrantes y salientes por el puerto 22.

Con iptables solo habría que ejecutar los comandos:
`iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT`
`iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT`

*-A para indicar entrada o salida*
*-dport para el puerto de destino del paquete*
*-sport para la fuente del paquete*
*-o para la interfaz de red de salida*
*-i para la interfaz de red de entrada*
*-m state –state para habilitar el modulo state con los respectivos estados escritos*
*-j para aceptar el objetivo*


Hasta aquí ya tendriamos todo montado. 

### Realizar la conexión
Lo primero de todo asegurarse que el cliente, ordenador desde el que nos conectamos al servidor que hemos creado, tiene instalado el paquete openssh-client. 

Una vez tengamos todo comprobado, para realizar la conexión ejecutaremos: 
ssh -p 22 -N -D 8081 sawyer@parrotsec.org

*-p para establecer el puerto de escucha*
*-D especificamos que se realice un reenvio dinámico de puertos o tunel dinámico*
*-N permite que se establezca el tunel ssh pero que no se abra una sesión interactiva con el servidor ssh*
*sawyer es la cuenta a la que nos conectamos*
*parrotsec.org es la dirección del server al que nos conectamos*

### Configurando el navegador

Deberemos acceder a las opciones de conexión a través de un proxy. Como el servidor proxy socks actuará localmente en nuestro ordenador en el apartado Host or IP address tenemos que introducir 127.0.0.1. Como el tunel SSH lo hemos abierto a través del puerto 8081 en la celda puerto tenemos que introducir el número de puerto 8081. Para finalizar tan solo tenemos que tildar la opción ¿proxy socks? y seleccionar la opción Socks v5 ya que la versión 5 del protocolo socks es más moderna y ofrece mayor seguridad que la versión 4.

Una vez realizados todos los pasos presionamos el botón Aceptar. Cerramos el navegador y lo volvemos abrir de nuevo.


> En caso de caer el servidor se nos cae a nosotros también la conexión a través del navegador.

Podemos hacer uso para la configuración del apartado de opciones de conexión a través de un proxy de herramientas como FoxyProxy (en el caso de Firefox) o Proxy Switchy (en el caso de chromium).

En caso tener aplicaciones que no soportan socks podremos hacer uso de tsocks.



## Proxy
Dicho de forma simple:  es una máquina a través de la cueal podemos conectarnos a servidores web, haciendo pensar a estos que somos dicha máquina.

Cuando estamos navegando a través de un proxy, lo que estamos haciendo es hacer peticiones al servidor proxy. Luego este es el encargado de hacer la petición al servidor al que queremos conectarnos. Se podría decir que un proxy es un intermediario.

> Solo se van a comentar servidores http proxy o proxy web. Existen otros servidores proxy como por ejemplo los servidores Proxy socks o Forward Proxy.

### Ventajas
La principal ventaja es ocultar información a la gente que nos está rastreando. Mediante el uso de un proxy no somos anónimos, solo desviamos la atención.

Nuestra dirección IP puede señalarnos en el mapa, literalmente. Usando un proxy cuya dirección IP procede de un servidor en EEUU, el servidor al que nos conectamos pensará que nos conectamos desde EEUU. 

Para consultar toda la información que se puede obtener a través de nuestra dirección ip podemos visitar páginas como: http://xhaus.com/headers

Puede parecer una chorrada, pero al proporcionar datos como la versión del navegador estamos dando a un posible atacante los puntos por donde puede atcarnos.


### Cómo conectarse a un proxy
Buscar un servidor proxy al que conectarnos: tan solo con buscar en google “proxy list” nos aparecerán resultados. Algunos de los encontrados son www.ip-adress.com/proxy_list/ o http://www.hidemyass.com/

#### Elegir bien el tipo de proxy:
- **Proxy Transparante:** este tipo de proxy proporciona la totalidad de datos mencionados en el apartado anterior al servidor web al cual nos conectamos. Es decir, nuestra IP será visible en todo momento.

- **Proxy Anónimo:** si queremos usar un proxy solo para navegar es más que suficiente para nosotros. Este tipo de proxy enmascara la variable http_x_forwarded_for impidiendo dar nuestra dirección IP.  Pese a no poder saber nuestra dirección IP si puede saberse que estamos realizando la conexión a través de un proxy.

- **Proxy Elite:** este proxy enmascara la variable http_x_forwarded_for, a la vez que otras otras variables como pueden ser la http_via y http_proxy_connection. Estaremos falseando la totalidad de información que entregamos al servidor web y además no podrán detectar que nos estamos conectando a través de un servidor proxy.

Datos que necesitamos saber para conectarnos: dirección IP y puerto

Cómo conectarnos a través del navegador: cada navegador se configura de una distinta forma. Tan solo debemos usar nuestro buscador y consultar la guia de configuración. Normalmente tiende a aparecer en configuración acanzada – conexiones. Siempre se necesitará la dirección IP del servidor al que nos vamos a conectar y el puerto a través del cual vamos a realizar la conexión.

### Problemas
- Cada aplicación debe configurarse para que los datos pasen a través del proxy
- Tiende a tener una lenta conexión
- No todo lo gratis es de confiar ni todo lo de pago es bueno.



## Encadenamiento de proxys
Comunmente, al pensar que si tras un proxy ocultamos nuestra identidad, tendemos a considerar la opción de hacerlo tras varios proxys. Dicha opción es posible mediante el uso de herramientas como proxychains.

### ¿Qué es Proxychains?
Una herramienta que redirige la totalidad de las peticiones TCP y resoluciones DNS de la aplicación por nosotros a través de una lista de proxys establecidos por nosotros

### Instalación
En nuestra distribuciópn parrot viene instalado por defecto. 

### ¿Cual es la idea?
Nosotros, ordenador A, queremos realizar una conexión directa con el servidor B. Dicho servidor B, al ser una conexión directa, conocerá nuestra dirección IP y sabrá que nos conectamos desde España. 

Ahora bien, nosotros vamos a hacer uso de un proxy, llamado C, situado en Marruecos. Desde A nos conectamos al servidor C, nuestro proxy, y C realiza una conexión con B. En el actual momento B recibe una petición desde Marruecos.

Hasta aquí todo normal. Bien, ahora digamos que queremos meter 3 proxys antes de conectarnos con B. Dichos proxys los llamaremos D, E, F y pertenecen a Dinamarca, Estonia y Francia respectivamente. Desde A, nuestro ordenador en España, nos conectaremos a D, situado en Dinamarca, desde D nos conectaremos a Estonia, es decir, al ordenador E, luego nos conectaremos desde E al ordenador F situado en Francia, por último, desde Francia haremos una conexión al ordenador B situado en Marruecos. Ya finalmente será desde este punto desde el cual se realizará la petición al servicor B, el cual tiene la página web que queriamos consultar. El rastro de la conexión va desde España, pasando por Dinamarca, Estonia, Francia y Marruecos. El servidor B recibirá una petición de Marruecos, quien la recibió de Francia, quien la recibió de Estonia, quien la recibió de Dinamarca, quien la recibio desde nosotros situados en España.

### ¿Es localizable?
Por supuesto, siempre puede localizarse a una persona. Lo malo viene cuando has de llamar al proveedor que realiza el servicio en Marruecos, que este te diga el provedoor de Francia, llamar al proveedor de Francia, que este te diga el proveedor de Estonia, etc. Todo con la dificultad de la llamada y el tiempo que requiere.

> Ojo. Podemos pensar que es una forma ideal de conexión. No todos los proxys son seguros. Algunos guardan nuestra información para luego después venderla. No todo podía ser perfecto.

### Pegas
Si ya de por si teniamos baja conexión con 1 solo proxy, cabe esperar una gran bajada al encadernar varios.

### Configuración
Primero debemos acceder al fichero de configuración situado en /etc/proxychains.conf a través de cualquier editor y mediante el uso del comando sudo: sudo nano /etc/proxychains.conf

Lo primero de todo deberemos seleccionar el tipo de cadena que queremos usar. Tenemos 3 tipos:

**Strict:** seguirá exactamente la lista de proxy escrita en el archvio. Si uno solo falla, se pierde la conexión. Esta opción es buena en caso de que nuestros proxys sean de fiar y queramos desconectarnos en caso de que alguno falle.

**Dinamic:** mismo funcionamiento que strict. Sigue uno a uno la lista de proxys. Se diferencia en que, en caso de fallar uno, pasa al siguiente.

**Random:** se conectará de forma aleatoria a la lista de proxys asignada. Esto sirve para modificar cada dirección betada del sistema.

Para seleccionar la opción que queremos basta con descomentar la lína. En el siguiente ejemplo estaremos haciendo uso del tipo strict:
*#dynamic_chain*
*strict_chain*
*#random_chain*

> Importante: línea de resolución de dns descomentada para asegurarnos que la resolución de DNS se haga a través de los servidores proxy.

Finalmente deberemos introducir al final del archivo la lista de proxy que vayamos a usar. Para escribir cada proxy se debe hacer uso de la estructura indicada en el archivo:






*# ProxyList format*
*#       type  host  port [user pass]*
*#       (values separated by 'tab' or 'blank')*
*#*
*#*
*#        Examples:*
*#*
*#            	socks5	192.168.67.78	1080	lamer	secret*
*#		http	192.168.89.3	8080	justu	hidden*
*#	 	socks4	192.168.1.49	1080*
*#	        http	192.168.39.93	8080	*
*#*		
*#*
*#       proxy types: http, socks4, socks5*
*#        ( auth types supported: "basic"-http  "user/pass"-socks )*
*#*
*[ProxyList]*
*# add proxy here ...*
*# meanwile*
*# defaults set to "tor"*
*socks4 	127.0.0.1 9050*

La última linea que indica “defaults set t
