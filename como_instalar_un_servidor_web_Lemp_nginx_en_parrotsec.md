<h1>Procedimiento de instalación de un servidor web Lemp Nginx en ParrotSec</h1>



#
<h2>Que es LEMP?</h2>


Puede encontrar cual es esta combinación de estas tecnologías usada principalmente para definir

la infraestructura de un servidor web, utilizando un paradigma de programación para el desarrollo. 

En --> <a href="https://en.wikipedia.org/wiki/LAMP_%28software_bundle%29" target="blank">LEMP</a>


#
Antes de todo necesita tener acceso root

<a href="https://github.com/josegatica/parrot-docu-es/blob/master/set_root_pw.md" target="blank">Establecer Clave Root </a>



#
Luego personalice su ParrotSec

<a href="https://github.com/josegatica/parrot-docu-es/blob/master/personalizar_ParrotSec.md" target="blank">Personalizar ParrotSec</a>.



#
La versión estable actualmente es ParrotSec 3.8

Si usted gusta puede incluir este paso para convertirse en un Beta Tester siguiendo este procedimiento:

<html><a href="https://github.com/josegatica/parrot-docu-es/blob/master/get_parrot_cloud_vps_from_kali.md">Parrot 3.9 Intruder</a></html>

Necesitará un cafe y seguir linea por linea.




#
<h2>Primer Paso</h2>



Primero vamos a deshabilitar Apache


su

systemctl stop apache2

sudo apt autoremove -y

sudo apt-get remove --purge apache2



#
Ahora instalemos LEMP con Nginx

sudo aptitude install nginx mariadb-server mariadb-client php-mysqli php7.1-common php7.1-readline php7.1-fpm php7.1-cli php7.1-gd php7.1-mysql php7.1-mcrypt php7.1-curl php7.1-mbstring php7.1-opcache php7.1-json -y php7.1-zip php-xml apt-transport-https lsb-release ca-certificates wget


#

Muy bien ahora detengamos la base de datos 

sudo /etc/init.d/mysql stop


#
Ok ahora configuremos PHP y los limites de memoria


sudo sed -i "s/memory_limit = .*/memory_limit = 256M/" /etc/php/7.1/fpm/php.ini

sudo sed -i "s/upload_max_filesize = .*/upload_max_filesize = 128M/" /etc/php/7.1/fpm/php.ini

sudo sed -i "s/zlib.output_compression = .*/zlib.output_compression = on/" /etc/php/7.1/fpm/php.ini

sudo sed -i "s/max_execution_time = .*/max_execution_time = 18000/" /etc/php/7.1/fpm/php.ini

sudo mv /etc/php/7.1/fpm/pool.d/www.conf /etc/php/7.1/fpm/pool.d/www.conf.org



#
sudo nano /etc/php/7.1/fpm/pool.d/www.conf


#
[www]
user = www-data
group = www-data
listen = /run/php/php7.1-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0666
pm = ondemand
pm.max_children = 5
pm.process_idle_timeout = 10s
pm.max_requests = 200
chdir = /
#


Guardemos los cambios con
Ctr + x

Respondamos que estamos seguros pulsando
Y


Reinicie PHP para que se reflejen los cambios

sudo systemctl restart php7.1-fpm 


#

Creemos nuestro sitio web definamos la ruta donde esta ubicado en nuestro almacenamiento

y los puertos de escucha, reemplace la palaba misitio.com por su dominio y en caso de que tenga un sitio seguro

protegido por un certificado ssl, agregue el puerto 443


#
nano /etc/nginx/sites-available/misitio.com


#
server {
server_name misitio.com;
listen 80;
root /var/www/html/misitio.com;
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
index index.php;

location / {
try_files $uri $uri/ /index.php?q=$uri&$args;
}

location ~* \.(jpg|jpeg|gif|css|png|js|ico|html)$ {
access_log off;
expires max;
}

location ~ /\.ht {
deny all;
}

location ~ \.php$ {
fastcgi_index index.php;
fastcgi_keep_conn on;
include /etc/nginx/fastcgi_params;
fastcgi_pass unix:/run/php/php7.1-fpm.sock;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
}
}
#


Guardemos los cambios
Ctrl + x
Y


#


Activemos nuestro sitio web en Nginx haciendo un link

sudo ln -s /etc/nginx/sites-available/misitio.com /etc/nginx/sites-enabled/misitio.com


#
Creemos la carpeta para nuestro sitio web

sudo mkdir -p /var/www/html/misitio.com



#
Demos los permisos de escritura

