Álvaro García Alcázar

Repositorio GitHub: https://github.com/agalcazar23/Monitorizacion/blob/main/Practica ARP NMAP

1.- Identificar la configuración de red

1.- Indica la IP y la interfaz de red de tu equipo.

2: eno1: <BROADCAST, MULTICAST, UP, LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
link/ether f4:b5:20:40:51:6f brd ff:ffffffffff
altname enpos31f6
altname enxf4b52040516f
inet 172.26.12.17/16 brd 172.26.255.255 scope global dynamic noprefixroute eno1
valid_lft 5124sec preferred_lft 5124sec
inet6 fe80::f6b5:20ff:fe40:516f/64 scope link noprefixroute
valid Ift forever preferred lft forever


IP: 172.26.12.17
Interfaz de red: eno1

2.- Anota la puerta de enlace predeterminada.

[Alvaro]-[PC205]:~$
ip r
default via 172.26.0.1 dev eno1 proto dhcp src 172.26.12.17 metric 100
172.17.0.0/16 dev dockero proto kernel scope link src 172.17.0.1 linkdown
172.18.0.0/16 dev br-d21e4f27ba46 proto kernel scope link src 172.18.0.1 linkdown
172.19.0.0/16 dev br-2a884c58d3a1 proto kernel scope link src 172.19.0.1
172.20.0.0/16 dev br-cbde158e095e proto kernel scope link src 172.20.0.1
172.26.0.0/165 dev eno1 proto kernel scope link src 172.26.12.17 metric 100
[Alvaro]-[PC205]:~$


Puerta de enlace: 172.26.0.0/16

3.- Indica la dirección de red y su prefijo o máscara.

Máscara de subred: 255.255.0.0

2.- Analizar y renovar una entrada ARP

arp -a

[Alvaro]-[PC205]:~$
arp a 172.26.20.3
? (172.26.20.3) en f4:b5:20:40:51:a6 [ether] en eno1


Muestra el contenido actual de la caché ARP del sistema en formato estilo BSD. Por defecto, intenta resolver los nombres de host asociados a las direcciones IP antes de mostrarlas junto a su dirección física e interfaz.

arp -n

[Alvaro]-[PC205]:~$
arp -n 172.26.20.3
Dirección TipoHW DirecciónHW Indic Máscara Interfaz
172.26.20.3 ether f4:b5:20:40:51:a6 C eno1


Muestra la tabla ARP en formato tabular estándar indicando la dirección en formato estrictamente numérico, evitando la resolución de nombres DNS.

ip neigh

[Alvaro]-[PC205]:~$
ip neigh
172.20.0.5 dev br-cbde158e095e lladdr 6a:e8:09:81:36:34 STALE
172.20.0.2 dev br-cbde158e095e lladdr 6e:4e:ed:59:5c:8f STALE
172.26.19.58 dev eno1 lladdr f4:b5:20:40:51:6e STALE
172.26.19.255 dev eno1 lladdr f4:b5:20:40:59:4a STALE
172.26.0.0 dev eno1 FAILED
172.26.0.1 dev eno1 lladdr 64:62:66:25:50:60 REACHABLE
172.26.19.183 dev eno1 lladdr f4:b5:20:40:51:c9 STALE
172.26.12.19 dev eno1 lladdr f4:b5:20:40:52:72 STALE
172.26.22.240 dev eno1 lladdr b4:22:00:41:f8:61 STALE
172.26.0.6 dev eno1 lladdr 30:16:9d:76:4c:40 STALE
172.26.20.3 dev eno1 lladdr f4:55:20:40:51:a6 STALE
172.19.0.2 dev br-2a884c58d3a1 lladdr ce:f0:4a:53:b8:29 STALE
172.26.20.6 dev eno1 lladdr f4:b5:20:40:51:a4 STALE
fe80::b622:ff:fe41:f861 dev eno1 lladdr b4:22:00:41:f8:61 STALE


Muestra la tabla de vecinos, indicando la IP, la interfaz, la dirección MAC y, como gran ventaja técnica, el estado actual de la entrada en el kernel.

[Alvaro]-[PC205]:~$
ip neigh show 172.26.20.3
172.26.20.3 dev eno1 lladdr f4:b5:20:40:51:a6 STALE
[Alvaro]-[PC205]:~$


3.- ¿Qué MAC aparece asociada a la puerta de enlace? ¿Por qué la tabla ARP no muestra todos los equipos de la red?

f4:b5:20:40:51:a6

Porque la tabla ARP es una caché dinámica y bajo demanda que además tiene un tiempo de caducidad.

4.- Genera y consulta la entrada del profesor:

ping -c 2 IP_PROFESOR

[Alvaro]-[PC205]:~$
ping -c 2 172.26.20.3
PING 172.26.20.3 (172.26.20.3) 56(84) bytes of data.
64 bytes from 172.26.20.3: icmp_seq=1 ttl=64 time=0.220 ms
64 bytes from 172.26.20.3: icmp_seq=2 ttl=64 time=0.346 ms

--- 172.26.20.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1014ms
rtt min/avg/max/mdev=0.220/0.283/0.346/0.063 ms
[Alvaro]-[PC205]:~$


ip neigh show IP_PROFESOR

[Alvaro]-[PC205]:~$
ip neigh show 172.26.20.3
172.26.20.3 dev eno1 lladdr f4:b5:20:40:51:36 STALE
[Alvaro]-[PC205]:~$


5.- Elimina únicamente esa asociación en tu ordenador:
(Sin salida proporcionada en el documento)

3.-Descubrir equipos con Nmap

1.- Utilizando el comando nmap, con la opción -sn, escanea:

[Alvaro]-[PC205]:~$
nmap -sn 172.26.0-255
Starting Nmap 7.98 (https://nmap.org) at 2026-11-12 09:23 +0100
Failed to resolve "172.26.0-255".
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 1.01 seconds


2.- Qué hace exactamente la opción -sn

La opción -sn le indica a Nmap que realice únicamente un descubrimiento de hosts activos en la red y que desactive el escaneo de puertos tras determinar qué máquinas están encendidas.

4.- Examinar puertos y servicios

1.- Escanea exclusivamente la máquina objetivo indicada por el profesor.

[Alvaro]-[PC205]:~$
nmap 172.26.20.3
Starting Nmap 7.98 (https://nmap.org) at 2026-11-12 09:25 +0100
Nmap scan report for 172.26.20.3
Host is up (0.0012s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT STATE SERVICE
22/tcp open ssh
139/tcp open netbios-ssn
445/tcp open microsoft-ds
Nmap done: 1 IP address (1 host up) scanned in 0.55 seconds
[Alvaro]-[PC205]:~$


[Alvaro]-[PC205]:~$
nmap -sV 172.26.20.3
Starting Nmap 7.98 (https://nmap.org) at 2026-11-12 09:25 +0100
Nmap scan report for 172.26.20.3
Host is up (0.00097s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 10.2p1 Ubuntu 2ubuntu3.8 (Ubuntu Linux; protocol 2.0)
139/tcp open netbios-ssn Samba smbd 4
445/tcp open netbios-ssn Samba smbd 4
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/
Nmap done: 1 IP address (1 host up) scanned in 11.72 seconds
[Alvaro]-[PC205]:~$


Una vez que Nmap descubre que un puerto está abierto en lugar de limitarse a mirar qué servicio suele usar ese puerto por defecto, establece una conexión real con el puerto y envía una serie de sondas definidas en su base de datos.
