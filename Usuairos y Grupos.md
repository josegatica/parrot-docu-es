## Grupos y cuentas de usuario en GNU/Linux.

 GNU/Linux es un sistema multiusuario, que se basa en estructuras y procedimientos de cuentas de datos utilizados para identificar usuarios individuales en un sistema. La administración de estas cuentas es una administración básica pero es una importante habilidad que debemos tener cuando decidimos ser usuarios de GNU/Linux. Gran parte de la administración del sistema en tareas del día a día se basa en la administración de usuarios, altas, bajas, modificación de usuarios, configuración de sus entornos, etc. En este capítulo vamos a ver como realizar estas tareas desde la linea de comandos. Este es un tema muy importante que cada usuario de GNU/Linux debe conocer y dominar.


De seguro usted ya tiene una comprensión básica de las cuentas, en Linux las cuentas son como las cuentas de Windows, Mac OS u otros sistemas operativos, pero hay algunos detalles que las hacen diferentes y merecen la pena explicarlos, ejemplo de esto son los diversos usuarios de Linux, la naturaleza de Grupos en Linux y la forma en que Linux asigna los números que utiliza para identificar los nombres de usuarios y de grupos que generalmente se usan.


Las cuentas típicas de Linux son cuentas de usuario identificadas a través del nombre de usuario de la cuenta, este tipo de cuenta son para personas que necesitan acceso al sistema. Las cuentas en Linux también pueden ser cuentas de servicios del sistema, tambien llamados daemons (programa que proporciona un servicio en particular). Los daemons no inician sesión en un sistema Linux, sin embargo, nesecitan una cuenta en el sistema para poder funcionar. Tambien hay tipos de cuentas especializadas que se crean con propósitos únicos, como es el caso de una cuenta de usuario para recibir correo electrónico pero no poder acceder al sistema local. 

## Vinculación de usuarios a través de grupos.

 Linux utiliza los grupos para organizar los usuarios, estos estan definidos en archivos de configuración similares, tienen nombres similares a los nombres de usuarios y están representados internamente por números. Cada archivo en Linux está asociado con un usuario específico y un grupo específico, esto nos permite asignar varios permisos a los miembros de un grupo, por ejemplo los miembros del grupo "Dirección" en una empresa se le pueden permitir leer algunos archivos, pero a los miembros del grupo "Obreros" pueden ser desautorizado el acceso a estos archivos. Ya que Linux proporciona acceso a la mayoría de los dispositivos de hardware a través de archivos, tambien se puede utilizar este mecanismo para controlar el acceso al Hardware, cada grupo puede tener desde ninguno hasta tantos miembros como usuarios en el sistema.

La pertenencia a grupos se controla a través del archivo /etc/grups, este archivo contiene una lista de grupos y miembros de cada grupo. Cada usuario tiene un grupo primario, este grupo primario de cada usuario se establece en el registro de configuración en /etc/passwd, este archivo define cada cuenta de sistema a través de registros de configuración de cuentas individuales. Cuando los usuarios inician sesión en el equipo, su grupo de miembro se establece en su grupo principal. Un usuario puede acceder a los archivos pertenecientes a otros grupos mientras que el usuario pertenezca a ese grupo y los permisos de dicho grupo permitan el acceso por parte de este usuario. Para ejecutar programas o crear archivos que no pertenescan a su grupo primario, el usuario debe ejecutar el comando "newgrp" para cambiar la pertenencia al grupo actual.

## Configurando cuenta de usuario

La frecuencia con la que se realiza el mantenimiento de las cuentas de usuario, depende de la naturaleza del sistema que se administre. Algunos sistemas como pequeñas estaciones de trabajo, rara vez requieren cambios, en cambio otros como servidores multiusuarios, pueden requerir mantenimiento a diario. En este capítulo vamos a trabajar con las herramientas tradicionales utilizadas en Linux para realizar este tipo de operaciones, altas, bajas, modificación y eliminación de cuentas de usuario. La mayoria de las distrubuciones hoy en día, se entregan con herramientas GUI que nos permiten realizar estas mismas operaciones, estas herramientas GUI varian en dependencia de la distribución por lo que es dificil dar una explicación generalizada para todas las distribuciones de estas herramientas de GUI. En general, las herramientas basadas en texto proporcionan mayor flexibilidad y son más ampliamente aplicables.

## Agregando usuarios

Para agregar usuarios en Linux, podemos hacer uso del comando useradd (en algunas distribuciones podemos encontrarlo como adduser). su sintaxis básica es la siguiente:

$ sudo useradd [-c comment] [-d directorio_home] [-e fecha_de_expiración] [-f dias_inactivos] 
  [-g grupo_por_defecto] [-G grupo] [-p password] [-s shell] [-u UID] nombre_de_usuario

