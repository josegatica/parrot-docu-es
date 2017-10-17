# Scripting

## Hola Parrot

NOTA: Los comandos que se muestran están pensados para escribirlos en un archivo de texto y no en la terminal, a no ser que se indique lo contrario.

Bash es principalmente un lenguaje de scripting, aparte de una shell. Vamos a introducirnos en el maravilloso mundo de scripting, comenzando por el consabido script "Hola Mundo". Usted puede crear scripts simplemente abriendo su editor de texto favorito y guardándolo. Aunque no es necesario que nuestros scripts tengan una extensión de archivo, generalmente se utiliza .sh como referencia. En nuestros ejemplos usaremos .sh

	#!/bin/bash         
	#Script hola ParrotSec
	echo "Hola ParrotSec"




En la primera línea del script simplemente definimos el intérprete a utilizar. NOTA: No hay espacio antes de  #!/bin/bash.   
En la segunda línea podemos ver un comentario. Cualquier cosa que empiece por '#', salvo '#!' que apareció en la primera línea, será tomado por el intérprete como un comentario y no se ejecutará. Acostúmbrese a escribir sus scripts con estos comentarios, para explicar lo que va haciendo y posteriormente pueda revisar su código de una manera más fácil.   
En la tercera línea utilizamos la instrucción "echo" para mostrar un texto por pantalla. En nuestro caso "Hola ParrotSec".   
Guardamos el script en la ubicación que deseemos con el nombre "hola_parrot.sh", o cualquier otro que deseemos.   

Y eso es todo. Muy sencillo verdad?  


Para poder ejecutar el script, el archivo debe tener los permisos correctos. Para cambiar los permisos utilizaremos la instrucción "chmod" (change mode) así:  

	$ chmod u+x /Path/donde/este/el/archivo/hola_parrot.sh   #Añade permiso de ejecución al usuario propietario del script
	# O
	$ chmod 700 /Path/donde/este/el/archivo/hola_parrot.sh   #Otorga control total al usuario propietario, eliminando el acceso al resto de usuarios

Esto nos dará los permisos necesarios para poder ejecutar el script. Ahora puede abrir un terminal y ejecutar el script de la siguiente manera:

	$ /Path/donde/este/el/archivo/hola_parrot.sh

Si todo es correcto podrá ver el texto "Hola Parrotsec" por pantalla. Felicidades!!! Acaba de crear su primer Bash script.

CONSEJO: Si escribe en la terminal

	$ pwd

