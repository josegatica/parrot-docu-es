## Grupos y cuentas de usuario en GNU/Linux.

 GNU/Linux es un sistema multiusuario, que se basa en estructuras y procedimientos de cuentas de datos utilizados para identificar usuarios individuales en un sistema. La administración de estas cuentas es una administración básica, pero es una importante habilidad que debemos tener cuando decidimos ser usuarios de GNU/Linux. Gran parte de la administración del sistema en tareas del día a día se basa en la administración de usuarios, altas, bajas, modificación de usuarios, configuración de sus entornos, etc. En este capítulo vamos a ver cómo realizar estas tareas desde la línea de comandos. Este es un tema muy importante que cada usuario de GNU/Linux debe conocer y dominar.


Seguramente, usted ya tiene una comprensión básica de las cuentas. En GNU/Linux las cuentas son como las cuentas de Windows, Mac OS u otros sistemas operativos, pero hay algunos detalles que las hacen diferentes y merecen la pena explicarlos. Ejemplo de esto, son los diversos usuarios de Linux, la naturaleza de Grupos en Linux y la forma en que Linux asigna los números que utiliza para identificar los nombres de usuarios y de grupos que generalmente se usan.


Las cuentas típicas de Linux son cuentas de usuario identificadas a través del nombre de usuario de la cuenta. Este tipo de cuentas, son para personas que necesitan acceso al sistema. Las cuentas en Linux también pueden ser cuentas de servicios del sistema, también llamados daemons (programa que proporciona un servicio en particular). Los daemons no inician sesión en un sistema Linux, sin embargo, necesitan una cuenta en el sistema para poder funcionar. También hay tipos de cuentas especializadas que se crean con propósitos únicos, como es el caso de una cuenta de usuario para recibir correo electrónico, pero que no debe poder acceder al sistema local. 

## Vinculación de usuarios a través de grupos.

 Linux utiliza los grupos para organizar a los usuarios. Estos están definidos en archivos de configuración similares, tienen nombres similares a los nombres de usuarios y están representados internamente por números. Cada archivo en Linux está asociado con un usuario específico y un grupo específico. Esto nos permite asignar varios permisos a los miembros de un grupo. Por ejemplo, a los miembros del grupo "Dirección" en una empresa, se les puede permitir leer algunos archivos, pero a los miembros del grupo "Obreros" les será desautorizado el acceso a estos archivos. Ya que Linux proporciona acceso a la mayoría de los dispositivos de hardware a través de archivos, también se puede utilizar este mecanismo para controlar el acceso al Hardware. Cada grupo puede tener desde ninguno hasta tantos miembros como usuarios en el sistema.

La pertenencia a grupos se controla a través del archivo /etc/groups. Este archivo contiene una lista de grupos y miembros de cada grupo. Cada usuario tiene un grupo primario. Este grupo primario, de cada usuario, se establece en el registro de configuración en /etc/passwd. Este archivo define cada cuenta de sistema a través de registros de configuración de cuentas individuales. Cuando los usuarios inician sesión en el equipo, su grupo de miembro se establece en su grupo principal. Un usuario puede acceder a los archivos pertenecientes a otros grupos mientras dicho usuario pertenezca a ese grupo y los permisos de dicho grupo permitan el acceso por parte de este usuario. Para ejecutar programas o crear archivos que no pertenezcan a su grupo primario, el usuario debe ejecutar el comando "newgrp" para cambiar la pertenencia al grupo actual.

## Configurando cuenta de usuario

La frecuencia con la que se realiza el mantenimiento de las cuentas de usuario, depende de la naturaleza del sistema que se administre. Algunos sistemas como pequeñas estaciones de trabajo, rara vez requieren cambios. En cambio otros como servidores multiusuarios, pueden requerir de un mantenimiento diario. En este capítulo vamos a trabajar con las herramientas tradicionales utilizadas en Linux para realizar este tipo de operaciones: altas, bajas, modificación y eliminación de cuentas de usuario. La mayoría de las distribuciones, hoy en día, se entregan con herramientas GUI que nos permiten realizar estas operaciones. Dichas herramientas GUI, varían en dependencia de la distribución, por lo que es difícil dar una explicación generalizada para todas las distribuciones de estas herramientas de GUI. En general, las herramientas basadas en texto proporcionan mayor flexibilidad y son más ampliamente aplicables.