ejemplo:

$ sudo useradd -m -d /home/parrot -g hackers -G hackers,noobs,sudo parrot

este comando nos crearia el usuario parrot, su carpeta home seria /home/parrot, grupo por defecto hackers, grupos secundarios hackers,noobs,sudo.


adduser es una utilidad menos compleja, ya que esta nos autocompleta practicamente casi todo, es útil para la creación de usuarios estandars en un sistema pequeño. Su sintaxis básica es la siguiente:

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


Con esto ya tendriamos creado el usuario parror en el sistema.


## Modificando Cuentas de Usuario.

Por lo general, las cuentas de usuario son modificadas a petición del usuario o para implementar alguna nueva política de seguridad o cambio en el sistema.

Las cuentas de usuario pueden modificarse de varias formas.

 1 - Editando directamente archivos críticos como /etc/passwd. (no recomendado).
 
 2 - Modificando los archivos de configuración específicos del usuarioen el directorio principal de la cuenta.
 
 3 - Utilizando las herramientas del sistema utilizadas anteriormente para crear cuentas.

## Configurar o cambiar contraseñas

La herramienta useradd proporciona el parámetro -p para establecer una contraseña, esta herramienta no es muy útil cuando directamente agrega un usuario porque requiere una contraseña pre-hash. Por lo tanto, suele ser más fácil crear una cuenta en forma deshabilitada (por no hacer uso del parametro -p) y una vez creada la cuenta configurarle una contraseña. Para ello podemos hacer uso de la herramienta passwd, su sintaxis es la siquiente:

$ sudo passwd [-k] [-l] [-u [-f]] [-d] [-S] [nombre de usuario]

- El parámetro -k indica que el sistema debe actualizar una cuenta expirada.

- El parámetro -l bloquea una cuenta prefijando la contraseña hash con un signo de exclamación, esto trae como resultado que el usuario ya no puede iniciar sesión en la cuenta pero los archivos aun están disponibles. Este bloqueo se deshace fácilmente, es útil si desea suspender el acceso del usuario de forma temporal cuando se ha detectado algun uso inadecuado de la cuenta.

- El parámetro -u desbloquea una cuenta eliminando un signo de exclamación principal, tenga en cuenta que el comando useradd crea cuentas que están deshabilitadas y no tienen contraseña a menos que haga uso del parametro -p, por lo tanto utilizando este parámetro passwd -u en una cuenta nueva no sólo elimina el bloqueo sino que también resulta en una cuenta sin contraseña, normalmente passwd no permite esto a menos que se use el parametro -f, el cual obliga a passwd a activar o convertir esta cuenta en una cuenta sin contraseña.

- El parámetro -d elimina la contraseña de una cuenta sin mensaje de advertencia ni nada.

- El parámetro -S muestra información sobre la contraseña de una cuenta, esta información nos dice si esta establecida una contraseña y que tipo de algoritmo fue utilizado para encriptar la contraseña en un hash.


Anteriormente creamos la cuenta para el usuario "Parrot El Pirata (parrot)", para establecer una nueva contraseña al usuario "parrot" utilizamos el comando passwd como se muestra en el siguiente ejemplo.

$ sudo passwd parrot 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully

## Uso básico de usermod

La herramienta usermod es muy similar a la herramienta useradd en cuanto a sus funciones y parametros, con la diferencia de que esta es para realizar cambios en una cuenta existente en lugar de crear una nueva. Las diferencias entre ambas herramientas son las siguientes:

- usermod permite el uso adicional del parámetro -m cuando se usa combina con -d. El parametro -d nos permite cambiar el directorio home del usuario pero no mueve ningun archivo, el parametro -m le permite a usermod mover los archivos del usuario al nuevo directorio home.

- usermod admite el paremetro -l, el cual cambia el nombre de usuario utilizado para el login a un nuevo valor especificado. Ejemplo, al ejecutar "usermod -l pirata parrot" cambiaria el nombre de usuario de parrot a pirata.

- Tambien podemos bloquear o desbloquer contraseñas de usuario utilizando los parámetros -L y -U respectivamente.