Podrá ver el directorio en el que usted está trabajando (pwd es un comando que le muestra la ruta en la que está situado. Si su path actual es /Path/donde/este/el/archivo/, el comando anterior podría reducirse de la siguiente forma:

	$ pwd
	/Path/donde/este/el/archivo
	$ ./hola_parrot.sh



Es el momento de ver cosas más interesantes, Variables!


## Variables

Las variables, básicamente, guardan información que puede variar (o no). Quedémonos con el dato de que guardan información.
Usted puede crear y asignar valores a una variable de la siguiente forma:

	var="PARROT"

El nombre de la variable puede ser cualquier cadena de texto sin espacios, siempre y cuando no comience por un número. A esta variable (en nuestro caso la hemos llamado 'var') le podremos asignar cualquier cadena de texto o número.

Para poder extraer el contenido de una variable simplemente utilizaremos el nombre de dicha variable precedida por el símbolo "$", como indicamos en el siguiente ejemplo:

	var="PARROT"
	echo $var

Escriba las dos líneas anteriores en un terminal. Verá que la primera línea no devuelve nada más que el prompt. Al pulsar enter, tras introducir la segunda línea, el sistema escribirá en pantalla PARROT.

Creemos un script que nos solicite información para mostrarla por pantalla.

	#!/bin/bash
	clear
	echo "Introduzca su nombre:"
	read nombre
	echo "Introduzca su distribución favorita:"
	read distribucion
	echo "Introduzca la marca de su PC/Laptop:"
	read PC
	echo "$nombre!! no sería fabuloso instalar $distribucion en su equipo $PC?"

La instrucción 'read' le permite al usuario introducir información, y guardar dicha información en una variable con el nombre definido después de 'read'.  'read' tomará la cadena introducida, para guardarla en $variable. Podemos acceder a su contenido mediante la instrucción 'echo' para así formar una frase.  Vamos a realizar unos cambios en nuestro script:

	#!/bin/bash
	clear
	read -p "Introduzca su nombre: " nombre
	read -p "Introduzca su distribución favorita: " distribucion
	read -p "Introduzca la marca de su PC/Laptop: " PC
	echo "$nombre!! no sería fabuloso instalar $distribucion en su equipo $PC?"


## Estructuras de control: condicional (if)
La estructura condicional se puede utilizar para ejecutar algo en función de un resultado dado que comprobaremos o realizar otra acción en el caso de que no se haya producido dicho resultado. Por ejemplo, podríamos consultar una variable y comprobar si su valor es 'PARROT', en el caso de que fuese así podríamos mostrar un texto y si fuese cualquier otro valor, mostraríamos otro texto diferente.

El formato para construir estructuras de control condicionales es:

	if [condicion]
        then
		comandos

       	elif [condicion]
       	then
		comandos
	else if [condicion]
	then
		comandos 

      	else
		comandos
	fi

Las líneas  else if, else o  elif no son estrictamente necesarias, pero se podrá utilizar si se desea.  
Es importante cerrar las estructuras de control condicionales para indicar que se han terminado las instrucciones de esta estructura. Para ello utilizaremos "fi".  
Veamos un ejemplo.  

Comprobemos el valor introducido por un usuario y mostremos diferentes textos en función de dicho valor:

	#!/bin/bash
	echo "Introduzca el nombre de su distribución favorita:"
	read distribucion

	if [ $distribucion = parrot ]
        then 
		echo "A todos nos gusta ParrotSEC"
        else 
		echo "$distribucion está bien. Pero pruebe Parrot"
	fi

En el ejemplo anterior, en la estructura de control:

	si [ el contenido de distribucion es 'parrot'  ]
       	entonces 
		di "A todos nos gusta ParrotSEC"
        si no 
	entonces 
		di "$distribucion está bien. Pero pruebe Parrot"
	fin

La estructura condicional es un concepto sencillo ya que se parece mucho al concepto condicional que utilizamos al hablar, utilizando "si [esto] haz instrucción". Las estructuras condicionales se pueden encadenar. Piense en otro ejemplo. Añadamos una condición más a nuestro ejemplo anterior. Supongamos que queremos controlar si el usuario introduce la distribución "Debian". Es decir, si introduce "parrot" mostramos el texto ya conocido y si es cualquier otra cosa, mostraremos el texto correspondiente. Pero ahora queremos controlar la salida de texto para la opción "Debian".

Veamos dos formas de hacerlo y elija la que más le guste (solo mostraré cómo queda la estructura de control y no todo el script):

	if [ $distribucion = parrot ]
	then
		echo "A todos nos gusta ParrotSEC"
	else
		if [ $distribucion = debian ]
		then
			echo "Debian es el maestro"
		else
			echo "$distribucion está bien. Pero pruebe Parrot"
		fi
	fi

Otra forma:

	if [ $distribucion = parrot ]
	then
		echo "A todos nos gusta ParrotSEC"
	elif [ $distribucion = debian ]
	then
		echo "Muy bien por Debian, intente instalar Parrot"
	else
		echo "$distribucion está bien. Pero pruebe Parrot"
	fi 
			

Esta última forma nos permite escribir de forma sencilla condicionales que bien podrían ir encadenadas (ejemplo 1), pero que requerirían mayor atención, sobre todo en el cierre de estas condiciones (fi). Hay otras formas de tener el control de condiciones múltiples pero hablaremos más adelante de esto.  



Hemos visto cómo podemos controlar condiciones sobre cadenas de texto, pero podemos ejecutar estructuras de control condicional sobre valores alfanuméricos.  
Podríamos utilizar condiciones del estilo "Si variable es mayor o igual que un número dado, resta 1 a variable", "Si una variable es menor que un valor dado, suma 3 a variable2",...  

Ejemplo:  

	#!/bin/bash
	echo "Introduzca un valor:"
	read valor
	if [ $ valor -ge 5 ]
	then 
		echo "El valor introducido es mayor o igual a 5"
	else
		echo "El valor introducido es menor de 5"
	fi




La condición dada y traducida al "español" sería la siguiente: "si el valor de $valor es mayor o igual a 5". ge significa "Greater or Equal than" (mayor o igual que).

Las siguientes tablas están sacadas del man de test (man test):


	##Operadores para cadenas de texto 
	Operador		Verdad (TRUE) si:
	------------------------------------------
	cadena1 = cadena2	las cadenas son iguales
	cadena1 != cadena2	las cadenas no son iguales
	-n cadena		la longitud de cadena no es 0
	-z cadena		la longitud de cadena es 0



	##Operadores para valores alfanuméricos
	Operador		Verdad (TRUE) si:
	------------------------------------------
	x -lt y			x menor que y
	x -le y			x menor o igual que y
	x -eq y			x igual que y
	x -ge y			x mayor o igual que y
	x -gt y			x mayor que y
	x -ne y			x no igual que y



	##Operadores para ficheros
	Operador		Verdad (TRUE) si:
	------------------------------------------
	-d fichero		fichero existe y es un directorio
	-e fichero		fichero existe
	-f fichero		fichero existe y es un fichero regular (no un
				directorio, u otro tipo de fichero especial)

	-r fichero		Tienes permiso de lectura en fichero
	-s fichero		fichero existe y no está vacío
	-w fichero		Tienes permiso de escritura en fichero
	-x fichero		Tienes permiso de ejecución en fichero (o de búsqueda
				si es un directorio)

	-O fichero		Eres el dueño del fichero
	-G fichero		El grupo del fichero es igual al tuyo.

	fichero1 -nt fichero2	fichero1 es más reciente que fichero2
	fichero1 -ot fichero2	fichero1 es más antiguo que fichero2



Existen más operadores. Uno para combinar diversas condiciones '-a'(AND) y '-o'(OR). Y '!'  para negar una condición.

Veamos unos cuantos ejemplos. Por favor, lean los comentarios de los scripts.

Ej. 1:  

	#!/bin/bash
	#
	# En este ejemplo comprobaremos cadenas
	#
	
	#Solicitamos al usuario que introduzca dos textos que guardaremos en variables texto1 y texto2
	read -p "Escriba un texto:" texto1
	read -p "Escriba otro texto:" texto2

	#Comprobamos si alguna cadena de texto es vacia (texto1 es vacío o texto2 es vacío)
	#Preste atención al entrecomillado para que realmente sea considerado texto
	if [ -z "$texto1" -o -z "$texto2" ]; then
		#Si entra en esta condición, no se necesita continuar con el script
		#Por lo que provocamos una salida del mismo, con un valor 1. Hablaremos de esto en algún punto posterior.
		#De momento debemos saber que exit finaliza el script y que el valor 1 es el código de retorno del script, porque así lo hemos indicado
		echo "Uno de los textos está vacío"
		exit 1
	fi

	#Comprobamos si texto1 es igual a texto2
	if [ $texto1 = $texto2 ]
	then
		echo "$texto1 es igual a $texto2"
	#Podríamos haber escogido simplemente else, y ahorrarnos la definición de la condición
	elif [ $texto1 != $texto2 ]; then
		# \" se utiliza para escapar las '"'. Es decir, el intérprete debe saber que esas comillas no son la terminación del comando echo, sino
		# qué debe mostrar esas comillas dentro de la cadena.  
		echo "El texto \"$texto1\" y el texto \"$texto2\" no es igual"
	fi

Anexo 1 Ej. 1: echo y secuencia de escape

Trate de escribir en una línea de comandos:

	$ echo "hola Parrot"

Pero... ¿cómo escribimos un comando echo para que muestre por pantalla hola "ParrotSec"? 
"echo" aunque tiene diversas opciones y podemos utilizar otros símbolos para indicar el principio y el fin de la cadena de texto a mostrar, generalmente se usa con las ". Para poder mostrar una cadena con " debemos "escapar" las dobles comillas.

Mire el siguiente ejemplo:

	$ echo "hola "Parrot""

Pues bien, para que las comillas intermedias sean parte de la cadena a mostrar debemos escapar dichas comillas. Esto se hace anteponiendo un símbolo '\' al carácter especial. Esto es muy utilizado para otros comandos también. Siguiendo con nuestro ejemplo, el comando quedaría de la siguiente forma:  

	$ echo " hola \"Parrot\"  "

Se han añadido varios espacios en la cadena a mostrar, tanto al principio como al final, simplemente para que se vea más claro el ejemplo.	 




Ej. 2:

	#!/bin/bash
	#
	## Comparación de valores numéricos
	#

	read -p "Introduce el primer número:" NUM1
	read -p "Introduce el segundo número:" NUM2
	read -p "Introduce el tercer número:" NUM3


	# Utilizar '&&' es lo mismo que utilizar el operador '-a'
	if [ $NUM1 -ne $NUM2 ] && [ $NUM1 -ne $NUM3 ]; then
		echo "$NUM1 es diferente a $NUM2 y $NUM3"
	fi

	# Utilizar '||' es lo mismo que utilizar el operador '-o'
	if [ $NUM1 -gt $NUM3 ] || [ $NUM1 -gt $NUM2 ]; then
	        echo "$NUM1 es el número más grande de los 3 introducidos"
        fi




## Estructuras de control: condicional (case)
Podríamos considerar la estructura de control "CASE" como un formato condicional parecido al "IF". Dado un resultado de una variable, seleccionaremos unas instrucciones concretas.
El formato para construir estructuras de control CASE es:

	case $variable in
     		caso_1 )
        		comandos;;
     		caso_2 ) 
			comandos;;
  		......
	esac  