## Agregando usuarios

Para agregar usuarios en Linux, podemos hacer uso del comando useradd (en algunas distribuciones podemos encontrarlo como adduser). Su sintaxis básica es la siguiente:

	$ sudo useradd [-c comment] [-d directorio_home] [-e fecha_de_expiración] [-f dias_inactivos] 
  	[-g grupo_por_defecto] [-G grupo] [-p password] [-s shell] [-u UID] nombre_de_usuario

ejemplo:

	$ sudo useradd -m -d /home/parrot -g hackers -G hackers,noobs,sudo parrot

Este comando nos creará al usuario parrot. Su carpeta home sería /home/parrot,con el grupo por defecto hackers y los grupos secundarios hackers,noobs,sudo.


"adduser" es una utilidad menos compleja, ya que nos autocompleta prácticamente casi todo. Es útil para la creación de usuarios estándares en un sistema pequeño. Su sintaxis básica es la siguiente:

	$ sudo adduser [nombre_de_usuario]

ejemplo:

	$ sudo adduser parrot
	Adding user `parrot' ...
	Adding new group `parrot' (1001) ...
	Adding new user `parrot' (1001) with group `parrot' ...
	Creating home directory `/home/parrot' ...
	Copying files from `/etc/skel' ...
	Enter new UNIX password: 
	Retype new UNIX password: 
	Changing the user information for parrot
	Enter the new value, or press ENTER for the default
		Full Name []: Parrot El Pirata
		Room Number []: Centro de Operaciones
		Work Phone []: *************
		Home Phone []: *************
		Other []: ************
	Is the information correct? [Y/n] 


Con esto ya tendríamos creado al usuario parrot en nuestro sistema.


## Modificando Cuentas de Usuario.

Por lo general, las cuentas de usuario son modificadas a petición del usuario o para implementar alguna nueva política de seguridad o cambio en el sistema.

Las cuentas de usuario pueden modificarse de varias formas:

 1 - Editando directamente archivos críticos como /etc/passwd (no recomendado).
 
 2 - Modificando los archivos de configuración específicos del usuario en el directorio principal de la cuenta.
 
 3 - Usando las herramientas del sistema utilizadas anteriormente para crear cuentas.

## Configurar o cambiar contraseñas

La herramienta useradd proporciona el parámetro -p para establecer una contraseña. Esta herramienta no es muy útil cuando directamente agregamos a un usuario, porque requiere una contraseña "pre-hasheada" (cifrada). Por lo tanto, suele ser más fácil crear una cuenta en forma deshabilitada (por no hacer uso del parámetro -p) y una vez creada la cuenta configurarle una contraseña. Para ello podemos hacer uso de la herramienta passwd, su sintaxis es la siguiente:

	$ sudo passwd [-k] [-l] [-u [-f]] [-d] [-S] [nombre de usuario]

- El parámetro -k indica que el sistema debe actualizar una cuenta expirada.

- El parámetro -l bloquea una cuenta, prefijando la contraseña hash con un signo de exclamación.  Esto da a lugar a la imposibilidad de que el usuario pueda iniciar sesión en la cuenta, pero permite que sus archivos aún esten disponibles. Este bloqueo se deshace fácilmente. Es útil si desea suspender el acceso del usuario de forma temporal cuando se ha detectado algún uso inadecuado de la cuenta.

- El parámetro -u desbloquea una cuenta eliminando el signo de exclamación que indica que la cuenta esta deshabilitada. Tenga en cuenta que el comando useradd crea cuentas que están deshabilitadas y no tienen contraseña, a menos que haga uso del parámetro -p. Por lo tanto, utilizando este parámetro "passwd -u" en una cuenta nueva, no sólo elimina el bloqueo sino que también resulta en una cuenta sin contraseña. Normalmente passwd no permite esto a menos que se use el parámetro -f, el cual obliga a passwd a activar o convertir esta cuenta en una cuenta sin contraseña.

