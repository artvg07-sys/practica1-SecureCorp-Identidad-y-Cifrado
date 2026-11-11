# Respuestas · Práctica 2 · Bodegas Atlántico

> Rellena este fichero en tu ordenador. Va en la entrega (`./lab.sh entregar`) y en la defensa te pueden preguntar por él.

## Bloque A · Piensa como un atacante (nmap)

Desde la calle: `./lab.sh cliente` y dentro `nmap -Pn -p 22,80,443,5432,8080 198.51.100.254`. Pega la salida:

```text
(pega aquí la salida de nmap)
```
arturo@arturo-BOHK-WAX9X:~/practica1-SecureCorp-Identidad-y-Cifrado/practica2-Bodegas-Atlantico/kit-sad-p2$ ./lab.sh cliente
root@cliente:/# nmap -Pn -p 22,80,443,5432,8080 198.51.100.254
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-06 15:49 UTC
Nmap scan report for sad-p2-fw.sad-p2_calle (198.51.100.254)
Host is up (0.000065s latency).

PORT     STATE    SERVICE
22/tcp   filtered ssh
80/tcp   open     http
443/tcp  open     https
5432/tcp filtered postgresql
8080/tcp filtered http-proxy
MAC Address: CE:2D:FA:37:F4:87 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.31 seconds


## Bloque A · El contador

Después de `./lab.sh comprobar`, mira `./lab.sh ver`. ¿Cuántos paquetes lleva el contador de A6? ¿Quién los ha mandado?

>

## Bloque B · La prueba del cifrado

En el firewall (`./lab.sh fw`): `tcpdump -ni wan host 198.51.100.62 -c 6`. Mientras, en otra terminal, el comercial
(`./lab.sh comercial`) hace `curl http://172.16.10.20:8080`. Pega lo que sale en el firewall:

```text
(pega aquí la salida de tcpdump)
```
B8 · Comprobación del tráfico VPN con tcpdump

Captura realizada en la interfaz WAN del firewall:

19:44:17.118549 IP 198.51.100.61.47494 > 198.51.100.254.51820: UDP, length 32
19:44:17.118569 IP 198.51.100.62.53360 > 198.51.100.254.51820: UDP, length 32
19:44:17.118704 IP 198.51.100.61.47494 > 198.51.100.254.51820: UDP, length 148
19:44:17.118715 IP 198.51.100.62.53360 > 198.51.100.254.51820: UDP, length 148
19:44:17.118955 IP 198.51.100.254.51820 > 198.51.100.61.47494: UDP, length 92
19:44:17.119188 IP 198.51.100.254.51820 > 198.51.100.62.53360: UDP, length 92

16 packets captured
16 packets received by filter
0 packets dropped by kernel

Prueba de conectividad desde el comercial:

root@comercial:/# curl http://172.16.10.20:8080

ERP de la bodega (puerto 8080) · te llega una peticion desde 10.66.0.3



En dos líneas: ¿qué se ve por la calle y qué NO se ve?

>No se ve el contenido de las peticiones HTTP ni las IP privadas internas, porque la comunicación viaja cifrada dentro del túnel VPN WireGuard.

## Bloque C · Para la defensa (C4)

Del **mismo ataque** (la inyección a la web): ¿qué ve el vigilante de la red (el IDS) y qué ve el portero de la web? ¿Por
qué hacen falta los dos?

>

## Bloque D · La nota del servidor (D0 y D4)

Nota del análisis **antes** de bastionar (D0):

>

Nota **después** (D4). ¿Por qué no llega a 100? ¿Significa que el servidor sigue inseguro? Comenta un aviso que siga saliendo.

>
