# Necesidades

En GNU/Linux existe un usuario llamado root. Este usuario es especial, es el administrador del sistema. Root puede hacer todo en un sistema GNU/linux, por esta razón es muy peligroso trabajar constantemente con dicho usuario. Podríamos escribir un comando incorrectamente y provocar un error en nuestro sistema. Al no utilizar la cuenta de root para trabajar normalmente, también mitigamos la posibilidad de vernos afectados por un virus. Así que la forma correcta para trabajar en un sistema GNU/Linux, es utilizar un usuario con privilegios limitados.

Pero habrá tareas para las que necesitemos convertirnos en root o al menos tener sus privilegios, o quizas lo que necesitemos sea convertirnos en otro usuario del sistema (es menos frecuente pero puede darse el caso).

# El comando "su"

El comando "su" lo utizamos para convertirnos en otro usuario. Puede ser root o puede ser otro usuario del sistema. 

La forma general de utilizarlo es:
	
	$ su nombre

Pero veamos unos ejemplos para entender mejor el funcionamiento de este comando.

Imaginemos una sesión con un usuario común del sistema ("whoami" para saber qué usuario soy yo y "pwd" para ver dónde estoy situado en el path del sistema):

	┌─[test@parrot]─[~]
	└──╼ $whoami
	test
	┌─[test@parrot]─[~]
	└──╼ $pwd
	/home/test

Ahora imaginemos que queremos convertirnos en root:

	┌─[test@parrot]─[~]
	└──╼ $su root
	Password: 


Nos solicitará la contraseña del usuario al que nos queremos convertir, en este caso deberíamos conocer la contraseña de root.
Podría interesarnos convertirnos en otro usuario, por ejemplo test2:

	┌─[test@parrot]─[~]
	└──╼ $su test2
	Password: 


Vemos que simplemente hemos cambiado el nombre del usuario al que nos queremos convertir. En cualquier caso nos solicitará la contraseña de dicho usuario. No la nuestra, sino la del usuario en el que nos queremos convertir. Si es root, la contraseña de root. Si es test2, la contraseña de test2.

Entre las opciones que podemos usar con "su" (man 1 su), existe una para convertirnos en el usuario pero con todo su "enviroment" tal y como si nos hubiésemos logeado en el sistema. Esta opción es "-" o "-l". Es decir, tendremos todas las variables de entorno de dicho usuario, pero sin utilizar la opción no necesariamente las tendremos:

	┌─[✗]─[test@parrot]─[~]
	└──╼ $whoami
	test
	┌─[test@parrot]─[~]
	└──╼ $pwd
	/home/test
	┌─[test@parrot]─[~]
	└──╼ $su root
	Password: 
	┌─[root@parrot]─[/home/test]
	└──╼ #whoami
	root
	┌─[root@parrot]─[/home/test]
	└──╼ #pwd
	/home/test
	┌─[root@parrot]─[/home/test]
	└──╼ #exit
	exit


El comando "exit" sale de la sesión.

Como verá, ahora sí ejecutamos un login "completo" de root, posicionándonos incluso en su $HOME.

	┌─[test@parrot]─[~]
	└──╼ $whoami
	test
	┌─[test@parrot]─[~]
	└──╼ $pwd
	/home/test
	┌─[test@parrot]─[~]
	└──╼ $su - root
	Password: 
	┌─[root@parrot]─[~]
	└──╼ #pwd
	/root
	┌─[root@parrot]─[~]
	└──╼ #exit
	logout


NOTA: Si no indicamos el nombre de usuario, el sistema supondrá que queremos convertirnos en root. Nos solicitará la contraseña de root.

	┌─[test@parrot]─[~]
	└──╼ $su -
	Password: 
	┌─[root@parrot]─[~]
	└──╼ #whoami
	root


NOTA2: Si ejecutamos "su" como usuario root, no se nos solicitará la contraseña. Es al único usuario que no se la solicita y podremos convertirnos en el usuario que queramos.


# El comando "sudo"

El comando "sudo" nos permite ejecutar tareas como otro usuario, sin la necesidad de conocer la contraseña de dicho usuario. Es una forma de delegar tareas en otros usuarios sin la necesidad de dar ninguna contraseña (de esta forma no deberá compartir la contraseña de root).

## Configuración sudoers

Para poder configurar sudo, en primer lugar deberemos tener instalado el paquete correspondiente:

	┌─[root@parrot]─[~]
	└──╼ #apt install sudo

El fichero de configuración, es "/etc/sudoers". Lo podemos editar con nuestro editor favorito o bien, mediante el comando "visudo". Recomendamos utilizar este último comando ya que, entre otras cosas, se encarga de realizar un chequeo de la sintaxis del fichero una vez modificado por nosotros.

	┌─[root@parrot]─[~]
	└──╼ #visudo

	#
	# This file MUST be edited with the 'visudo' command as root.
	#
	# Please consider adding local content in /etc/sudoers.d/ instead of
	# directly modifying this file.
	#
	# See the man page for details on how to write a sudoers file.
	#
	Defaults	env_reset
	Defaults	mail_badpass
	Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
	
	# Host alias specification
	
	# User alias specification

	# Cmnd alias specification

	# User privilege specification
	root	ALL=(ALL:ALL) ALL

	# Allow members of group sudo to execute any command
	%sudo	ALL=(ALL:ALL) ALL

	# See sudoers(5) for more information on "#include" directives:

	#includedir /etc/sudoers.d