- El parámetro -d elimina la contraseña de una cuenta sin nigún mensaje de advertencia.

- El parámetro -S muestra información sobre la contraseña de una cuenta. Esta información nos dice si está establecida una contraseña y qué tipo de algoritmo fue utilizado para cifrar la contraseña en un hash.


Anteriormente creamos la cuenta para el usuario "Parrot El Pirata (parrot)". Para establecer una nueva contraseña al usuario "parrot", utilizamos el comando passwd como se muestra en el siguiente ejemplo.

	$ sudo passwd parrot 
	Enter new UNIX password: 
	Retype new UNIX password: 
	passwd: password updated successfully

## Uso básico de usermod

La herramienta usermod es muy similar a la herramienta useradd en cuanto a sus funciones y parámetros, con la diferencia de que ésta se utiliza para realizar cambios en una cuenta existente, en lugar de crear una nueva. Las diferencias entre ambas herramientas son las siguientes:

- usermod permite el uso adicional del parámetro -m cuando se usa de forma combinada con -d. El parámetro -d nos permite cambiar el directorio home del usuario pero no mueve ningún archivo, el parámetro -m le permite a usermod mover los archivos del usuario al nuevo directorio home.

- usermod admite el parámetro -l, el cual cambia el nombre de usuario utilizado para el login a un nuevo valor especificado. Por ejemplo, al ejecutar "usermod -l pirata parrot", cambiaría el nombre del usuario parrot a pirata.

- También podemos bloquear o desbloquear contraseñas de usuario utilizando los parámetros -L y -U respectivamente.

- La herramienta usermod cambia el contenido del /etc/passwd o /etc/shadow, dependiendo del parámetro utilizado. Si se usa -m también mueve los archivos del usuario como se indicó anteriormente.


  Editar las características de una cuenta mientras está en uso, puede traer consecuencias indeseadas. Principalmente cuando usamos los parámetros -d y -m. Esto puede causar que el usuario pierda el acceso a archivos en los que está trabajando al moverlos de directorio. La mayoría de los cambios en la configuración de la cuenta no surtirán efecto hasta que el usuario se haya desconectado y regresado nuevamente.

  Esto sucede porque al cambiar el UID de la cuenta, esta acción no cambia los UID asociados con los archivos. Debido a esto, el usuario puede perder el acceso a sus archivos personales. Se puede actualizar de forma manual los UID en todos los archivos utilizando el comando chown, el cual veremos en el siguiente capítulo.

  Cuando utilice la opción -G para agregar a un usuario a nuevos grupos, tenga en cuenta que cualquier grupo que no esté listado será eliminado, por lo tanto, es una buena idea utilizar el parámetro -a también. Utilizar los parámetros -a y -G juntos nos permite agregar a un usuario a un nuevo grupo sin tener que enumerar los grupos antiguos en el comando. Por ejemplo para agregar el usuario "pirata" al grupo "hackers", utilizamos el siguiente comando:

		$ groups parrot
		parrot : parrot
		$ sudo usermod -a -G hackers parrot
		parrot : parrot hackers


En el ejemplo anterior, utilizamos el comando groups para mostrar el grupo actual de la cuenta del usuario parrot. El grupo actual es parrot. Para agregarlo como miembro de otro grupo, se utiliza el comando usermod con los parámetros -a y -G, permitiéndonos conservar la membresía al grupo original (parrot).


## Uso del comando chage

El comando chage nos permite modificar los ajustes de la cuenta relacionados con la caducidad de la misma. Es posible configurar las cuentas en Linux de modo que expiren automáticamente si se cumplen dos condiciones:

1 - La contraseña no se ha cambiado en un periodo de tiempo especificado.

2 - La fecha del sistema ha pasado un tiempo predeterminado.