Ej. 1:

	case "$1" in
		start)
			echo "Opción pasada: Start"
			;;
		stop)
			echo "Opción pasada: Stop"
			;;
		*)
			echo "Uso: $0 {start|stop}"
			exit 1
			;;
	esac

Anexo Ej. 1:   
El valor de $1, es la primera opción pasada a nuestro script. Es decir, si nuestro script se llama "miscript.sh", podríamos ejecutar "./miscript.sh opcion1"y $1 sería igual a opcion1.


Ej. 2:

	read opcion
	case $opcion in
		s|S)
			echo "Se ha pulsado Si"
			;;
		n|N)
			echo "Se ha pulsado No"
			;;
		*)
			echo "No es una opción contemplada"
			;;
	esac



## Estructura de control: condicional (SELECT)
Esta estructura es especial y el sueño de muchos programadores. Es la forma más sencilla de crear un menú.

La forma que tiene esta estructura es:

	select nombre in opcion1 opcion2 opcion3
	do
  		comandos
	done



Ej. 1:

	#!/bin/bash
	select OPCION in parrot debian otros salir
	do
		case $OPCION in
			parrot)
				echo "Selecciono usted: $OPCION"
				;;
			debian)	
				echo "Selecciono usted $OPCION. Esta preparado para utilizar ParrotSec"
				;;
			otros)
				echo "Hágase un favor a usted mismo e instálese Parrot"
				;;
			salir)
				echo "Hecho"
				exit
				;;
		esac
	done


