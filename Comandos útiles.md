# Lista de Comandos GNU/Linux

### Trabajo con Ficheros

- ls
		
		Lista los ficheros de un directorio.
	
	
	1 - ls -l
		
		Lista también las propiedades y atributos.

	2 - ls -la
	
		Lista ficheros incluidos los ocultos de sistema.

	3 - ls -la | more
		
		Lista los ficheros de un directorio de forma paginada.

	4 - ls -lh
		
		Lista ficheros especificando la unidad de tamaño (Kilobyte, Megabyte, Gigabyte).

	5 - ls -l | grep ^d
		
		Lista sólo los directorios

- cat -n fichero
		
		Muestra el contenido de un fichero.(-n lo numera)

- pr -t fichero
	
		Muestra el contenido de un fichero de manera formateada
	
	
- cat fichero | less
- cat fichero | lmore
- more fichero
- less fichero
	
		Muestran el contenido de un fichero de forma paginada.

- zcat fichero
- zmore fichero
- zless fichero
		
		Muestran el contenido de un fichero comprimido (.gz)

- echo candena
		
		echo nos muestra en pantalla el texto que le siga.

- grep \'cadena\' archivo
		
		Muestra las líneas del archivo que contienen la cadena.

- stat fichero
		
		Muestra el estado de un fichero.

- file fichero
		
		Muestra de qué tipo es un fichero.

- tail archivo
		
		Muestra las últimas líneas de un archivo, 10 por defecto.

	1 - tail -n 12 archivo
		
		Muestra las 12 últimas líneas del archivo.
      
	2 - tail -f archivo
		
		Muestra las últimas líneas del archivo, actualizándolo a medida que se van añadiendo. Útil para controlar logs.

- head fichero
		
		Muestra las primeras líneas de un archivo, 10 por defecto. Admite opción -n igual que el comando tail.

- find /usr -name lilo -print
		
		Busca todos los ficheros con nombre lilo en /usr.

- find /home/paco -name *.jpg -print
	
		Busca todas las imágenes .jpg en /home/paco.

- whereis ejecutable
		
		Busca ejecutables(ejemplo: whereis find)

- type comando
		
		Muestra la ubicación del comando indicado. 
		Si es un comando interno del shell mostrará algo así como:comando is a shell builtin.
- pwd
		
		Visualiza el directorio actual.

- history
		
		Muestra el listado de comandos usados por el usuario (~/.bash_history)

- cd nom_directorio
		
		Cambia de directorio

      	
	1 - cd ..
        
		Vuelves al anterior.
     
	2 - cd /home/paco/.mozilla
        	
		Entras al de mozilla.(indicando la ruta completa)

- cp -dpR fichero1 ruta_fichero2
		
		Realiza una copia del fichero1 a ruta_fichero2, cambiándole el nombre.

	cp -dpR fichero1 /directorio
		
		Copia fichero1 a directorio, conservando fichero1 el nombre.
	-R
     	
		Copia un directorio recursivamente,salvo los ficheros especiales.
	-p
     	
		Copia preservando permisos, propietario, grupos y fechas.
        
	-d
     	
		Conserva los enlaces simbólicos como tales y preserva las relaciones de los duros.
      	
	-a
     	
		Lo mismo que -dpR .

- mv ruta_fichero1 ruta_fichero2
		
		Mueve y/o renombra ficheros o directorios.

- mkdir nom_directorio
		
		Crea un directorio.

- rmdir nom_directorio
		
		Elimina un directorio(tiene que estar vacío).
- rm archivo

		Elimina archivos .

	rm -r directorio
		
		Borra los ficheros de un directorio recursivamente.Quietorrrrrrr...

	rm *.jpg
	
		Borra todos los ficheros .jpg del directorio actual.

- ln ruta_fichero ruta_enlace
	
		Crea un enlace duro (con el mismo inodo,es decir mismo fichero con distintos nombres)

