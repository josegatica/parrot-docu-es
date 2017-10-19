Edite su repositorio, mayor detalle en https://mirrors.ocf.berkeley.edu/parrot

nano /etc/apt/sources.list.d/parrot.list


## stable repository
deb http://deb.parrotsec.org/parrot stable main contrib non-free
#deb-src http://archive.parrotsec.org/parrot stable main contrib non-free


Ctrl + x
Yes

Ahora linea por linea hasta que su sistema quede lo mas actualizado posible
repitalo hasta que su sistema quede volando y se resuelvan los problemas



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