Anexo Ej. 1:   
Como puede observar en este ejemplo, hemos encadenado dentro de nuestra estructura "select" otra estructura de tipo case. 



## Estructura blucle: FOR
Para acciones repetitivas se han creado estructuras en forma de bucle. Hay varios tipos. En este primer lugar veremos bucles for.    
El bucle es una lista de comandos que se realiza de forma repetitiva.    
En el caso de FOR, este bloque se repetirá mientras tengamos valores en la lista que estamos comprobando.   
La forma que tiene esta estructura es la siguiente:   

	for variable [in lista]
	do
		ejecucion
	done


Ej. 1:

	for i in 1 2 3 4 5
	do
		echo "Cuenta $i"
	done


Ej. 2:  

	for i in {2..10..2}
	do
		echo "$i es par"
	do

Anexo Ej. 2:    
En versiones bash, anteriores a la V.3, se utilizaba normalmente un comando seq para sacar una lista de valores incrementales. En la versión 4 de bash se implementó una forma de crear estos valores incrementales dentro del propio bucle for.   
No es necesario llamar ya al programa externo "seq", ya que el built-in del for aplicado en este ejemplo es más rápido. "{Primer_numero..Ultimo_numero..Incremento}"   


Ej. 3:

	for (( c=1; c<=5; c++))
	do
		echo "Valor de c es : $c"
	done

Anexo Ej. 3:    
Este ejemplo de for, es igual (no es extraño ya que es heredado de él) al for del lenguaje de programación C.    

	Expresion1 = inicialización de variable   
	Expresion2 = condición de ejecución de bucle  
	Expresion3 = incremento de variable   





## Estructura bucle: WHILE
En esta estructura el bucle se ejecutará mientras se cumpla una condición.   
La forma que tiene esta estructura es:   

	while condicion
	do
		comando
		comando
		...
	done


Ej. 1   

	#!/bin/bash
	
	CONTADOR=1
	while [ $CONTADOR -le 10 ]
	do
		echo $CONTADOR
		((CONTADOR++))
	done
	echo "Terminado"


