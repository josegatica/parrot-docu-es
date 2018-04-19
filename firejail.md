# Firejail

## Introducción

Firejail es un programa que reduce el riesgo de posibles brechas de seguridad en las aplicaciones que controla. Es un programa de tipo Sandbox, es decir aísla el programa del resto del sistema operativo otorgándole memoria, espacio en disco, restringiendo el acceso a ciertos dispositivos o directorios del sistema...

Para crear este sandbox ("caja de arena"), firejail utiliza varios mecanismos que le provee el kernel de linux como puede ser SUID, Linux Namespaces o seccomp-bpf.

Un proceso lanzado por firejail tendrá su propia vista privada de los recursos compartidos del kernel, como pueden ser los recursos de red, la tabla de procesos, la tabla de montajes... De esta forma no podrá acceder a partes críticas del sistema. El proceso será encapsulado en su propio sandbox("caja de arena"). Esto es muy útil (y seguro), para utilizar con navegadores web, clientes de correo, etc... Aunque también se puede utilizar para "enjaular" servidores, aplicaciones gráficas e incluso sesiones de login.

Firejail puede ejecutarse en cualquier sistema que utilice un kernel igual o superior a 3.x.

ParrotSec trae instalado firejail por defecto. Por lo tanto, no tenemos que preocuparnos de su instalación. Y su ejecución es muy sencilla, tanto que para una gran cantidad de programas no deberemos hacer nada más que pulsar el botón lanzador de la aplicación, o ejecutarla desde la línea de comandos, como por ejemplo firefox.


Nota: Debemos tener en cuenta que los programas SUID (firejail lo es) son considerados peligrosos en sistemas multiusuario. Si usted tiene un servidor lleno de usuarios conectándose por ssh, lo mejor que puede hacer es desinstalar este sandbox.



## Utilizando firejail

Si hubiésemos instalado firejail, se podría lanzar (En su distribución ParrotSec no es necesario!!!) la instrucción "sudo firecfg" para que todos los perfiles que existan dentro del directorio de configuración firejail (/etc/firejail) generen un enlace simbólico en el directorio /usr/local/bin que apunten a /usr/bin/firejail. Los desarrolladores de ParrotSec ya lo han hecho por nosotros. O al menos han "linkado" las aplicaciones más comunes.

        $ls -la /usr/local/bin/
        total 12
        drwxrwsr-x  2 root staff 4096 Apr 18 11:54 .
        drwxrwsr-x 10 root staff 4096 Feb  8 12:42 ..
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 VirtualBox -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 apktool -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 arduino -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 atril -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 claws-mail -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 conky -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 cvlc -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 dex2jar -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 display -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 dnsmasq -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 eom -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 evince -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 exiftool -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 ffmpeg -> /usr/bin/firejail
        lrwxrwxrwx  1 root staff   17 Apr 18 11:54 firefox -> /usr/bin/firejail
        ...
        ...
        ...
        ...




Si usted no quiere que una aplicación (o todas) se lance dentro de firejail, simplemente debe borrar este enlace simbólico. Justamente lo contrario, si desea que alguna aplicación se lance por medio de firejail, usted simplemente debe crear un enlace simbólico con el nombre de su aplicación apuntando a /usr/bin/firejail. No olvide generar antes el perfil que controlará a su aplicación (lo veremos más adelante).

        $ sudo rm /usr/local/bin/nombre_programa (El programa dejará de lanzarse dentro de firejail)
        $ sudo ln -s /usr/bin/firejail /usr/local/bin/nombre_programa (El programa se ejecutará dentro de firejail)

Si usted desea utilizar una aplicación fuera de firejail momentáneamente, no necesita eliminar el enlace completamente. Abra una terminal y compruebe dónde está el programa que quiere lanzar:

        $ whereis firefox
        firefox: /usr/bin/firefox /usr/lib/firefox /etc/firefox /usr/local/bin/firefox /usr/share/firefox /usr/share/man/man1/firefox.1.gz

En el resultado del comando anterior podemos ver que nuestro sistema encuentra entre otros: 
-	/usr/bin/firefox (que es el ejecutable real de nuestro programa firefox) 
-	/usr/local/bin/firefox (que es el enlace simbólico que hace que firefox se lance dentro de firejail). 

