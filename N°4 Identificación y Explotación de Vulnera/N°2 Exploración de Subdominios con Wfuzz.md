
---
- TAG: #Exploración #Subdominios #Wfuzz 
----
# Descubriendo Virtual Hosting y Subdominios con Wfuzz

En esta clase, aprenderemos sobre **Virtual Hosting** y cómo encontrar subdominios utilizando **Wfuzz**. Para esta práctica, utilizaremos la máquina **Logan** de HackMyVM.

## Preparativos

1. **Configurar las Máquinas:**
   - Descargamos y extraemos la máquina **Logan** de HackMyVM.
   - Abrimos la máquina en VirtualBox, configurando la red en **Adaptador Puente**.

2. **Localizar la Máquina:**
   - Desde nuestra máquina **Kali Linux**, usamos **arp-scan** para encontrar la IP de la máquina Logan:
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
nmap -p- -sS -sC -sV --open --min-rate=5000 -vvv -n -Pn 1.2.3.4 -oN escaneo
```

## Resolviendo el Problema de Virtual Hosting

1. **Identificación del Problema:**
   - Observamos que el puerto **80** está abierto, pero no podemos acceder a la página web.
   - Revisamos el archivo de escaneo y encontramos información sobre el host:
```bash
Service Info: Host: logan.hmv
```
   - Esto indica un caso de **Virtual Hosting**.

2. **Solución del Problema:**
   - Editamos el archivo `/etc/hosts` para asociar la IP con el host `logan.hmv`:
```bash
sudo nano /etc/hosts
```
   - Añadimos la línea:
```bash
1.2.3.4 logan.hmv
```
   - Guardamos el archivo y recargamos la página en el navegador. Ahora debería funcionar correctamente.

## Descubriendo Subdominios con Wfuzz

1. **Preparativos para Fuzzing de Subdominios:**
   - Utilizamos **Wfuzz** para encontrar subdominios. Si no lo tenemos instalado, lo hacemos con:
```bash
sudo apt install wfuzz
```

2. **Ejecutando Wfuzz:**
   - Ejecutamos **Wfuzz** para buscar subdominios:
```bash
wfuzz -c --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.logan.hmv" -u http://1.2.3.4
```

3. **Filtrando Resultados:**
   - Si obtenemos muchos resultados irrelevantes, filtramos por líneas con `--hl`:
```bash
wfuzz -c --hc=404 --hl=1 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.logan.hmv" -u http://1.2.3.4
```

4. **Encontrando Subdominios Relevantes:**
   - Encontramos un subdominio interesante llamado `admin`.
   - Intentamos acceder a `http://admin.logan.hmv` y obtenemos un error similar al problema de **Virtual Hosting**.

5. **Resolviendo el Problema de Subdominio:**
   - Editamos nuevamente el archivo `/etc/hosts` para añadir el subdominio:
```bash
sudo nano /etc/hosts
```
   - Añadimos la línea:
```bash
1.2.3.4 logan.hmv admin.logan.hmv
```
   - Guardamos el archivo y recargamos la página en el navegador. Ahora debería cargar correctamente.

## Ejemplo Completo

1. **Instalación de Herramientas:**
```bash
sudo apt install wfuzz dirbuster
```

2. **Escaneo de Red y Puertos:**
```bash
arp-scan -I wlan0 --localnet
ping 1.2.3.4
nmap -p- -sS -sC -sV --open --min-rate=5000 -vvv -n -Pn 1.2.3.4 -oN escaneo
```

3. **Solución de Virtual Hosting:**
```bash
sudo nano /etc/hosts
# Añadir línea:
1.2.3.4 logan.hmv
```

4. **Fuzzing de Subdominios con Wfuzz:**
```bash
wfuzz -c --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.logan.hmv" -u http://1.2.3.4
wfuzz -c --hc=404 --hl=1 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.logan.hmv" -u http://1.2.3.4
```

5. **Solución de Subdominio:**
   ```bash
   sudo nano /etc/hosts
   # Añadir línea:
   1.2.3.4 logan.hmv admin.logan.hmv
   ```

Siguiendo estos pasos, podrás solucionar problemas de **Virtual Hosting** y encontrar subdominios en una página web, mejorando tus habilidades de pentesting y preparándote para el examen **eJPT**. ¡Buena suerte!