
---
- TAG: #Wireshark #Capturar #Credenciales #Protocolos #Vulnerabilidades 
----
# Manejo de Wireshark

En la clase de hoy, aprenderemos el manejo de **Wireshark**. Veremos algunos ejemplos prácticos de cómo utilizar esta herramienta desde **Kali Linux**.

Para esta práctica, utilizaremos:
- Nuestra máquina **Kali Linux**
- Máquina **Metasploit2** en (adaptador puente)

La herramienta **Wireshark** nos permite observar el tráfico de la red. Por ejemplo, si un equipo está utilizando un protocolo vulnerable como HTTP, FTP, SSH, etc., podemos observar la información que se está enviando por la red.

## Procedimiento

### 1. Preparación del Entorno

Nos dirigimos a nuestra máquina **Kali Linux**.

1. **Arp-scan** para localizar la máquina **Metasploit2**.
2. Usamos el protocolo **FTP** para conectarnos:
   ```bash
   ftp 1.2.3.4
   ```
   - **Usuario**: admin
   - **Contraseña**: msfadmin

### 2. Captura de Tráfico con Wireshark

3. Abrimos **Wireshark** para ponernos en escucha:
   - Entramos a la interfaz de nuestra PC, ya sea `eth0` o `wlan0`.
   - Vamos a la terminal y nos volvemos a loguear por el protocolo **FTP**.
   - Regresamos a **Wireshark** y filtramos por **FTP**. Veremos que muestra el usuario y la contraseña que hemos ingresado como `USER` y `PASS`.

### 3. Observación del Tráfico HTTP

En caso de ser un protocolo **HTTP**, sería de la misma manera:

1. Nos dirigimos al puerto `80` a través del navegador y su dirección IP.
2. Damos click en el enlace de **DVWA**.
3. Nos logueamos con las credenciales:
   - **Usuario**: admin
   - **Contraseña**: password

4. Regresamos a **Wireshark** y filtramos por el protocolo **HTTP**.
5. Buscamos la petición por **POST**.
6. Le damos click en **POST**.
7. Buscamos por `HTML Form URL Encoded: application/` y le damos click. Veremos que muestra el `username` y la `password`.

---

Es raro que en el examen les pregunten sobre esta herramienta, pero es posible, así que practiquen y curioseen un poco más sobre **Wireshark**.

hola