Este fichero expuesto arriba, es la configuración por defecto en ParrotSec.

La línea "%sudo    ALL=(ALL:ALL) ALL" establece que cualquier usuario que pertenezca al grupo sudo podrá ejecutar cualquier comando (se sobreentiende que como root).

No vamos a ver más configuraciones, tiene una guía extensa en la pagina del manual sudoers (man 5 sudoers). También puede acudir a su buscador favorito y buscar ejemplos sudo. Pero tenga en cuenta que, aunque en este ejemplo estamos otorgando la posibilidad a un usuario para que ejecute tareas como root, también podría configurar sudo para que se ejecutasen tareas como otro usuario distinto (por ejemplo un usuario de base de datos) o que el usuario sólo pudiése ejecutar algunas tareas (y no todas) como root. Pero el ejemplo que tenemos es el descrito. Usuarios del grupo sudo pueden ejecutar cualquier tarea como root.

De acuerdo con lo visto, necesitamos que nuestro usuario test pueda ejecutar tareas como root. Tal y como vimos, este usuario debe pertenecer al grupo sudo. Para ver la membresía de un usuario ejecutamos la instrucción "id" seguida del nombre de usuario.

	┌─[root@parrot]─[~]
	└──╼ #id test
	uid=1001(test) gid=1001(test) groups=1001(test)

Vemos que el usuario pertenece al grupo(groups) test. Añadamos al usuario al grupo sudo:

	┌─[root@parrot]─[~]
	└──╼ #usermod -a -G sudo test
	┌─[root@parrot]─[~]
	└──╼ #id test
	uid=1001(test) gid=1001(test) groups=1001(test),27(sudo)


"usermod -a -G <grupo> usuario" añade un grupo secundario al usuario.


## Ejecución de comandos con sudo

Para ejecutar comandos como root, desde nuestro usuario test, debemos anteponer a nuestras instrucciones el comando sudo (man 8 sudo).

Veamos un comando ya visto anteriormente: "whoami". Este comando devuelve nuestro nombre de usuario:

	┌─[test@parrot]─[~]
	└──╼ $whoami
	test

¿Qué pasaría si ejecutásemos "sudo whoami"?. Estaríamos lanzando "whoami" como usuario root, por lo que la respuesta debería ser "root".

Comprobémoslo:

	┌─[test@parrot]─[~]
	└──╼ $sudo whoami

	We trust you have received the usual lecture from the local System
	Administrator. It usually boils down to these three things:
	
	    #1) Respect the privacy of others.
	    #2) Think before you type.
	    #3) With great power comes great responsibility.

	[sudo] password for test: 
	root

Al ejecutar sudo, nos pide una contraseña. ¡¡¡ATENCIÓN!!! NO ES LA CONTRASEÑA DE ROOT. Es la contraseña de nuestro usuario test. Una vez introducida vemos que nos devuelve "root", indicándonos que somos root. Tras la devolución del prompt, volvemos a ser el usuario test.

Veamos esto:

	┌─[test@parrot]─[~]
	└──╼ $sudo whoami
	root
	┌─[test@parrot]─[~]
	└──╼ $whoami
	test


La primera vez ejecutamos whoami anteponiéndole sudo, es decir lo ejecutamos como root. La segunda vez desde nuestro usuario test.

Como ha podido comprobar, esta segunda vez que he ejecutado sudo, no se ha solictado la contraseña del usuario test. Tal como indica la página del manual de sudoers (" The user may then use sudo without a password for a short period of time (15 minutes unless overridden by the timestamp_timeout option).") tendremos 15 minutos en los que en la sesión activa no se nos solicitará la contraseña.

Como podemos ejecutar cualquier programa como cualquier usuario, llegado el caso, también podríamos ejecutar comandos como otro usuario (ejemplo con test2):

	┌─[test@parrot]─[~]
	└──╼ $sudo -u test2 whoami
	test2

Hemos visto comandos, si se me permite un poco absurdos. Pero llegado el caso, nuestro usuario test podría editar algún fichero de configuración importante para el sistema, reiniciar un servicio/sistema o incluso cambiar la contraseña de root:

	┌─[test@parrot]─[~]
	└──╼ $sudo passwd root
	Enter new UNIX password: 
	Retype new UNIX password: 
	passwd: password updated successfully

En este último ejemplo ha cambiado la contraseña de root ;-).


# Diferencia entre su y sudo

A modo de resumen:

- su 	--> Nos convierte en otro usuario y debemos conocer su contraseña.
- sudo 	--> Nos permite ejecutar comandos como otros usuarios y no tenemos que conocer la contraseña de dicho usuario.