Ej. 2  

	while :
	do
		echo "Bucle infinito [pulse CTRL+C para terminar]"
		sleep 1
	done

Anexo Ej. 2:    
Si a continuación de while ponemos el símbolo ":", el bucle se ejecutará de forma ininterrumpida.


Ej. 3:

	while read linea
	do
		echo $linea
	done < archivo

Anexo Ej. 3:    
Se utiliza esta estructura para leer un archivo línea a línea. "archivo" es un nombre de archivo que nuestro bucle leerá línea a línea.




## Estructura bucle: UNTIL
El bucle until continúa ejecutando comandos mientras se cumpla la condición. Una vez que dicha condición sea falsa, se sale del bucle.
La forma que tiene esta estructura es:  

	until condicion
	do
		comando
		comando
		...
	done 


Diferencias de bucle until y while:    
	1- El bucle until se ejecuta mientras la condición retorna un valor "nozero".   
	2- El bucle while se ejecuta mientras la condición retorna un valor "zero".   
	3- El bucle until siempre se ejecuta por lo menos una vez.    


Ej. 1:  

	#!/bin/bash
	i=1
	until [ $i -gt 10 ]
	do
		echo $i
		((i++))
	done


	


## Guardando la salida de un comando en una variable

En bash scripting, incluso en la shell, podemos asignar a una variable el resultado de un comando que ejecutemos. La forma elegante de hacerlo es con el siguiente formato: VARIABLE=$(comando).

Ejemplo    

	$ VARIABLE=$(who)
	$ echo $VARIABLE

Al ejecutar estos comandos, se nos mostrará por pantalla los usuarios conectados a cualquier terminal de nuestro sistema.      
Tenga en cuenta que esto elimina los saltos de línea. Acuérdese de que puede utilizar pipes "|" para filtrar el resultado. Como en el siguiente ejemplo:
	
	$ VARIABLE=$(who|awk '{print $1}')
	$ echo $VARIABLE

En este ejemplo hemos recogido, mediante awk, el primer campo de la salida de who. Es decir el/los nombre/s de usuario logeado/s en nuestro sistema.





## Redirigir salida de un comando a un fichero

Un comando tiene 2 punteros asociados a su salida por pantalla. La salida "1" o salida estándar y la salida de errores o "2". Ejecutemos 2 comandos "ls" para ver esto. En una terminal ejecute los siguientes comandos:

	$ ls -la /etc/passwd
	$ ls -la /nombre_no_existente

Al ejecutar el primer comando deberemos ver lo siguiente (o algo parecido):

	-rw-r--r-- 1 root root 3395 Sep 15 08:37 /etc/passwd	

Este resultado, al ser correcto, se ha mostrado en el terminal por la salida estándar o "1".  
En el segundo comando veremos un mensaje de error, ya que el archivo no existe.   

	ls: cannot access '/nombre_no_existente': No such file or directory

Para mostrar este resultado, el sistema ha direccionado ese error a la salida "2" o salida de errores.  

Podemos redirigir esa salida a un fichero si queremos (incluso al fichero especial /dev/null, el "agujero negro" o papelera sin retorno del sistema).  

Veamos varios ejemplos, abriendo una terminal:

	$ ls -la /etc/passwd 1> salida1.standard
	$ cat salida1.satnadard

Vemos que aparentemente el primer comando no ha mostrado nada por pantalla. Lo que hemos hecho es redirigir su salida estándar a un archivo salida1.txt.
El operador "1" se puede omitir si se desea. Podríamos haber escrito:  
	
	$ ls -la /etc/passwd > salida1.standard

Siendo exactamente igual. Esto sólo lo podemos hacer para la salida estándar, no podemos utilizarlo para la salida de errores.  

Veamos cómo podemos redirigir la salida de errores. Como ya hemos comentado, la salida de errores tiene un puntero representado con el número "2".   

	$ ls -la /nombre_no_existente 2> salida2.error
	$ cat salida2.error

En este caso, como en el ejemplo anterior, no vemos el error por pantalla ya que hemos redirigido esta salida ("2" por ser error) al archivo salida2.txt.   

Podemos, también, direccionar en una sola línea tanto la salida estándar como la de errores:   
	
	$ ls -la /etc/passwd /nombre_no existente 1> salida1.standar 2>salida2.error

Existen varias opciones para redireccionar las dos salidas a un archivo común:   
	
	$ ls -la /etc/passwd /nombre_no_existente 1> salida.txt 2> salida.txt

