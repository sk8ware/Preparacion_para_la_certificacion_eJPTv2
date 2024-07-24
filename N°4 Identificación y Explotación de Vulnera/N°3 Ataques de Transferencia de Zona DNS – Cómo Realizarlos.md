
----
- TAG: #Ataques #Transferencia #Zona #Dns #Introducción 
---
# Ataques de Transferencia de Zona DNS

En esta lección, vamos a explorar los **Ataques de Transferencia de Zona DNS**. Este tipo de ataque nos permite enumerar el protocolo **DNS** que está funcionando en una máquina. Normalmente, el protocolo **DNS** opera en el puerto 53, que es el puerto que nos interesa.

## Conceptos Básicos del Protocolo DNS

El protocolo DNS (Sistema de Nombres de Dominio) traduce nombres de dominio, como `google.es`, a direcciones IP. Este proceso es fundamental, ya que facilita la navegación en internet al permitirnos usar nombres fáciles de recordar en lugar de direcciones IP.

## Preparativos

Para esta práctica, utilizaremos la máquina **Hunter** de Vulnyx. Aquí están los pasos:

1. **Descargar y Configurar la Máquina:**
   - Descargamos la máquina [Hunter de Vulnyx](https://vulnyx.com/#hunter).
   - Extraemos el fichero y abrimos la máquina en VirtualBox.
   - Configuramos la red en **Adaptador Puente**.
   - Iniciamos la máquina.

2. **Localizar la Máquina:**
   - Desde nuestra máquina **Kali Linux**, usamos **arp-scan** para encontrar la IP de la máquina Hunter:
```bash
arp-scan -I wlan0 --localnet
```
   - Comprobamos la conectividad con un **ping**:
```bash
ping 1.2.3.4
```

3. **Escaneo de Puertos y Vulnerabilidades:**
   - Realizamos un escaneo con **nmap** para encontrar puertos abiertos y posibles vulnerabilidades:
```bash
nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
```
   - Observamos que el puerto **53** está abierto, indicando que el protocolo **DNS** está activo.

## Configuración del Archivo Hosts

1. **Editar el Archivo `/etc/hosts`:**
   - Añadimos el dominio encontrado durante el escaneo:
```bash
sudo nano /etc/hosts
```
   - Añadimos la línea:
```bash
1.2.3.4 hunterzone.nyx
```

## Realizando el Ataque de Transferencia de Zona DNS

El **Ataque de Transferencia de Zona DNS** nos permite obtener información detallada sobre los dominios y subdominios de una zona DNS específica.

1. **Ejecutar el Ataque con Dig:**
   - Utilizamos la herramienta **dig** para realizar el ataque:
```bash
dig axfr hunterzone.nyx @1.2.3.4
```
   - En la respuesta, obtendremos información detallada sobre directorios y subdominios.

2. **Registrar Subdominios Encontrados:**
   - Anotamos todos los subdominios encontrados para futuras pruebas. Por ejemplo:
```bash
 admin.hunterzone.nyx.
```

## Enumeración de Subdominios con Wfuzz

Ahora que tenemos nuevos dominios, vamos a enumerar esos subdominios utilizando **Wfuzz**.

1. **Enumerar Subdominios:**
   - Ejecutamos **Wfuzz** con una lista de subdominios específica:
     ```bash
     wfuzz -c --hc=404 --hl=367 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host: FUZZ.devhunter.nyx" -u http://1.2.3.4
     ```

2. **Filtrar Resultados:**
   - Filtramos los resultados para obtener información relevante. El parámetro `--hl=` nos ayuda a filtrar líneas específicas:
```bash
wfuzz -c --hc=404 --hl=1 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host: FUZZ.devhunter.nyx" -u http://1.2.3.4
```

3. **Añadir Subdominios al Archivo Hosts:**
   - Editamos nuevamente el archivo `/etc/hosts` para añadir subdominios encontrados:
```bash
 sudo nano /etc/hosts
```
   - Añadimos la línea:
```bash
1.2.3.4 hunterzone.nyx admin.hunterzone.nyx
```
   - Guardamos el archivo y recargamos la página en el navegador.

## Resumen

Siguiendo estos pasos, hemos aprendido a:

- Configurar y localizar una máquina vulnerable en nuestra red.
- Realizar un ataque de transferencia de zona DNS para enumerar información sobre el protocolo DNS.
- Utilizar **Wfuzz** para enumerar subdominios y filtrar resultados relevantes.

Estos conocimientos son cruciales para identificar y explotar vulnerabilidades en sistemas DNS, mejorando nuestras habilidades de pentesting y preparación para el examen **eJPT**. ¡Sigan practicando y explorando!


