Con este procedimiento vamos a obtener Parrot Cloud convirtiendo desde Kali 2017.2

Copie esta combinación en su terminal de Parrot

También nos convertiremos en Beta Testers pues tendremos la versión Intruder 3.9


echo -e "deb http://mirrordirector.archive.parrotsec.org/parrot parrot main contrib non-free" > /etc/apt/sources.list.d/parrot.list 	echo -e "# This file is empty, feel free to add here your custom APT repositories\n\n# The standard Parrot repositories are NOT here. If you want to\n# edit them, take a look into\n#                      /etc/apt/sources.list.d/parrot.list\n#                      /etc/apt/sources.list.d/debian.list\n\n\n\n# If you want to change the default parrot repositories setting\n# another localized mirror, then use the command parrot-mirror-selector\n# and see its usage message to know what mirrors are available\n\n\n\n#uncomment the following line to enable the Parrot Testing Repository\n#deb http://us.repository.frozenbox.org/parrot testing main contrib nonfree" > /etc/apt/sources.list 	

wget -qO - https://archive.parrotsec.org/parrot/misc/parrotsec.gpg | apt-key add -


nano /etc/apt/sources.list

Comente la primera línea agregando este símbolo "#"

Ahora guardemos los cambios con

Ctrl + x

Y respondemos que estamos seguros de realizar los cambios

Yes

#

ahora 

nano /etc/apt/sources.list.d/parrot.list

Copie esta línea 

deb http://deb.parrotsec.org/parrot stable main contrib non-free #deb-src http://archive.parrotsec.org/parrot stable main contrib non-free

Ahora guardemos los cambios con

Ctrl + x

Y respondemos que estamos seguros de realizar los cambios

Yes


sudo apt-get autoclean 

sudo apt-get clean

sudo apt-get update -y

apt-get install parrot-archive-keyring -y



sudo apt autoremove -f -y

apt-get install apt-parrot -y (2 Veces)

apt-get -y -o Dpkg::Options::="--force-overwrite" install parrot-interface parrot-interface-full parrot-tools-full


sudo apt-get update -y

sudo apt-get upgrade -y

sudo apt-get -y dist-upgrade -f

sudo apt autoremove -f -y

sudo dpkg --configure -a

sudo apt-get update && sudo apt-get dist-upgrade -f -y

sudo apt-get autoclean $$ apt-get clear cache

apt-get install software-properties-common -y

apt-get install apt-file -y

apt-file update

apt-get upgrade -y

apt autoremove -y

sudo apt-get update --fix-missing

sudo apt-get install --fix-broken && sudo apt-get autoremove && sudo apt-get update -y

apt-get install curl -y

sudo apt-get update && apt-get upgrade -y -f

sudo apt-get install aptitude -y && sudo aptitude safe-upgrade -y

sudo dpkg --configure -a

sudo grub-mkconfig

sudo reboot



si todo salió bien usted debe ver esto al ejecutar el comando

lsb_release-a




Distributor ID: Parrot
Description:    Parrot 3.9 - Intruder
Release:        3.9
Codename:       intruder



Esta idea está basada en esta https://dev.parrotsec.org/parrot/alternate-install/blob/master/parrot-install.sh

Y  creada con el apoyo del grupo Documentación Parrot OS Español