Simplemente debemos ejecutar en una terminal el comando con su ruta completa hacia el binario, es decir, en nuestro caso de firefox, /usr/bin/firefox. De esta forma, habrá lanzado Firefox fuera de firejail, pero la próxima vez que lance firefox mediante el método normal (pulsando el lanzador en su escritorio o escribiendo en una terminal “firefox”) volverá a ejecutarse dentro de firejail. 

Si lo que desea es comprobar cómo se comporta una aplicación dentro de firejail podría ejecutar:

        $ firejail nombre_programa

Nota: Antes de ejecutar el comando anterior no olvide crear el perfil del programa en /etc/firejail/nombre_programa.profile


## Perfiles firejail

En el punto anterior, hemos hablado de los perfiles de firejail. Existe un perfil por cada programa que se lanza dentro de firejail. Estos perfiles le indican a firejail que propiedades y características debe activar para el programa lanzado: que directorios puede ver, en cuales puede escribir, si debe crear ficheros y/o directorios temporales, si debe ejecutarse en un entorno privado, etc…  Estos perfiles se encuentran ubicados en el directorio /etc/firejail. Como podrá observar, existe al menos un perfil por cada aplicación que se lanza dentro de firejail. Los nombres de estos archivos son "nombre_programa.profile".


        $ ls -al /etc/firejail/
        total 1816
        drwxr-xr-x   2 root root 24576 Mar 21 17:37 .
        drwxr-xr-x 222 root root 12288 Apr 18 11:54 ..
        -rw-r--r--   1 root root   894 Jan  8 23:05 0ad.profile
        -rw-r--r--   1 root root   691 Jan  8 23:05 2048-qt.profile
        -rw-r--r--   1 root root   399 Jan  8 23:05 7z.profile
        -rw-r--r--   1 root root   583 Jan  8 23:05 Cryptocat.profile
        -rw-r--r--   1 root root   144 Jan  8 23:05 Cyberfox.profile
        -rw-r--r--   1 root root   146 Jan  8 23:05 FossaMail.profile
        -rw-r--r--   1 root root   140 Jan  8 23:05 Gitter.profile
        -rw-r--r--   1 root root   742 Jan  8 23:05 Mathematica.profile
        -rw-r--r--   1 root root   140 Jan  8 23:05 Natron.profile
        -rw-r--r--   1 root root   144 Jan  8 23:05 Telegram.profile
        -rw-r--r--   1 root root   665 Jan  8 23:05 Thunar.profile
        -rw-r--r--   1 root root   833 Jan  8 23:05 Viber.profile
        -rw-r--r--   1 root root   148 Jan  8 23:05 VirtualBox.profile
        -rw-r--r--   1 root root   136 Jan  8 23:05 Wire.profile
        -rw-r--r--   1 root root  1047 Jan  8 23:05 Xephyr.profile
        -rw-r--r--   1 root root  1249 Jan  8 23:05 Xvfb.profile
        ...
        ...
        ...
        ...

La lista de perfiles preparados para la utilización de firejail sobrepasa la cifra de los 400. 

Estos perfiles pueden ser modificados para que nuestros programas se comporten de forma diferente a la predeterminada dentro de firejail y así satisfacer nuestras necesidades.

Para modificar un perfil, simplemente abra el fichero correspondiente al perfil del programa que quiere modificar en su editor preferido. Ahora modifique la configuración y parámetros que necesite.

Para conocer la configuración y opciones que puede utilizar en estos perfiles puede comprobar el man de firejail-profile(5).

        $ man firejail-profile



## Más información

Su sistema tiene las siguientes páginas man sobre firejail, donde podrá encontrar más información.

        firejail (1)         - Linux namespaces sandbox program
        firecfg (1)          - Desktop integration utility for Firejail software.
        firejail-login (5)   - Login file syntax for Firejail
        firejail-profile (5) - Security profile file syntax for Firejail
        firemon (1)          - Monitoring program for processes started in a Firejail sandbox.



También puede visitar la página oficial de firejail:

https://firejail.wordpress.com/

