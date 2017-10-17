# NetworkManager

En Parrot la configuración de las interfaces de red se maneja por medio de un demonio de sistema llamado NetworkManager.
Para comprobar si tenemos activado NetworkManager:

	┌─[root@parrot]─[~]
	└──╼ #systemctl status NetworkManager
	● NetworkManager.service - Network Manager
   	Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
   	Active: active (running) since Mon 2017-10-09 15:39:51 CEST; 6 days ago
    	 Docs: man:NetworkManager(8)
	 Main PID: 1010 (NetworkManager)
	    Tasks: 3 (limit: 4915)
	   CGroup: /system.slice/NetworkManager.service
	           └─1010 /usr/sbin/NetworkManager --no-daemon


Para NetworkManager:
- Un "device" es una interfaz de red
- Un "connection" es una colección de parametros que se pueden configurar para un "device"
- Sólo se puede activar una conexión para cada "device" a la vez
- Cada conexión tiene un nombre o ID que lo identifica
- Las conexiones y configuraciones se guardan en /etc/NetworkManager/system-connections, en un archivo con el nombre de la conexión
- Podemos utilizar la utilidad nmcli para crear y editar conexiones desde la línea de comandos



El comando "nmcli dev status" nos mostrará el estado de las interfaces de red:

	┌─[root@parrot]─[~]
	└──╼ #nmcli dev status
	DEVICE  TYPE      STATE      CONNECTION         
	eth0    ethernet  connected  Wired connection 1 
	lo      loopback  unmanaged  --  




El comando "nmcli con show" nos mostrará una lista de todas las conexiones. Para listar sólo las conexiones activas añadiremos la opción --active:


	┌─[root@parrot]─[~]
	└──╼ #nmcli con show
	NAME                UUID                                  TYPE            DEVICE 
	Wired connection 1  c0277ac0-d15c-3835-b31d-24d0518e7359  802-3-ethernet  eth0 





El comando "ip addr show" nos muestra la configuración actual de nuestras interfaces de red. Para mostrar tan sólo una de las interfaces, deberemos añadir el nombre de nuestra interfaz como último argumento del comando:

	┌─[root@parrot]─[~]
	└──╼ #ip addr show eth0
	2: eth0: <BROADCAST,MULTICAST,UP(1),LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    		(2) link/ether 52:54:00:fc:b0:e2 brd ff:ff:ff:ff:ff:ff
    		(3) inet 10.0.200.91/24 brd 10.0.200.255 scope global eth0
       			valid_lft forever preferred_lft forever
    		(4) inet6 fe80::b601:aab4:1422:f537/64 scope link 
       			valid_lft forever preferred_lft forever



- (1) UP. Una interfaz activa que está levantada
- (2) link/ether. Esta línea nos muestra la dirección hardware(MAC) del dispositivo
- (3) inet. Aquí vemos la dirección IPV4
- (4) inet6. Aquí vemos la dirección IPV6



## Añadiendo una conexión de red