Estos ajustes se controlan a través de la utilidad chage y su sintaxis es la siguiente:

	chage [-l] [-m dias_minimos] [-M días_máximos] [-d último_día] [-I días_inactivos] 
	[-E fecha_de_expiración] [-W advertencia] nombre_de_usuario

Estos parámetros modifican las acciones del comando chage:

- El parámetro -l hace que chage muestre la caducidad de la cuenta y la información de envejecimiento de la contraseña para un usuario particular.

- El parámetro -m establece el número mínimo de días entre los cambios de contraseña o indica que un usuario puede cambiar una contraseña varias veces en un día, indicando con números cada cuantos días puede cambiar la contraseña. "1" indica que el usuario puede cambiar la contraseña una vez al día, 2 indica que el usuario puede cambiar la contraseña cada 2 días y así sucesivamente.

- El parámetro -M establece el número máximo de días que puede pasar entre cambios de contraseña. 30 indica que se requiere un cambio de contraseña aproximadamente una vez al mes.

- El parámetro -d establece el último día en que se cambió una contraseña. Linux, normalmente, mantiene ese valor automáticamente, pero se puede utilizar este parámetro para modificar el recuento de cambio de contraseña. El último día se expresa en el formato AAAA/MM/DD o con el número de días que han pasado desde el 1 de Enero de 1970.

- El parámetro -I establece el número de días entre la caducidad de la contraseña y la cuenta. Una cuenta caducada no se puede utilizar o puede obligar al usuario a cambiar la contraseña inmediatamente después de conectarse. Esto depende de la configuración elegida por la distribución.

- El parámetro -E establece una fecha de caducidad obsoleta. Por ejemplo, puede utilizar -E 2017/12/31 para que una cuenta venza el 31 de Diciembre del 2017 o utilizando el número de días que han pasado desde el 1 de Enero de 1970. También es importante mencionar que el valor "-1" no representa una fecha de caducidad.

- El parámetro -W establece el número de días antes de la expiración de la cuenta, en los que el sistema debe enviar avisos sobre la caducidad inminente al usuario. Es importante mencionar que estos avisos solo se muestran a usuarios de inicio de sesión en modo texto, los usuarios que inician sesión desde la GUI por lo general no ven estos avisos.


## Descripción de los archivos de configuración de las cuentas.

Se pueden modificar directamente los archivos de configuración de usuarios. Los archivos /etc/passwd y /etc/shadow controlan la mayoría de las características básicas de una cuenta. Ambos archivos constan de un conjunto de registros, una línea por registro de cuenta. Cada registro comienza con un nombre de usuario y continúa con un conjunto de campos determinados por dos puntos (:). Muchos de estos campos pueden ser modificados con usermod o passwd. Una entrada de registro típica de /etc/passwd es similar a la siguiente:

	user:x:1000:1000:User,,,:/home/user:/bin/bash

Cada uno de los campos tiene un significado específico:

- El primer campo en cada línea de /etc/passwd es el nombre de usuario (en este caso user).

- El segundo campo se ha reservado tradicionalmente para la contraseña. Sin embargo, en la mayoría de los sistemas GNU/Linux hoy en día, se utiliza un sistema de contraseñas de sombra (Shadow Password), en la que la contraseña se almacena en /etc/shadow. La x en el campo de contraseña de ejemplo, indica que las contraseñas de sombra están en uso. En un sistema que no utiliza contraseñas sombra, en su lugar aparece una contraseña cifrada.

- El siguiente campo es el número ID de la cuenta de usuario (1000 en este ejemplo).

- El ID de grupo de inicio de sesión predeterminado es el siguiente campo en la línea /etc/passwd. En este caso el GID primario es 1000.

- El campo de comentario puede tener diferentes contenidos en sistemas diferentes. En el ejemplo anterior, el nombre completo del usuario (User). En algunos sistemas (como es el caso de Parrot) se coloca información adicional separada por comas. Esta información puede incluir números de teléfonos del usuario, oficina o departamento, título o puesto que ocupa, etc.

- El directorio principal del usuario, /home/user, es el siguiente campo de datos en el registro.