- ln -s ruta_directorio ruta_enlace
		
		Crea un enlace simbólico (con diferente inodo,es decir se crea un nuevo fichero	que apunta al 
		\"apuntado\",permitiendo enlazar con directorios y con ficheros de otro sistema de archivos)

- diff [opciones] fichero1 fichero2
		
		Compara ficheros.

	diff -w fichero1 fichero2

		Descarta espacio en blanco cuando compara líneas.

	diff -q fichero1 fichero2
		
		Informa sólo de si los ficheros difieren,no de los detalles de las diferencias.

	diff -y fichero1 fichero2
		
		Muestra la salida a dos columnas. 

- join [opciones] fichero1 fichero2
		
		Muestra las líneas coincidentes entre fichero1 y fichero2.

- wc fichero
		
		Muestra el nº de palabras,líneas y caracteres de un archivo.

- wc -c fichero
		
		Muestra el tamaño en bytes de un fichero.

- touch [-am][-t] fichero
		
		Cambia las fechas de acceso (-a) y/o modificación (-m) de un archivo.

	touch -am fichero
		
		A la fecha actual.Si no existiese el fichero,se crearía.

	touch -am -t 0604031433.30 fich
                
		AAMMDDhhmm.ss ------- Si no se especifican los segundos,tomaría 0 como valor. 
		A la fecha especificada.Si no existiese el fichero,se crearía.

	touch fichero
		
		Usado sin opciones crearía un fichero con la fecha actual.


- split -b 1445640 mozart.ogg mozart
  	  
			Partir un archivo

- cat mozart.* > mozart.ogg
		
		Unir las distintas partes de un fichero cortado con split.

- chown [-R] usuario fichero
		
		Cambia el propietario de un fichero o directorio.

- chgrp [-R] grupo fichero
		
		Cambia el grupo de un fichero o directorio.

- chmod [-R][ugo][+/- rwxs] fichero
		
		Cambia los permisos de acceso de un fichero

		+: da permisos -: quita permisos
		u: propietario R: recursivo
		g: grupo r: lectura ejemplo: chmod +x fichero, es lo mismo que: chmod a+x fichero
		o: otros w: escritura explicación: a es la opción por defecto.
		a: todos x: ejecución
		s: los atributos suid y sgid,otorgan a un \"fichero\" los permisos de su dueño o grupo respectivamente,cada vez 
		que se ejecute,sea quien sea el que lo ejecute.

		Ejemplo: chmod +s /usr/bin/cdrecord

		Cómo afectan los permisos a los directorios:
		r permite ver su contenido(no el de sus ficheros)
		w permite añadir o eliminar ficheros (no modificarlos)
		x permite acceder al directorio.

		Método absoluto de determinar los permisos: chmod 760 fichero
		explicación:          dueño     grupo      otros
		------------          -----     -----      -----
		asci                  r w x     r w -      - - -
		binario               1 1 1     1 1 0      0 0 0
		octal                   7         6          0
		paso de asci          r w x     r w -      - - -    activar=1
		a binario             1 1 1     1 1 0      0 0 0   desactivar=0
		paso de               1 1 1     1 1 0      0 0 0   r activado=4
		binario               4+2+1     4+2+0      0+0+0   w activado=2
		a octal                 7         6          0     x activado=1 



### Empaquetado y compresión

        7z a fichero.7z fichero
- Comprimir.

        7z e fichero_comprimido
- Descomprimir.

        7z x fichero_comprimido -o ruta_de_destino
- Extraer donde indicamos.

    	7z l fichero_comprimido
- Ver contenido.

        7z t fichero_comprimido
- Chequea el contenido.

##### Notas sobre 7zip

Comprime en formato 7z, zip, gzip, bzip2 y tar.
Si es un directorio lo hace recursivamente sin emplear la opción -r
        
        
- Con -t{tipo de fichero} tras las opción \"a\" elegimos el formato de compresión:
		
		7z a -tgzip fichero.gz fichero

- Con -p protegemos con una contraseña el fichero:
        	
		7z a -tgzip -p fichero.gz fichero

- Para comprimir más de un archivo gz o bz2 antes hay que empaquetarlos en formato tar:
        	
		1º)
     	   	- 7z a -ttar prueba.tar *.txt

        	2º)
        	- 7z a -tgzip prueba.tgz prueba.tar