El comando "nmcli con add" se utiliza para añadir nuevas conexiones de red. En el ejemplo que veremos se asume que el nombre de la conexión que se va a añadir no está siendo utilizada.   
El siguiente comando añadirá una nueva conexión (con_dhcp) para nuestra interfaz eth0, la cual obtendrá la información de red utilizando DHCP y se autoconectará en el inicio del sistema.
A continuación comprobaremos su configuración desde el propio nmcli como mirando su fichero de configuración:

	┌─[root@parrot]─[~]
	└──╼ #nmcli con add con-name con_dhcp type ethernet ifname eth0
	Connection 'con_dhcp' (717ca962-bcd7-4371-b1bd-9a8eb6531244) successfully added.    


	┌─[root@parrot]─[~]
	└──╼ #nmcli con show con_dhcp 
	connection.id:                          con_dhcp
	connection.uuid:                        717ca962-bcd7-4371-b1bd-9a8eb6531244
	connection.stable-id:                   --
	connection.interface-name:              eth0
	connection.type:                        802-3-ethernet
	connection.autoconnect:                 yes
	connection.autoconnect-priority:        0
	connection.autoconnect-retries:         -1 (default)
	connection.timestamp:                   0
	connection.read-only:                   no
	connection.permissions:                 --
	connection.zone:                        --
	connection.master:                      --
	connection.slave-type:                  --
	connection.autoconnect-slaves:          -1 (default)
	connection.secondaries:                 --
	connection.gateway-ping-timeout:        0
	connection.metered:                     unknown
	connection.lldp:                        -1 (default)
	802-3-ethernet.port:                    --
	802-3-ethernet.speed:                   0
	802-3-ethernet.duplex:                  --
	802-3-ethernet.auto-negotiate:          no
	802-3-ethernet.mac-address:             --
	802-3-ethernet.cloned-mac-address:      --
	802-3-ethernet.generate-mac-address-mask:--
	802-3-ethernet.mac-address-blacklist:   --
	802-3-ethernet.mtu:                     auto
	802-3-ethernet.s390-subchannels:        --
	802-3-ethernet.s390-nettype:            --
	802-3-ethernet.s390-options:            --
	802-3-ethernet.wake-on-lan:             1 (default)
	802-3-ethernet.wake-on-lan-password:    --
	ipv4.method:                            auto
	ipv4.dns:                               --
	ipv4.dns-search:                        --
	ipv4.dns-options:                       (default)
	ipv4.dns-priority:                      0
	ipv4.addresses:                         --
	ipv4.gateway:                           --
	ipv4.routes:                            --
	ipv4.route-metric:                      -1
	ipv4.ignore-auto-routes:                no
	ipv4.ignore-auto-dns:                   no
	ipv4.dhcp-client-id:                    --
	ipv4.dhcp-timeout:                      0
	ipv4.dhcp-send-hostname:                yes
	ipv4.dhcp-hostname:                     --
	ipv4.dhcp-fqdn:                         --
	ipv4.never-default:                     no
	ipv4.may-fail:                          yes
	ipv4.dad-timeout:                       -1 (default)
	ipv6.method:                            auto
	ipv6.dns:                               --
	ipv6.dns-search:                        --
	ipv6.dns-options:                       (default)
	ipv6.dns-priority:                      0
	ipv6.addresses:                         --
	ipv6.gateway:                           --
	ipv6.routes:                            --
	ipv6.route-metric:                      -1
	ipv6.ignore-auto-routes:                no
	ipv6.ignore-auto-dns:                   no
	ipv6.never-default:                     no
	ipv6.may-fail:                          yes
	ipv6.ip6-privacy:                       -1 (unknown)
	ipv6.addr-gen-mode:                     stable-privacy
	ipv6.dhcp-send-hostname:                yes
	ipv6.dhcp-hostname:                     --
	ipv6.token:                             --
	proxy.method:                           none
	proxy.browser-only:                     no
	proxy.pac-url:                          --
	proxy.pac-script:                       --     




	┌─[root@parrot]─[~]
	└──╼ #cat /etc/NetworkManager/system-connections/con_dhcp 
	[connection]
	id=con_dhcp
	uuid=717ca962-bcd7-4371-b1bd-9a8eb6531244
	type=ethernet
	interface-name=eth0
	permissions=

	[ethernet]
	mac-address-blacklist=

	[ipv4]
	dns-search=
	method=auto

	[ipv6]
	addr-gen-mode=stable-privacy
	dns-search=
	method=auto




Veamos ahora un ejemplo con una configuración para la misma interfaz eth0, pero con una dirección ip 192.168.0.3/24 y el gateway por defecto 192.168.0.254. El nombre de la conexión la llamaremos con_static.

	+-[root@parrot]-[~]
	+--? #nmcli con add con-name con_static type ethernet ifname eth0 ip4 192.168.0.3/24 gw4 192.168.0.254
	Connection 'con_static' (12dc18a5-ef1c-4926-8cc7-82cbf0acaa25) successfully added.



## Controlando las conexiones de red


El comando "nmcli con up <nombre_conexion>" activará la conexión "nombre_conexion" en la interfaz a la que este asociada dicha configuración. Recuerde que el comando toma como argumento el nombre de la conexión y no el nombre de la interfaz.


	┌─[root@parrot]─[~]
	└──╼ #nmcli con up con_static


El comando "nmcli dev disconnect <device>", desconectará el interfaz de red <device>. Este comando se puede abreviar de la siguiente forma "nmcli dev dis <device>".


	┌─[root@parrot]─[~]
	└──╼ #nmcli dev dis eth0

Use este comando para desactivar una interfaz de red.




## Modificando la configuración de red