O también:

	$ ls -la /etc/passwd /nombre/no_existente > salida.txt 2>&1

En el caso "2>&1", estamos indicando al sistema que la salida de errores ("2"), se redirija al mismo lugar que a donde apunta la salida uno. Como primeramente hemos redireccionado la salida "1" (estándar) al archivo salida.txt, los mensajes de error también aparecerán en el mismo archivo. Podríamos traducir "2>&1" como "la salida de errores (2) redirígela (>) a donde apunte (&) la salida estándar (1)".  

Redirigiendo las salidas, bien sea la estándar como la de errores, a un archivo, este se generará vacío cada vez que el símbolo ">" aparezca. Veamos esto con un ejemplo, mediante un script que nos escriba la fecha del sistema cada segundo en un archivo:  

	#!/bin/bash
	#
	#Escribir en el fichero fecha.txt, la fecha del sistema cada segundo hasta 3 veces
	#
	for ((i=0; i<3; i++))
	do
		#El primer date lo mostramos por pantalla
		date
		#El segundo date lo redirigimos a fecha.txt
		date > fecha.txt
		#La siguiente instrucción, simplemente para la ejecución del script durante un segundo
		sleep 1
	done


En este ejemplo, hemos redirigido con el símbolo ">" la salida de "date" a "fecha.txt". Si hacemos "cat fecha.txt" sólo veremos una línea (la de la última ejecución). Explicando esto en más detalle, vemos que nuestro script ha repetido la iteración "for" 3 veces. En la primera ejecución, tras mostrar por pantalla la salida de "date", ha creado un archivo nuevo "fecha.txt" para escribir la salida estándar del siguiente "date". En la segunda ejecución del bucle ocurre lo mismo. En la instrucción "date > fecha.txt", la salida del comando se redirige a un archivo recién creado llamado "fecha.txt", sobrescribiéndolo. De esta forma perderá el contenido de la primera iteración. En la tercera pasada vuelve a ocurrir exactamente lo mismo, generando que el archivo resultante sólo muestre la última ejecución.   
Para que no ocurra esto, podemos utilizar los símbolos de redirección ">>". Con esto, conseguiremos que no se sobrescriba el archivo sino que la salida se añadirá al final de él.

	#!/bin/bash
	#
	#Escribir en el fichero fecha.txt, la fecha del sistema cada segundo hasta 3 veces
	#
	#El siguiente comando es una forma simple para vaciar un archivo. Lo utilizo para eliminar la ejecución del script anterior
	>fecha.txt

	for ((i=1; i<3; i++))
	do
		date
		date >> fecha.txt
		sleep 1
	done


Ahora sí, el archivo contiene la salida de las tres iteraciones.


Existe un comando para poder añadir las salidas de un comando a un archivo y mostrarlos también por pantalla. Esta instrucción es "tee".     
De esta forma, nuestro script quedará de la siguiente forma:   

	#!/bin/bash
	#
	#Escribir en el fichero fecha.txt, la fecha del sistema cada segundo hasta 3 veces
	#
	>fecha.txt
	for ((i=1; i<3; i++))
	do
		date | tee -a fecha.txt
		sleep 1
	done






## Funciones
 
Bash permite crear funciones, lo cual es muy útil si va a utilizar un bloque de código más de una vez. Las funciones reducen la cantidad de código que debe escribir y editar en el caso de modificaciones. Veámoslo!!!


Ejemplo:

	echo "Hola parrot"
	echo "Que función más divertida"
	echo "Hola parrot"


Aunque es un script muy sencillo, podemos ver como debemos editar dos líneas para cambiar el mensaje "Hola parrot" por "Buenos días PARROT".
La estructura de las funciones es la siguiente:
	
	nombre_funcion() {
		comandos
		}

	....
	....
	....

	nombre_funcion


A continuación veremos cómo podemos utilizar funciones en el script anterior.

Ejemplo:

	funcion_parrot() {
  		echo "Buenos días PARROT"
	}
	funcion_diver() {
  		echo "Que función más divertida"
	}

	funcion_parrot
	funcion_diver
	funcion_parrot


Para llamar a las funciones simplemente escribimos el nombre de la función que queremos utilizar sin los paréntesis. Ahora podríamos extender nuestras funciones simplemente cambiando el bloque de código correspondiente.



## Depuración de código

