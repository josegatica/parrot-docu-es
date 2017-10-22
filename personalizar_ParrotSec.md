Edite su repositorio, mayor detalle en https://mirrors.ocf.berkeley.edu/parrot

sudo nano /etc/apt/sources.list.d/parrot.list
(Ahora revisaremos que esté como debe estar, para el correcto funcionamiento de nuestra distro)

Debería lucir así:<p>
<i>## stable repository<p>
deb http://deb.parrotsec.org/parrot stable main contrib non-free<p>
#deb-src http://archive.parrotsec.org/parrot stable main contrib non-free</i>

Ctrl + x

Yes

Ahora línea por línea hasta que su sistema quede lo más actualizado posible.
Repítalo hasta que su sistema se muestre óptimo y se resuelvan los problemas.


dpkg -l

apt list --upgradable

sudo apt-get remove --purge

sudo apt-get autoclean 

sudo apt autoremove -f -y

sudo apt-get clean

sudo apt-get update -y

sudo apt-get -y dist-upgrade -f

sudo dpkg --configure -a

sudo apt-get update && sudo apt-get dist-upgrade -f -y

sudo apt-get update && sudo apt-get dist-upgrade --forces-yes

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
