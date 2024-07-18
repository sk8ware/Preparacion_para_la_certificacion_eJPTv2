
----
- TAG: #Fuerza-Bruta #Ataques #Usuarios #Hydra #SSH #FTP 
---
# Uso de Hydra para Descubrir Usuarios Desconocidos

En esta ocasión vamos a ver cómo descubrir nuestro usuario cuando tenemos la contraseña pero desconocemos el nombre del usuario. Aprenderemos a utilizar **Hydra** de otra manera. Es importante tener en cuenta que el diccionario **rockyou.txt** solo contiene contraseñas, pero no usuarios. Les indicaré cuál sería la opción a utilizar en el examen de la **eJPT**.

## Preparación del Diccionario de Usuarios

Primero, deben asegurarse de tener el diccionario **/usr/share/wordlists/metasploit/unix_users.txt**:

```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ssh://1.2.3.4
```

Si no tienen instaladas todas las librerías de **Wordlists**, las pueden obtener descargándolas de la siguiente manera:

```bash
sudo apt install wordlists
```

Con esto, tendrán las librerías de Metasploit, Seclists, Wfuzz, nmap.lst, etc.

## Ruta Alternativa de Diccionarios

Si no encuentran resultados con esta ruta `/usr/share/wordlists/metasploit/unix_users.txt`, debemos seguir intentando con las demás rutas hasta que las encontremos. Por lo general, todos los usuarios se encontrarán dentro de la lista. 

Para resumir y explicar mejor, agregaremos el usuario **juan** en la lista con **nano** o **nvim**:

```bash
sudo nano /usr/share/wordlists/metasploit/unix_users.txt
```

Luego, al volver a ejecutar el comando, aparecerá que el usuario juan es el correcto:

```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ssh://1.2.3.4
```

## Ataque de Fuerza Bruta con FTP

Si desean hacerlo con el protocolo **FTP**, lo pueden realizar de la misma manera, pero cambiando el protocolo:

```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ftp://1.2.3.4
```

---

## Recomendaciones Finales

Para finalizar, les recomiendo mucho navegar por los diccionarios mencionados, ya que contienen archivos que pueden llegar a ser muy útiles para encontrar usuarios y contraseñas, tanto en las carpetas de Metasploit como en el archivo Rockyou.txt para el examen **eJPT**.

---

### Resumen de Comandos Utilizados

1. **Instalación de Wordlists**:

```bash
sudo apt install wordlists
```

2. **Uso de Hydra para Descubrir Usuarios**:

```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ssh://1.2.3.4
```

3. **Agregar Usuario en la Lista**:

```bash
sudo nano /usr/share/wordlists/metasploit/unix_users.txt
```

4. **Uso de Hydra para FTP**:

```bash
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ftp://1.2.3.4
```

Con estos conocimientos y prácticas, estarán mejor preparados para el examen **eJPT**. ¡Buena suerte!