sudo chown www-data: -R /var/www/html/misitio.com

sudo chown www-data: -R /var/www/html



#
Descargue y descomprima la version que prefiera de Wordpress, la version estable es Wordpress Version Estable 4.8.2.

Esta procedimiento se realizo con la Wordpress development version (4.9-beta4-42031)



#
Wordpress Version Estable 4.8.2

sudo wget https://wordpress.org/latest.zip

sudo unzip latest.zip -d /var/www/html/misitio.com


#

Wordpress nightly-builds

https://wordpress.org/nightly-builds/wordpress-latest.zip

sudo unzip wordpress-latest.zip -d /var/www/html/misitio.com


#
Wordpress development version (4.9-beta4-42031)

sudo wget https://wordpress.org/wordpress-4.9-beta4.zip

sudo unzip wordpress-4.9-beta4.zip -d /var/www/html/misitio.com


#

cd /var/www/html/misitio.com/wordpress

mv -v * /var/www/html/misitio.com

cd ..

rm -r wordpress



#

Creemos la base de datos el usuario y la clave, reemplace la palabra SuClaveSegura

Por la clave que usted defina


sudo /etc/init.d/mysql stop

sudo /etc/init.d/mysql start

sudo mysqld_safe --skip-grant-tables &

mysql -uroot

use mysql;

update user set password=PASSWORD("SuClaveSegura") where User='root';

flush privileges;

quit

sudo /etc/init.d/mysql stop

sudo /etc/init.d/mysql start

mysql -u root -p

CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'SuClaveSegura';

CREATE DATABASE wp_database;

GRANT ALL ON `wp_database`.* TO `wpuser`@`localhost`;

flush privileges;

exit;



#
Reiniciemos mysql para que se apliquen los cambios.

sudo /etc/init.d/mysql restart

mysqladmin -u root -p status

ps aux | grep -i sql

rm /etc/mysql/my.cnf


#

Active los servicios web para que inicien de forma automatica con el sistema

systemctl enable php7.1-fpm.service

update-rc.d postgresql enable

update-rc.d nginx enable

update-rc.d mysql enable


#
Creemos el archivo de configuracion de Wordpress 

cp wp-config-sample.php wp-config.php


Documente el archivo con el nombre de la base de datos el usuario y la clave definida por usted

nano wp-config.php

Ctrl + x
Y


#
Verifique que los servicios Web y las bases de datos esta operando correctamente.

sudo service php7.1-fpm status -l

sudo service nginx status -l

sudo service mysql status -l

sudo service postgresql status -l


#
Apunte su sitio Web hacia la ruta que usted creo

nano /etc/nginx/sites-available/default

La linea 42 reemplazela por:

root /var/www/html/misitio.com;

Ctrl + x
Y


#
Apliquemos los cambios a Nginx

sudo nginx -t

sudo systemctl restart nginx


#
Reiniciemos nuestra maquina

sudo reboot


#

Verifique que el asistente de instalacion de wordpress esta cargando en su maquina

Abra su navegador e ingrese a su equipo local

http://127.0.0.1

Si ve el logo de Wordpress y la lista de idiomas para instalar este CMS todo salio bien

y ahora usted tiene su propio servidor web.

Asegurese de qeu suversion de wordpress quede actualizada ingresando a

misitio.com/wp-admin/update-core.php


<html><img src="https://github.com/4k4xs4pH1r3/parrot-docu-es/blob/master/images/lemp_wp1.png"></html>


#

<html><img src="https://github.com/4k4xs4pH1r3/parrot-docu-es/blob/master/images/lemp_wp2.png"></html>


#

<html><img src="https://github.com/4k4xs4pH1r3/parrot-docu-es/blob/master/images/lemp_wp3.png"></html>


#

<html><img src="https://github.com/4k4xs4pH1r3/parrot-docu-es/blob/master/images/lemp_wp4.png"></html>



#
Ahora vamos a Instalar wp cli para gestionar Wordpress desde la terminal 

cd

curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar

hp wp-cli.phar --info

chmod +x wp-cli.phar

sudo mv wp-cli.phar /usr/local/bin/wp

wp --info


#
wget https://raw.githubusercontent.com/wp-cli/wp-cli/master/utils/wp-completion.bash

nano ~/.bash_profile

source /root/wp-completion.bash

Ctr + x
Y


#
cd /var/www/html/misitio.com


#
Cerremos la sesion de root dado que wp cli no lo remcomienda hacer con este usuario

exit

wp cli update

wp cli info

wp cli version

wp plugin update --all
