
---
- TAG: #Introducción #hacking #Redes #ARP-SCAN #NETDISCOVER #Escaneo 
----
## Encontrar Equipos en la Red Privada

En este apartado, aprenderemos a encontrar equipos dentro de nuestra red privada, una habilidad crucial para aprobar el examen eJPT. Durante el examen, utilizaremos una máquina Kali Linux sin conocer las direcciones IP de las máquinas objetivo. Para identificarlas, emplearemos herramientas que nos permitan escanear las direcciones IP accesibles en nuestra red privada.

### Preparativos

Arrancamos nuestras máquinas **Kali Linux** y **Windows 7**. En el examen, tendremos una máquina Kali Linux similar a la que estamos utilizando, pero desconoceremos las máquinas objetivo. A continuación, aprenderemos a usar tres herramientas esenciales para escanear direcciones IP disponibles en nuestra red privada. Es importante tener un plan B y C por si alguna herramienta no funciona.

### ARP-SCAN

ARP-SCAN es una herramienta sencilla y fácil de usar. Primero, necesitamos conocer nuestra interfaz de red. Para ello, ejecutamos:

```bash
ifconfig
```

Este comando nos mostrará nuestra dirección IP y la interfaz (por ejemplo, `eth0`). Luego, para encontrar todos los equipos en la red, usamos:

```bash
arp-scan -I eth0 --localnet
```

Este comando listará todas las direcciones IP disponibles en nuestra red privada. Si aparecen muchas direcciones, intentemos escanear todas.

Para identificar el sistema operativo de las máquinas, usamos `ping` y observamos el TTL:

```bash
ping -c 1 1.2.3.4
```

- **TTL = 128** -> **Windows**
- **TTL = 64** -> **Linux**

### Netdiscover

Netdiscover es similar a ARP-SCAN y requiere conocer nuestra dirección IP e interfaz. Ejecutamos:

```bash
netdiscover -i eth0 -r 1.2.3.0/24
```

El rango `/24` permite escanear todas las direcciones en la subred. Esta herramienta nos mostrará todos los dispositivos conectados en nuestra red privada.

### Nmap

Nmap es una herramienta poderosa con múltiples funciones. Para escanear todas las direcciones IP en nuestra red privada, utilizamos:

```bash
nmap -sn 1.2.3.0/24
```

Es recomendable usar más de una herramienta para obtener resultados más precisos. Nmap también permite exportar los resultados a un archivo, lo cual es útil para organizar la información:

```bash
nmap -sn 1.2.3.0/24 -oN datos_de_ips.txt
```

### Conclusión

Aprender a encontrar máquinas dentro de nuestra red privada es el primer paso para vulnerarlas. Utilizando estas herramientas, podremos identificar las direcciones IP disponibles y preparar nuestros ataques de manera eficiente.