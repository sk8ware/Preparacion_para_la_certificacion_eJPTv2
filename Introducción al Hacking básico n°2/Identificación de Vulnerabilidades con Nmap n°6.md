

---
- TAG: #Introducción #hacking #Nmap #Vulnerabilidades 
----
## Encontrando Vulnerabilidades con Nmap

En este apartado, aprenderemos a encontrar vulnerabilidades utilizando **Nmap**. Esta herramienta es muy versátil y no solo sirve para escanear puertos; también nos permite identificar vulnerabilidades en los puertos abiertos.

### Configuración del Entorno

Para esta práctica, necesitaremos:

- **Kali Linux** como máquina atacante.
- **Windows** como máquina víctima (por tener desactualizaciones y vulnerabilidades).

Ambas máquinas deben estar encendidas y en la misma red para que puedan comunicarse.

### Paso 1: Encontrar Máquinas Vulnerables en la Red Local

El primer paso es identificar las máquinas vulnerables dentro de nuestra red local. Podemos comenzar lanzando un **arp-scan** desde nuestra máquina Kali Linux.

#### Uso de la Herramienta **arp-scan**

En Kali Linux, abrimos la terminal y ejecutamos:

```bash
arp-scan -I eth0 --localnet
```

El output nos mostrará las máquinas en la red junto con sus direcciones **MAC**.

#### Confirmar Información con **Ping**

Podemos confirmar la información obtenida haciendo un **ping** a la dirección IP para identificar el tipo de sistema operativo según el **TTL** (Time To Live) asignado:

```bash
ping -c 1 1.2.3.4
```

### Paso 2: Escanear Vulnerabilidades con Nmap

Ahora, utilizaremos **Nmap** con el parámetro `--script "vuln"` para ejecutar un script que nos indique si una máquina tiene vulnerabilidades en determinado puerto.

#### Ejecución del Escaneo de Vulnerabilidades

Para realizar un escaneo de vulnerabilidades en el puerto 445 de la máquina víctima, usamos:

```bash
nmap --script "vuln" -p445 1.2.3.4
```

Este escaneo nos mostrará si la máquina es vulnerable y nos proporcionará el **ID** de la vulnerabilidad pública, como **CVE**.

### Interpretación de Resultados

Después de realizar el escaneo, Nmap nos indicará si la máquina es vulnerable a ejecución remota y nos dará el **ID** de la vulnerabilidad. Con esta información, pasamos a la fase de **explotación**.

### Paso 3: Explotación con Metasploit

Para explotar la vulnerabilidad, podemos utilizar herramientas automatizadas como **Metasploit**. Aunque en el examen eJPT no tendremos conexión a internet, se nos permite utilizar herramientas automatizadas.

#### Inicio de Metasploit

Abrimos Metasploit con el siguiente comando:

```bash
msfconsole
```

Esto nos abrirá una consola interactiva con múltiples opciones.

#### Búsqueda de Exploits

Para encontrar el exploit adecuado para la vulnerabilidad (por ejemplo, **EternalBlue**), buscamos utilizando el CVE correspondiente:

```metasploit
search CVE-2017-0143
```

Esto nos mostrará el exploit necesario para realizar la prueba de penetración.

---

En próximas clases, aprenderemos a utilizar Metasploit en detalle para explotar vulnerabilidades y realizar pruebas de penetración. ¡Mantente atento y sigue practicando!