###### Archivos zip
        zip -r fichero.zip fichero ;ejemplo: zip -r sinatra.zip ./sinatra/
- Comprimir zip.
        
        unzip archivo.zip
- Descomprimir zip.
        
        unzip -v archivo.zip

- Ver contenido zip.
        
        unrar e -r archivo.rar (e extrae en el directorio actual)

- Descomprimir rar.
        
        unrar x -r archivo.rar directorio de destino (x extrae donde se indique)

- Descomprimir rar.
        
        unrar v archivo.rar

- Ver contenido rar.

        gzip -r fichero ; ejemplo: gzip -r ./sinatra

- Comprimir gz.

        gzip -d fichero.gz

- Descomprimir gz.
        
        gzip -c fichero.gz

- Ver contenido gz.
        
        bzip2 fichero ; ejemplo: bzip2 ./sinatra/*.ogg

- Comprimir bz2.
        
        bzip2 -d fichero.bz2

- Descomprimir bz2.
       
       
##### NOTA:
r equivale en todos los casos a recursivo
Mientras que zip comprime y empaqueta,gzip ó bzip2 sólo comprimen ficheros,no directorios,para eso existe tar.


##### Ficheros tar
        tar -vcf archivo.tar /fichero1 /fichero2 ...(fichero puede ser directorio)
- Empaquetar.

        tar -vxf archivo.tar
- Desempaquetar.

        tar -vtf archivo.tar
- Ver contenido.

##### Para comprimir varios ficheros y empaquetarlos en un solo archivo hay que combinar el tar y el gzip o el bzip2 de la siguiente manera:

##### Ficheros tar.gz (tgz)
        tar -zvcf archivo.tgz directorio
- Empaquetar y comprimir.


        tar -zvxf archivo.tgz
- Desempaquetar y descomprimir.
        
        tar -zvtf archivo.tgz
- Ver contenido.
        
##### Ficheros tar.bz2 (tbz2)
        tar -jvcf archivo.tbz2 directorio
- Empaquetar y comprimir.

        tar -jvxf archivo.tbz2
- Desempaquetar y descomprimir.

        tar -jvtf archivo.tbz2
- Ver contenido.

##### Opciones de tar:
-c : crea un nuevo archivo.
-f : cuando se usa con la opción -c, usa el nombre del fichero especificado para la creación del fichero tar
cuando se usa con la opción -x, retira del archivo el fichero especificado.
-t : muestra la lista de los ficheros que se encuentran en el fichero tar
-v : muestra el proceso de archivo de los ficheros.
-x : extrae los ficheros de un archivo.
-z : comprime el fichero tar con gzip.
-j : comprime el fichero tar con bzip2.

### Comodines

- (~) Sustituye el directorio home de manera que:

- ~/comandos.txt equivale a /home/paco/comandos.txt (si estamos en nuestro propio directorio)
- ~pepe/comandos.txt equivale a /home/pepe/comandos.txt (pepe es otro usuario)

- (?) Sustituye un solo caracter.Ejemplos:
  
        ls p?pe

- mostraría todos los ficheros cuyos 1º 3º y 4º caracteres fuesen p,p y e

        ls ?epe

- mostraría todos los ficheros de 4 caracteres y acabados en epe


- (*) Sustituye cualquier sucesión de caracteres.Ejemplos:

        ls .ba*

- muestra todos los directorios o ficheros que comiencen con .ba

        ls .*

- muestra todos los archivos ocultos.

        rm -r *

- otra manera de desinstalar el sistema operativo.

        rm *.jpg

- borra todas las imágenes jpg

        oggdec *.ogg

- pasa de ogg a wav todos los ogg del directorio en el que estamos.

- (;) Puesto entre dos comandos hace que tras el primero se ejecute el segundo. Ejemplos:

        nano nuevo.txt ; cat nuevo.txt

- nos abrirá el editor nano para que escribamos lo que queramos en un nuevo archivo que se llamará nuevo.txt y tras guardar y salir del editor,cat nos mostrará el contenido de lo que acabamos de crear. 



# == Documento en Construcción ==
