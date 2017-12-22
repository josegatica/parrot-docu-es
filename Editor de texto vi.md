# Editor de texto Vi/Vim

Vi es el clásico editor de texto en sistemas GNU/Linux y Unix, se puede decir que los sistemas que tienen solo un editor de texto es vi, de ahi la importancia de aprender a trabajar con este excelente editor. Muchos lo ignoran debido a que es un poco complejo para nuevos usuarios y terminan adaptándose a otros editores como nano, gedit o leafpad. La complejidad de Vi se debe a que fue creado cuando no todos los teclados tenían teclas de movimiento de cursor, por lo tanto, todas las tareas pueden realizarse en vi pueden llevarse a cabo con las teclas alfanuméricas tradicionales y otras teclas como Esc e Insert. En la actualidad contamos con una versión mejorada de vi, llamada vim, la cual es compatible con versiones anteriores de vi y cuenta con un modo gráfico (gvim) a demas de la interfaz de modo texto estandar de vi. Hoy en día, en la gran mayoría de las distribuciones el comando vi suele ser un alias o un vínculo simbólico a vim.

## Versiones de VIM:

- Tiny (mínima)

- Small (pequeña)

- Normal

- Big (grande)

- Huge (enorme)

Para comprobar qué versión de vim estamos corriendo en nuestro sistema, ejecutamos el siguente comando:

	$ vi --version



## Desplazamiento
Antes de pasar a la edición de texto, veamos cuáles son las teclas que se usan para desplazarnos dentro de un archivo usando VIM.

Use los siguientes comandos para desplazarse dentro de un archivo:

- Desplazarse un caracter hacia la isquierda en la línea actual
	
		h
 
- Pasar a la línea siguiente
	
		j

- Pasar a la línea anterior
	
		k

- Desplazarse un caracter a la derecha en la línea actual
	
		l

- Pasar a la palabra siguiente en la línea actual
	
		w

- Pasar al siguiente fin de palabra en la línea actual
		e

- Pasar al anterior inicio de palabra en la línea actual
		b

- Pasar a la página siguiente
		
		Ctrl+b

- Volver a la página anterior
	
		Ctrl+b

Nota: si tipea un número antes de ejecutar alguno de estos comandos, el comando se repetirá las veces que indique dicho número, digamos que es un conteo de repetición.


Use los siguientes comandos para desplazarse a líneas especificas dentro de un archivo.
	
		G
- Desplazarse a una línea específica dentro del archivo. Por ejemplo, el comando 3G lo ubicará en la línea 3. Sin ningún parámetro, G lo ubica en la última línea del archivo.

		H
- Desplazarse en relación a la línea superior de la pantalla. Por ejemplo, el comando 3H lo ubicará en la tercera línea actual de la pantalla.

		L
- Igual a H, pero en relación a la última línea de la pantalla. Por lo tanto, el comando 2L lo ubicará en la antepenúltima línea de la pantalla. 


Nota: Usted debe practicar cada uno de estos comandos hasta que logre desplazarse cómodamente dentro de un archivo, luego continúe leyendo este capítulo.



Salir del editor:

Es de suma importancia que usted conozca cómo salir del editor para evitar cometer un error, dañar archivos de configuración o documentos importantes. Las opciones para salir de vim son las siguientes:

	
- Salir del editor descartando los cambios.
	
		:q!

- Guardar los cambios realizados en el archivo.
		
		:w!

- Guardar los cambios y salir del archivo (no pide confirmación).
	
		ZZ

- Editar la copia de disco actual del archivo. El archivo se volverá a cargar y todos los cambios realizados se cancelarán.
	
		:e!

- Ejecutar un comando shell. Tipee el comando y presione Enter. Cuando el comando finalice, verá los datos de salida y un aviso para volver al editor vi.
		
		:!

Nota: Al tipear dos puntos (:), el cursor se desplazará a la última linea de la pantalla para permitirle tipear un comando con sus respectivos parámetros. También puede usar los comandos en formato no abreviado (:quit, :write, :edit), esto le permite recordar más fácil cada uno de los comandos, pero su uso es poco frecuente.



## Modos vi

Modo comandos:
En este modo, podemos desplazarnos dentro de un archivo y efectuar operaciones de edición como buscar texto, eliminar texto, etc. vi suele iniciarse en modo de comandos.

Modo insertar:
En el modo insertar, podemos tipear texto nuevo en el punto de inserción de un archivo. Para volder al modo comandos, presione la tecla Esc.


## Edición de texto

Use los siguientes comandos para insertar, eliminar o modificar texto. Tome en cuenta que algunos de estos comandos poseen una forma en mayúscula que es similar a la forma en minúscula.

- Ingrese al modo insertar, tipee el texto deseado y pulse Esc para volver al modo comandos.
	
		i

- Ingrese al modo insertar, tipee el texto deseado y pulse Esc para volver al modo comandos.
		
		a
		# Nota: Use I para insertar texto al comienzo de la línea actual y A para insertar texto al final de la 		# línea actual.

- Use c para modificar el caracter en la posición actual e ingrese al modo insertar para escribir los caracteres de reemplazo. 
	
		c

- Abrir una línea nueva para insertar texto debajo de la línea actual.
	
		o

- Abrir una línea nueva para insertar texto arriba de la línea actual.
	
		O

- Eliminar el resto de la palabra actual e ingresar al modo insertar para reemplazarla.
	
		cw
		# use un conteo para reemplazar varias palabra y c$ para reemplazar hasta el final de la línea.

- Eliminar la linea actual, se puede usar un conteo para eliminar varias líneas.
	
		dd

- Elimine el caracter en la posición del cursor. También puede utilizar un conteo para varios caracteres.
		
		x

- Colocar el último texto eliminado después del caracter actual. Use P para colocarlo antes del cursor.
	
		P

- Intercambia lugares entre el caracter en la posición del cursor y el que tiene a la derecha. 
	
		xp

## Búsqueda de texto

- Use / seguido de una expresión regular para buscar hacia delante en el archivo.

- Use ? seguido de una expresión regular para buscar hacia atrás en el archivo. 

- Use n para repetir la última búsqueda en cualquier dirección. 

Nota: Puede anteponer a cualquiera de los comandos de búsqueda un número que indique un conteo de repetición.


## Acceder a la Ayuda en vi.

- Para obtener ayuda en vi podemos ejecutar el siguiente comando:
	
		:help

- Obtener una ayuda sobre un comando en particular.
	
		:help [comando]

- Esta es una ayuda básica de vi, la cual se abrirá dentro del mismo editor, para salir de la ayuda ejecutamos:
	
		:q

## Conclusión

Como se mencionó al inicio del capítulo, vi es un excelente editor de texto y es de suma importancia que todo usuario de GNU/Linux tenga al menos un conocimiento básico de su uso. Al principio puede ser una tarea tediosa, difícil e incluso aburrida. Existe una herramienta que podemos usar para aprender a trabajar con vi de forma muy fácil e interactiva, esta herramienta es vimtutor, su uso es muy sencillo y realmente ayuda mucho, de seguro cuando usted la pruebe, pasará los próximos treita minutos aprendiendo a trabajar con vi.
	
		┌─[user@parrot]~[/home/user]
		└──╼ $ vimtutor