- La Shell predeterminada es el último elemento de cada registro de /etc/passwd. Normalmente /bin/bash o alguna otra Shell de comandos común. También es posible crear cuentas de usuario con un shell por defecto, como /bin/false. Esto impide que los usuarios inicien sesión como usuarios comunes pero deja intactas otras utilidades del sistema como recibir correos electrónicos utilizando protocolos de recuperación como POP, IMAP... otro ejemplo interesante es utilizar /bin/passwd para que los usuarios puedan cambiar su contraseña de forma remota, pero no puedan iniciar sesión utilizando una shell de comandos.


Normalmente una entrada de registro en /etc/shadow se parece a la siguiente:

	user:$6$vocRBGox$Nngt/9rYAijBogufMToHFdb3xzD/7J0GLp9.67/WM82mSsfb3FuC0:17450:0:99999:7:-1:-1:

La mayoría de los campos corresponden a las opciones establecidas con la herramienta chage. Otras se establecen con passwd, useradd o usermod. A continuación se explica el significado de cada uno de los campos:

- Cada línea comienza con el nombre de usuario. Se puede notar como el UID no es utilizado en el archivo /etc/shadow. El nombre de usuario vincula las entradas de este archivo a las de /etc/passwd.

- El siguiente campo, corresponde a las contraseñas. Estas se almacenan en forma de hash (cifrado), por lo que no se parecen en nada a la contraseña real. Un asterisco (*) en el campo de contraseña, indica que esta cuenta no acepta inicios de sesión. Un signo de exclamación (!) delante del hash de la contraseña, indica que la cuenta está bloqueada (esto se produce al bloquear la cuenta con la herramienta "usermod -L". Al utilizar el parametro -L, se agrega este símbolo de exclamación para evitar que el usuario inicie sesión, pero se mantiene la contraseña original). Dos signos de exclamación (!!), indican que no se ha establecido una contraseña para la cuenta. Esto también puede indicar que la cuenta no acepta inicios de sesión.

- El siguiente campo (17450), indica la fecha del último cambio de contraseña. Se indica como el número de días pasados desde el 1 de Enero de 1970.

- El siguiente campo (0), es el número de días antes de que se permita un cambio de contraseña.

- El siguiente campo (99999), indica el número de días (después de la última modificación de la contraseña) en los que se requiere un cambio de contraseña.

- El siguiente campo (7), indica la cantidad días en los que se va a comenzar a notificar al usuario sobre la expiración de la contraseña.

- Normalmente en un sistema configurado con caducidad para las cuentas, nos encontramos con 3 campos más: Días entre la Expiración y la Desactivación de la cuenta (-1 en el ejemplo anterior), Dia de Expiración de la cuenta (-1 en el ejemplo anterior) y el último campo está reservado para un uso futuro. Por lo general, en los sistemas GNU/Linux, no se utiliza o contiene un valor sin sentido.


  Analizando detenidamente lo anterior, podemos afirmar que las cuentas de usuario pueden ser modificadas editando directamente estos archivos (/etc/passwd y /etc/shadow). Los valores -1 y 99999 indican que dicho campo ha sido desactivado, y ha sido inutilizado. Realmente, se recomienda realizar estas modificaciones usando las herramientas usermod y chage, ya que es mucho más difícil realizar estos cambios directamente en los archivos de configuración. Esto es así porque se pueden cometer errores como: desactivar una cuenta antes o después del tiempo requerido, cometer un error a la hora de calcular los días desde el 1 de Enero de 1970, omitir años bisiestos, etc.

  Algo similar sucede al tratar de modificar un hash de una contraseña. Esta no puede ser editada de forma efectiva, excepto a través de la herramienta passwd o herramientas similares. Se puede copiar y pegar un valor de un archivo compatible o usar cripto, pero generalmente es mucho más fácil usar passwd. Por otro lado, no es recomendable copiar hashes de contraseñas desde otros sistemas porque significará que los usuarios tendrán las mismas contraseñas en ambos sistemas, y este hecho será obvio para alguien que haya adquirido ambas listas de hashes.
