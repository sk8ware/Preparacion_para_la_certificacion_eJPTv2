
----
- TAG: #Introducción #hacking #UDP #Nmap 
---
## Escaneo de Puertos: TCP y UDP

En esta sección, vamos a profundizar en el escaneo de puertos, pero esta vez realizaremos un escaneo diferente. Anteriormente, utilizamos **Nmap** para encontrar puertos abiertos bajo el protocolo **TCP**, que es el más común en un proceso de hacking, especialmente para el examen eJPT. Sin embargo, también es importante conocer cómo realizar escaneos de puertos bajo el protocolo **UDP**.

### Preparación del Entorno

Para esta práctica, utilizaremos la máquina virtual **Chain** de [VulNyx](https://vulnyx.com/#chain). Aquí están los pasos para configurarla:

1. **Descarga la Máquina Virtual**:
    
    - Descarga el archivo desde el enlace de **MEGA** proporcionado.
    - Utiliza **WinRar** para extraer la máquina virtual.
    - Haz doble clic en el archivo extraído para abrirlo.
    - Configura el sistema de red como **Adaptador Puente** antes de iniciar la máquina, para que la máquina atacante pueda ver a la máquina víctima.
2. **Inicia las Máquinas**:
    
    - Inicia tu máquina atacante (Kali Linux) y la máquina víctima (Chain).

### Paso 1: Escaneo de la Red Local

Desde nuestra máquina atacante (Kali Linux), comenzamos con un escaneo **arp-scan** para identificar las máquinas en nuestra red local:

```bash
arp-scan -I eth0 --localnet
```

### Paso 2: Confirmar la Máquina Víctima

Realizamos un **ping** a la máquina víctima para confirmar que es una máquina Linux, observando el **TTL** de 64:

```bash
ping -c 1 1.2.3.4
```

### Paso 3: Escaneo de Puertos TCP y UDP

#### Escaneo de Puertos TCP

Primero, realizamos un escaneo de puertos TCP utilizando **Nmap** con los siguientes parámetros:

```bash
nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4
```

Estos parámetros nos ayudarán a identificar los puertos TCP abiertos en la máquina víctima.

#### Escaneo de Puertos UDP

Para encontrar puertos abiertos bajo el protocolo UDP, utilizamos los siguientes parámetros en **Nmap**:

```bash
nmap -sU --top-ports 200 --min-rate=500 -Pn 1.2.3.4
```

Este comando nos permitirá identificar todos los puertos UDP abiertos y cerrados.

### Observación de Resultados

En el output del escaneo, observaremos que el puerto **161/udp** está abierto, lo que indica que el servicio **SNMP** (Simple Network Management Protocol) está activo.

### Recomendaciones para el Examen eJPT

Es crucial realizar ambos tipos de escaneos, TCP y UDP, para asegurarnos de no dejar pasar ninguna vulnerabilidad. Esto es especialmente importante para el examen, ya que debemos estar preparados para identificar cualquier tipo de puerto abierto.

---

En próximas clases, aprenderemos cómo enumerar los servicios que corren en estos puertos, comenzando por **SNMP** en el puerto 161/udp. ¡Sigue practicando y nos vemos en la próxima lección!