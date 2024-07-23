
---
- TAG: #Introducción #Fuzzing #web #Gobuster
---
Esta maravillosa herramiente nos permite encontrar directorios activos sobre una página web, con la potente herremineta de **Gobuster**.

Para esta práctica vamos a estar utilizando nuestra máquina **MetaSploitable2**

Al momento que ingresamos a una página web, por lo general no sabemos que directorios contiene, como `upload, login, admin, etc`

Abrimos nuestra máquina **Metasploitable2** y nuestra máquina Linux

En **Kali** empezamos haciendo un tipico **arp-scan** para localizar los dispositivos conectados a nuestra red.

Localizamos a nuestra máquina, luego de ello le realizamos un **Ping** para ver si tenemos conexión a la máquina y ver que tipo de **ttl** es.

Vemos que nos enfrentamos a una máquina **Linux**

Realizamos el escaneo con **nmap** para poder encontrar las vulnerabilidades y vemos que tiene un monton de puertos abiertos.

Como vemos que tiene el puerto 80 abierto, podemos revisar en el navegador que tiene su pagina  web un poco sencilla pero a la cual le estaremos encontrando directorios con Fuzzing

Cuando estemos dentro del examen y nos encontremos alguna maquina victima con el puerto 80 abierto, lo primero que debemos hacer es un **Fuzzing** 

Para ello lo podemos realizar de la siguiente manera

```bash
dirb http://1.2.3.4/
```

Sino tienen instalados en su sitema lo pueden instalar con 

```bash
sudo apt install dirb
```

Esta es una opción buena pero tambien les recomiendo más utilizar la herramienta **gobuster**

```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlist/dirbuster/
```

En este ejemplo mostramos el directorio que deberiamos utilizar para el examen 