Siempre es útil poder depurar el código. Para tracear un bash script, podemos ejecutarlo mediante una llamada a bash especificando la opción "-x".

Ejecutaremos en la línea de comando:

	$ bash -x ./script.sh

Esto escribirá cada comando por la salida de errores (precedido por el símbolo "+") antes de ser ejecutado.








## Exit code

Cuando un proceso termina, devuelve un valor no negativo llamado valor de retorno o código de salida (exit code) al sistema operativo. Generalmente y por conveniencia devolverá un 0 si se ha ejecutado correctamente y cualquier otro valor si hay un error. De esta forma también se pueden elegir diferentes códigos de error en función del error que haya producido el comando. Un bash script puede devolver un valor utilizando el comando "exit". Por ejemplo:

	exit 4


finaliza el script devolviendo un código de retorno "4" indicando algún tipo de error. Si no especificamos el código de salida, el script devolverá la salida del último comando ejecutado.

Una forma de utilizar estos valores de salida es utilizando los operadores && ("y") y || ("o"). Si disponemos de dos comandos separados por &&, entonces el comando que está a la izquierda se ejecutará primero, y  el comando que está a la derecha sólo se ejecutará si el primero ha terminado satisfactoriamente. Al revés, si están separados por ||, el segundo comando sólo se ejecutará si falla el primero.

Por ejemplo, supongamos que queremos suprimir el archivo log.txt y volverlo a crear como un archivo vacío. Podemos ejecutar estas dos instrucciones:

	$ rm log.txt
	$ touch log.txt


"rm" elimina un archivo dado y "touch" lo  crea vacío en el caso de no existir. Pero realmente, si "rm" falla no queremos ejecutar "touch", es decir, si el archivo log.txt no existe no queremos recrearlo. De esta forma podrá ejecutar los siguientes comandos:

	$ rm log.txt && touch log.txt


Esto es lo mismo que los dos comandos ejecutados anteriormente, exceptuando que el archivo no será creado si este no existía con anterioridad.

El código de retorno de un comando se puede consultar a través de "$?".

Veamos varios ejemplos:

	$ ls ficheronoexistente
	$ echo $?

La salida de estos dos comandos debería ser algo perecido a:

	ls: cannot access 'ficheronoexistente': No such file or directory
	2

La primera línea indica un error a la hora de listar la existencia de un archivo (inventado para la ocasión).  
En la segunda línea la respuesta es un "2". Es decir, nuestro "ls" devolvió un "2" ya que se produjo un error.   
Tenga en cuenta que "echo $?", solamente podrá retener el "exit code" de la última instrucción ejecutada, en este caso un "ls" de un fichero inexistente.   

Comprobemos el "exit code" del mismo comando sobre un archivo que sí exista:


	$ ls /home
	$ echo $?

¿Cuál es la respuesta del segundo comando esta vez? Efectivamente, es un "0". Esto se debe a que el directorio /home existe, por lo tanto nuestro "ls" ha sido capaz de listar su contenido, y así el comando se ha ejecutado perfectamente y sin ningún error. El "echo $?" es muy utilizado en línea de comando para comprobar si, sobre todo cuando  una instrucción ha durado en el tiempo y ha mostrado mucho texto en su salida (p.e. una compilación de código), ha terminado correctamente. Para ello, tras la ejecución del comando, y sin ejecutar ninguna instrucción preguntaremos por "$?" (echo $?) y si devuelve un "0", sabremos que ha terminado correctamente.



A continuación podemos ver un ejemplo de un script, con la utilidad de esto.


	#!/bin/bash
	#
	#Script que solicita una cadena a buscar entre todos los archivos de un directorio dado
	#
	#
        clear

	#Recogida de datos
        read -p "Introduzca un archivo con su ruta absoluta: " ARCHIVO
        read -p "Introduzca cadena de búsqueda: " CADENA

	#Redireccionamos las salidas tanto estándar como de errores para que no aparezca nada por pantalla
        grep -r $CADENA $ARCHIVO 1>/dev/null 2>&1
	
	#Podríamos haber recogido el exit code en una variable
        #EXITCODE=$(echo $?)
        #case $EXITCODE in
        
        
        case $? in
	#Preguntamos por el exit code de nuestro grep
        0)
        	echo "Se encontraron coincidencias"
		#Nuestro script devuelve 0 en su exit code
                exit 0
                ;;
        1)
                echo "No se encontraron coincidencias"
		#Aunque el exit code del grep ha sido "1", en la salida de nuestro script devolvemos un "0", ya que así lo queremos.
                exit 0
                ;;
        2)
                echo "Compruebe el nombre de archivo"
                exit 2
                ;;
        *)
                echo "Error no contemplado"
                exit 3
                ;;
        esac