- La herramienta usermod cambia el contenido del /etc/passwd o /etc/shadow, dependiendo del parámetro utilizado. Si se usa -m también mueve los archivos del usuario como se indicó anteriormente.


  Editar las caracteristicas de una cuenta mientras esta esta en uso, puede traer consecuencias indeseadas. Principalmente cuando usamos los parámetros -d y -m, esto puede causar que el usuario pierda el acceso a archivos en los que está trabajando, al moverlos de directorio. La mayoría de los cambios en la configuración de la cuenta no sufrián efectos hasta que el usuario se haya desconectado y regresado nuevamente.

  Esto sucede porque al cambiar el UID de la cuenta, esta acción no cambia los UID asociados con los archivos, debido a esto el usuario puede perder el acceso a sus archivos personales. Se puede actualizar de forma manual los UID en todos los archivos utilizando el comando chown que lo vamos a ver en el siguiente capitulo.

  Cuendo se utiliza la opción -G para agregar un usuario a nuevos grupos, tenga en cuenta que cualquier grupo que no esté listado será eliminado, por lo tanto, es una buena idea utilizar el parámetro -a también. Utilizar los parámetros -a y -G juntos nos permite agregar un usuario a un nuevo grupo sin tener que enumerar los grupos antiguos en el comando. Por ejemplo para agregar el usuario pirata al grupo hackers, utilizamos el siguiente comando:

$ groups parrot
parrot : parrot
$ sudo usermod -a -G hackers parrot
parrot : parrot hackers


En el ejemplo anterior, utilizamos el comando groups para mostrar el grupo actual de la cuenta del usuario parrot, el grupo actual es parrot, para agregarlo como miembro en otro grupo, se utiliza el comando usermod con los parametros -a y -G esto nos permite conservar la membresía en el grupo original (parrot).


## Uso del comando change

El comando change nos permite modificar los ajustes de la cuenta relacionados con la caducidad de la misma. Es posible configurar las cuentas en Linux de modo que expiren automáticamente si se cumplen dos condiciones:

1 - La contraseña no se ha cambiado en un periodo de tiempo especificado.

2 - La fecha del sistema ha pasado un tiempo predeterminado.
Estos ajustes se controlan a través de la utilidad change, su sintaxis es la siquiente:

change [-l] [-m dias_minimos] [-M dias_máximos] [-d último_dia] [-I dias_inactivos] 
	[-E fecha_de_expiración] [-W advertencia] nombre_de_usuario

Estos parámetro modifican las acciones del comando change.

- El parámetro -l hace que change muestre la caducidad de la cuentea y la información de envejecimiento de la contraseña par aun usuario particular.

- El parámetro -m establece el número mímimo de días entre los cambios de contraseña o indica que un usuario puede cambiar una contraseña varias veces en un día, indicando con números cada cuantos dias puede cambiar la contraseña el usuario, 1 indica que el usuario puede cambiar la contraseña una vez al día, 2 indica que el usuario puede cambiar la contraseña cada 2 días y así sucesíbamente.

- El parámetro -M establece el número máximo de días que puede pasar entre cambios de contraseña. 30 indica que se requiere un cambio de contraseña aproximadamente una vez al mes.

- El parámetro -d establece el último día en que se cambió una contraseña, Linux normalmente mantiene ese valor automáticamente, pero se puede utilizar este parámetro para modificar el recuento de cambio de contraseña, el último día se expresa en el formato AAAA/MM/DD o con el número de días desde el 1ro de Enero de 1970.

- El parámetro -I establece el número de días entre la caducidad de la contraseña y la cuenta, Una cuenta caducada no se puede utilizar o puede obligar al usuario a cambiar la contraseña inmediatamente después de conectarse, esto depende de la distribución.

- El parámetro -E establece una fecha de caducidad obsoleta, por ejemplo, puede utilizar -E 2017/12/31 para que una cuenta venza el 31 de Diciembre del 2017 o utilizando el número de días desde el 1ro de Enero de 1970, tambien es importante mencionar que el valor -1 no representa una fecha de caducidad.

- El parámetro -W establece e número de días antes de la expiración de la cuenta en los que el sistema debe enviar avisos sobre la caducidad inminente al usuario. Es importante mencionar que estos avisos solo se muestran a usuarios de inicio de sesión en modo texto, los usuarios que inician sesión desde la GUI por lo general no ven estos avisos.


## Descripción de los archivos de configuración de las cuentas.

Se pueden modificar directamente los archivos de condiguración del usuario, los archivos /etc/passwd y /etc/shadow controlan la mayoría de las características básicas de una cuenta. AMbos archivos constan de un conjunto de registros, una línea por registro de cuenta. Cada registro comienza con un nombre de usuario y continua con un conjunto de campos determinados por dos puntos (:). Muchos de estos artículos pueden ser modificados con usermod o passwd. Una entrada de registro típica de /etc/passwd es similar a la siguiente:

user:x:1000:1000:User,,,:/home/user:/bin/bash

Cada uno de los campos tiene un significado específico:

- El primer campo en cada línea de /etc/passwd es el nombre de usuario (en este caso user).

- El segundo campo se ha reservado tradicionalmente para la contraseña, sin embargo en la mayoria de los sistemas Linux hoy en día se utiliza un sistema de contraseñas de sombra en en que la contraseña se almacena en /etc/shadow, la x en el campo de contraseña de ejemplo indica que las contraseñas de sombra están en uso. En un sistema que no utiliza contraseñas sombra, en su lugar aparece una contraseña hash.

