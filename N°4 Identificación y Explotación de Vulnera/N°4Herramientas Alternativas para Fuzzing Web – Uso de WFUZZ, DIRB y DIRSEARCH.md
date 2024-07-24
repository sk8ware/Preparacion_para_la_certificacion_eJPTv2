
---

- TAG: #Fuzzing #Web #Wfuzz #Dirb #Dirsearch
----
# Alternativas a Gobuster para Fuzzing Web

En esta lección, vamos a explorar alternativas a **Gobuster** para realizar **Fuzzing Web**. Estas herramientas nos permiten encontrar directorios en una página web, lo cual es crucial en las pruebas de penetración. A veces, es posible que **Gobuster** no esté instalado en la máquina o presente errores, por lo que es útil conocer otras opciones.

Para esta práctica, utilizaremos la máquina **Jenk-Vulnyx** de Vulnyx. Aquí están los pasos:

## Preparativos

1. **Descargar y Configurar la Máquina:**
   - Descargamos la máquina [Jenk-Vulnyx](https://vulnyx.com/#jenk).
   - Extraemos el fichero y abrimos la máquina en VirtualBox.
   - Configuramos la red en **Adaptador Puente**.
   - Iniciamos la máquina.

2. **Localizar la Máquina:**
   - Desde nuestra máquina **Kali Linux**, usamos **arp-scan** para encontrar la IP de la máquina Jenk-Vulnyx:
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
   - Nos dirigimos al navegador e ingresamos la dirección IP para ver qué muestra.

## Alternativas a Gobuster

### Wfuzz

**Wfuzz** es una herramienta excelente para el fuzzing web, permitiendo seleccionar directorios de acuerdo a nuestras necesidades.

1. **Uso de Wfuzz:**
```bash
wfuzz -c --hc 404 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt http://1.2.3.4/FUZZ
```
   - Este comando realizará fuzzing de manera similar a **Gobuster**, encontrando dominios al final de la **URL**.
   - Encontramos una ruta interesante llamada **webcams**.

### Dirb

**Dirb** es otra herramienta útil para el fuzzing web. Aunque utiliza un directorio más pequeño, puede ser efectiva en ciertos casos.

1. **Uso de Dirb:**
```bash
dirb http://1.2.3.4/
```
   - Aunque muestra menos resultados, es útil tenerla como alternativa.

### Dirsearch

**Dirsearch** es una herramienta automatizada que no requiere la especificación de un directorio. Busca extensiones, métodos HTTP, hilos, etc.

1. **Uso de Dirsearch:**
```bash
dirsearch -u http://1.2.3.4/
```
   - Inicialmente, no muestra el dominio **webcams** debido al uso de un diccionario predeterminado.
   - Usamos el parámetro `-w` para especificar un diccionario:
```bash
dirsearch -u http://1.2.3.4/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```
   - Esto muestra el directorio **webcams** utilizando un diccionario más extenso.

## Resumen

Hemos explorado varias alternativas a **Gobuster** para realizar fuzzing web:

- **Wfuzz** para encontrar directorios.
- **Dirb** como opción secundaria con un directorio más pequeño.
- **Dirsearch** para una búsqueda automatizada y detallada.

Es importante recordar que hay una diferencia significativa entre buscar directorios web y buscar subdominios. Hemos estado enfocados en la búsqueda de directorios web en esta lección. Conocer estas alternativas es crucial para estar preparados en el examen **eJPT** y en cualquier prueba de penetración.

¡Sigan practicando y explorando estas herramientas!