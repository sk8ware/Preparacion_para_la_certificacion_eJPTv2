
---
- TAG: #Introducción #Fuzzing #web #Gobuster
---
# Descubriendo Directorios Activos con Gobuster

En esta clase, aprenderemos a encontrar directorios activos en una página web utilizando la herramienta **Gobuster**. Para esta práctica, utilizaremos la máquina **MetaSploitable2**.

## Preparativos

1. **Configurar las Máquinas:**
   - Iniciamos nuestra máquina **MetaSploitable2**.
   - En nuestra máquina **Kali Linux**, realizamos un escaneo de la red para localizar los dispositivos conectados.

2. **Localizar la Máquina:**
   - Usamos **arp-scan** para encontrar la IP de la máquina MetaSploitable2:
```bash
arp-scan -I wlan0 --localnet
```
   - Comprobamos la conectividad con un **ping** y determinamos el sistema operativo observando el TTL:
```bash
ping 1.2.3.4
```

## Escaneo de Puertos y Fuzzing

1. **Escaneo con Nmap:**
   - Realizamos un escaneo con **nmap** para encontrar los puertos abiertos y posibles vulnerabilidades:
```bash
nmap -p- -sS -sC -sV --open --min-rate=5000 -v -n -Pn 1.2.3.4
```

2. **Revisión de la Página Web:**
   - Al encontrar el puerto **80** abierto, revisamos la página web en el navegador. 

3. **Fuzzing con Dirb:**
   - Si el puerto **80** está abierto, realizamos un fuzzing inicial con **dirb**:
```bash
dirb http://1.2.3.4/
```
   - Si no tienes **dirb** instalado, puedes hacerlo con:
```bash
sudo apt install dirb
```

4. **Fuzzing con Gobuster:**
   - **Gobuster** es una herramienta más potente y recomendada para este tipo de tareas.
   - Instalamos **gobuster** si no lo tenemos:
```bash
sudo apt install gobuster
```

5. **Ejecutando Gobuster:**
   - Usamos **gobuster** para encontrar directorios activos utilizando una lista de directorios común:
```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
   - Si no tienes la lista de directorios, puedes instalar **dirbuster**:
```bash
sudo apt install dirbuster
```

6. **Encontrando Archivos Específicos:**
   - Para buscar archivos específicos como `txt`, `py`, `php`, `sh`, agregamos el parámetro `-x`:
```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```

## Consejos y Recomendaciones

1. **Fuzzing en el Examen eJPT:**
   - Siempre realiza fuzzing cuando encuentres un puerto **80** abierto en una máquina víctima.

2. **Uso de Diferentes Herramientas:**
   - Es bueno utilizar tanto **dirb** como **gobuster** ya que pueden encontrar resultados diferentes.

3. **Exploración de Directorios:**
   - **dirb** puede encontrar directorios dentro de otros directorios, por lo que es útil para una exploración más profunda.

4. **Ruta de Wordlists:**
   - Usa rutas específicas de diccionarios en **/usr/share/wordlists/dirbuster/**, ya que cada ruta está destinada para diferentes acciones.

## Ejemplo de Uso Completo

1. **Instalación de Herramientas:**
```bash
sudo apt install dirb
sudo apt install gobuster
sudo apt install dirbuster
```

2. **Escaneo de Red y Puertos:**
```bash
arp-scan -I wlan0 --localnet
ping 1.2.3.4
nmap -p- -sS -sC -sV --open --min-rate=5000 -v -n -Pn 1.2.3.4
```

3. **Fuzzing con Dirb:**
```bash
dirb http://1.2.3.4/
```

4. **Fuzzing con Gobuster:**
```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php,sh
```

Siguiendo estos pasos, serás capaz de identificar directorios activos y archivos importantes en una página web, mejorando tus habilidades de pentesting y preparándote para el examen **eJPT**. ¡Buena suerte!