Para comprobar la configuración actual de una conexión ejecutamos "nmcli con show <nombre_conexion>".

	┌─[root@parrot]─[~]
	└──╼ #nmcli con show con_static
	connection.id:                          con_static
	connection.uuid:                        12dc18a5-ef1c-4926-8cc7-82cbf0acaa25
	connection.stable-id:                   --
	connection.interface-name:              eth0
	connection.type:                        802-3-ethernet
	connection.autoconnect:                 yes
	connection.autoconnect-priority:        0
	connection.autoconnect-retries:         -1 (default)
	connection.timestamp:                   0
	connection.read-only:                   no
	connection.permissions:                 --
	connection.zone:                        --
	connection.master:                      --
	connection.slave-type:                  --
	connection.autoconnect-slaves:          -1 (default)
	connection.secondaries:                 --
	connection.gateway-ping-timeout:        0
	connection.metered:                     unknown
	connection.lldp:                        -1 (default)
	802-3-ethernet.port:                    --
	802-3-ethernet.speed:                   0
	802-3-ethernet.duplex:                  --
	802-3-ethernet.auto-negotiate:          no
	802-3-ethernet.mac-address:             --
	802-3-ethernet.cloned-mac-address:      --
	802-3-ethernet.generate-mac-address-mask:--
	802-3-ethernet.mac-address-blacklist:   --
	802-3-ethernet.mtu:                     auto
	802-3-ethernet.s390-subchannels:        --
	802-3-ethernet.s390-nettype:            --
	802-3-ethernet.s390-options:            --
	802-3-ethernet.wake-on-lan:             1 (default)
	802-3-ethernet.wake-on-lan-password:    --
	ipv4.method:                            manual
	ipv4.dns:                               --
	ipv4.dns-search:                        --
	ipv4.dns-options:                       (default)
	ipv4.dns-priority:                      0
	ipv4.addresses:                         192.168.0.3/24
	ipv4.gateway:                           192.168.0.254
	ipv4.routes:                            --
	ipv4.route-metric:                      -1
	ipv4.ignore-auto-routes:                no
	ipv4.ignore-auto-dns:                   no
	ipv4.dhcp-client-id:                    --
	ipv4.dhcp-timeout:                      0
	ipv4.dhcp-send-hostname:                yes
	ipv4.dhcp-hostname:                     --
	ipv4.dhcp-fqdn:                         --
	ipv4.never-default:                     no
	ipv4.may-fail:                          yes
	ipv4.dad-timeout:                       -1 (default)
	ipv6.method:                            auto
	ipv6.dns:                               --
	ipv6.dns-search:                        --
	ipv6.dns-options:                       (default)
	ipv6.dns-priority:                      0
	ipv6.addresses:                         --
	ipv6.gateway:                           --
	ipv6.routes:                            --
	ipv6.route-metric:                      -1
	ipv6.ignore-auto-routes:                no
	ipv6.ignore-auto-dns:                   no
	ipv6.never-default:                     no
	ipv6.may-fail:                          yes
	ipv6.ip6-privacy:                       -1 (unknown)
	ipv6.addr-gen-mode:                     stable-privacy
	ipv6.dhcp-send-hostname:                yes
	ipv6.dhcp-hostname:                     --
	ipv6.token:                             --
	proxy.method:                           none
	proxy.browser-only:                     no
	proxy.pac-url:                          --
	proxy.pac-script:                       --



Podemos utilizar el comando "nmcli con mod <nomre_conexion>" para modificar su configuración. Las diferentes configuraciones que podemos utilizar las podemos ver en la página del manual nm-settings(5).
Por ejemplo, imaginemos que queremos cambiar la conexión "con_static" para que se utilice la dirección 192.168.1.3/24 y el gateway en 192.168.1.1, en vez de la que configuramos anteriormente.

	┌─[root@parrot]─[~]
	└──╼ #nmcli con mod con_static ipv4.addresses "192.168.1.3/24" ipv4.gateway 192.168.1.1



Es importante que si cambiamos una conexión tipo DHCP a estática, modifiquemos también el parametro ipv4.method de auto a manual.

Tras los cambios deberemos ejecutar "nmcli con reload <nombre_conexion>" para que estos tomen efecto.



## Modificando resolv.conf

Para cambiar los resolutores de nombres, es decir, aquellos servidores que nos traducen nombres a ip, podemos modificar el archivo /etc/resolv.conf.
Abrimos este fichero con nuestro editor favorito y debemos combrobar que tenemos al menos una línea que comience por nameserver seguido de una ip. Esta ip indicará el servidor DNS que uilizaremos.

	┌─[root@parrot]─[~]
	└──╼ #cat /etc/resolv.conf
	# ParrotDNS
	nameserver 92.222.97.145
	nameserver 192.99.85.244

	# OpenNIC
	nameserver 185.121.177.177

	# Round Robin
	options rotate



## Borrando una conexión de red
El comando "nmcli con del <nombre_conexion>" borrará la conexión que le indiquemos.


## Modificando el nombre de nuestro sistema
El comando "hostname" muestra o cambia temporalmente el nombre de nuestro sistema.

	┌─[root@parrot]─[~]
	└──╼ #hostname
	parrot


Podemos modificar el fichero /etc/hostname para cambiar el nombre de nuestro sistema.  

También podemos utilizar el comando hostnamectl, con la opción set-hostname. "hostnamectl" también nos sirve para comprobar el nombre de nuestro sistema actualmente.

	┌─[root@parrot]─[~]
	└──╼ #hostnamectl set-hostname demo.test.com
	┌─[root@parrot]─[~]
	└──╼ #hostnamectl status
   	Static hostname: demo.test.com
              Icon name: computer-vm
          	Chassis: vm
       	     Machine ID: 7c6d78cba9084717ba7d7316bf775376
           	Boot ID: 8540f57c7203421d86a26995dc2c9293
    	 Virtualization: kvm
       Operating System: Parrot GNU/Linux 3.8 JollyRoger
                 Kernel: Linux 4.12.0-parrot6-amd64
           Architecture: x86-64





## Vídeo
Por favor no se pierdan el video de nuestro compañero Xc0d3, en el que nos muestra otros métodos para configurar la red. Lo puede ver aquí:

https://www.youtube.com/watch?v=1HcGUP90eMU&index=8&list=PLUFdNvWy-5CHapOW43A2T6i0fcVBHzHy0