Intente comprobar las diferentes opciones de nuestro "case". En cada una de las ejecuciones, escriba en la terminal $? para comprobar los exit codes forzados por nosotros.


## Otras variables interesantes

	$0 - El nombre de nuestro bash script.
	$1-$9 - Los primeros 9 argumentos pasados desde la línea de comandos a nuestro bash script.
	$# - Muestra el número de argumentos pasados desde la línea de comandos a nuestro script.
	$@ - Muestra todos los argumentos que se han pasado a nuestro script.
	$? - Muestra el exit code del último proceso ejecutado.
	$$ - Muestra el PID (Process ID) del script.
	$USER - Usuario que está ejecutando el script.
	$HOSTNAME - El hostname de la máquina en la que está corriendo el script.
	$SECONDS - El número de segundos desde que el script comenzó.
	$RANDOM - Devuelve un número aleatorio cada vez que es invocado.


Si escribe "env" en la línea de comandos, podrá ver una lista con diferentes variables que puede utilizar.   
También puede ver otras variables y mucha más información en la página del manual de bash. 
	$ man bash




## Otros "lenguajes" relacionados con bash


Es muy interesante que comprueben el funcionamiento de varios programas muy utilizados en la realización de bash scripts. Pueden ver sus páginas man, e incluso comprobar páginas de ayuda en Internet para ver su funcionamiento.

	tr:  cambia y elimina caracteres de una cadena.
	awk: un lenguaje de programación en si. Se utiliza para procesar textos y cadenas.
	sed: También muy utilizado para procesado de textos.
	grep: Busca cadenas de texto en archivos.
	cut: Procesa textos, mostrándonos diferentes campos.



## whiptail

En Bash script también podemos mostrar cajas de diálogo si el programa whiptail está instalado. Por defecto, Parrot ya lo tiene instalado.

	$ whiptail
	Box options: 
		--msgbox <text> <height> <width>
		--yesno  <text> <height> <width>
		--infobox <text> <height> <width>
		--inputbox <text> <height> <width> [init] 
		--passwordbox <text> <height> <width> [init] 
		--textbox <file> <height> <width>
		--menu <text> <height> <width> <listheight> [tag item] ...
		--checklist <text> <height> <width> <listheight> [tag item status]...
		--radiolist <text> <height> <width> <listheight> [tag item status]...
		--gauge <text> <height> <width> <percent>
	Options: (depend on box-option)
		--clear				clear screen on exit
		--defaultno			default no button
		--default-item <string>		set default string
		--fb, --fullbuttons		use full buttons
		--nocancel			no cancel button
		--yes-button <text>		set text of yes button
		--no-button <text>		set text of no button
		--ok-button <text>		set text of ok button
		--cancel-button <text>		set text of cancel button
		--noitem			don't display items
		--notags			don't display tags
		--separate-output		output one line at a time
		--output-fd <fd>		output to fd, not stdout
		--title <title>			display title
		--backtitle <backtitle>		display backtitle
		--scrolltext			force vertical scrollbars
		--topleft			put window in top-left corner
		-h, --help			print this message
		-v, --version			print version information


Veamos el ejemplo más simple:

	$ whiptail --title "Ejemplo de diálogo" --infobox "Parrot es maravilloso" 8 78

Ejecutamos (o escribimos en un bash script) whiptail con las siguientes opciones:   
- title "Título". El título que queramos para nuestro cuadro de diálogo.    
- infobox "texto". Seleccionamos el tipo de cuadro de diálogo que queremos utilizar.    
- 8 78. El tamaño de nuestra caja.         

## Dónde conseguir más información
Podemos conseguir más información en las siguientes fuentes:   
- El man de bash   
- http://tldp.org/guides.html   
- http://linuxcommand.org/   
- Realizando las búsquedas pertinentes en su buscador favorito  
- Preguntando en el foro de parrotsec https://community.parrotsec.org/   
- Accediendo al grupo de telegram tanto en inglés(https://t.me/parrotsecgroup) como en español (https://t.me/ParrotSpanishGroup). ESTAREMOS ENCANTADOS DE CONOCERLE Y AYUDARLE EN LO QUE PODAMOS.    