- La siguiente contraseña es el ID de la cuenta de usuario (1000 en este ejemplo).

- El ID de grupo de inicio de sesión predeterminado es el siguiente en la línea /etc/passwd, en este caso el GID primario es 1000.

- El campo de comentario puede tener diferentes contenidos en sistemas diferentes, en el ejemplo anterior, el nombre completo del usuario (User), en algunos sistemas (como es el caso de Parrot) se coloca información adicional separada por comas, esta información puede incluir numeros de telefonos del usuario, oficina o departamente, título o puesto que ocupa, etc.

- El directorio principal del usuario, /home/user, es el siguiente campo de datos en el registro.

- La Shell predeterminada es el último elemento de cada registro de /etc/passwd, normalmente /bin/bash o alguna otra Shell de comandos común. También es posible crear cuentas de usuario con un shell por defecto como /bin/false, lo cual impide que los usuarios incien sesión como usuarios comunes pero dejando intactas otras utilidades del sistema como resivir correos electronicos utilizando protocolos de recuperación como POP, IMAP... otro ejemplo interesante es utilizar /bin/passwd para que los usuarios puedan cambiar su contraseña de forma remota pero no puedan iniciar sesión utilizando una shell de comandos.


Normalmente una entrada de registro en /etc/shadow se parece a la siguiente:

user:$6$vocRBGox$Nngt/9rYAijBogufMToHFdb3xzD/7J0GLp9.67/WM82mSsfb3FuC0:17450:0:99999:7:-1:-1:

La mayoría de los campos corresponden a las opciones establecidas con la herramienta change, otras se establecen con passwd, useradd o usermod. A continuación se explica el significado de cada uno de los campos.

- Cada línea comienza con el nombre de usuario, se puede notar como el UID no utiliza en el archivo /etc/shadow. El nombre de usuario vincula las entradas de este archivo a las de /etc/passwd.

- El siguiente campo, corresponde a las contraseñas, estas se almacenan en forma de hash, por lo que no se parecen en nada a la contraseña real. Un asterisco (*) en el campo de contraseña,indica que esta cuenta no acepta inicios de sesión. Un signo de exclamación (!) delante del hash de la contraseña, indica que la cuenta está bloqueada (esto se produce al bloquear la cuenta con la herramienta "usermod -L" al utilizar el parametro -L, sea agrega este simbolo de exclamación para evitar que el usuario inicie sesión, pero se mantiene la contraseña original). Dos signos de exclamación (!!), indican que no se a establecido una contraseña para la cuenta, esto tambien puede indicar que la cuenta no acepta inicios de sesión.

- El siguiente campo (17450) indica la fecha del último cambio de contraseña, se indica en número de días desde el 1ro de Enero de 1970.

- El siguiente campo (0) es el número de días antes de que se permita un cambio de contraseña.

- El siguiente campo (99999) indica el número de días (después de la última modificación de la contraseña) en los que se requiere un cambio de contraseña.

- El siguiente campo (7) indica la cantidad dias en que se va a comenzar a notificar sobre la expiración de la contraseña.

- Normalmente en un sistema configurado para caducar las cuentas, nos encontramos con 3 campos más, Dias entre la Expiración y la Desactivaciòn de la cuenta (-1 en el ejemplo anterior), Dia de Expiración de la cuenta (-1 en el ejemplo anterior) y el último campo esta reservado para un uso futuro, por lo general en todos los sistemas Linux no se usa o contiene un valor sin sentido.


  Analizando detenidamente lo anterior, podemos afirmar que las cuentas de usuario pueden ser modificadas editando directamente estos archivos (/etc/passwd y /etc/shadow). Los valores -1 y 99999 indican que dicho campo ha sido desactivado, esta inutilizado. Realmente se recomienda realizar estas modificaciones usando las herramientas usermod y change ya que es mucho más dificil realizar estos cambios directamente en los archivos de configuración ya que se pueden cometer errores como: desactivar una cuenta antes o despues del tiempo requerido, cometer un error a la hora de calcular los días desde el 1ro de Enero de 1970, omitir años bisiestos, etc.

  Algo similar sucede al tratar de modificar en hash de la contraseña, este no puede ser editado de forma efectiva exepto a traves de la herramienta passwd o herramientas similares. Se puede copiar y pegar un valor de un archivo compatible o usar cripto, pero generalmente es mucho más fácil usar passwd, por otro lado, no es recomendable copiar hashes de contraseñas desde otros sistemas porque significa que los usuarios tendran las mismas contraseñas en ambos sistemas y este hecho será obvio para alguien que haya adquirido ambas listas de hashes.

# == Documento pendiente de revisión ==


