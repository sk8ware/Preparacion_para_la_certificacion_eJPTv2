
---
- TAG:  #Introducción #hacking #Escaneo #Nmap
---
## Escaneos de Red con Nmap

Vamos a empezar con uno de los temas favoritos de muchos: realizar escaneos de red con la herramienta **Nmap**. Esta poderosa herramienta nos permite obtener información detallada sobre nuestra máquina objetivo, ayudándonos a identificar posibles vulnerabilidades.

### Realización de un Reconocimiento de Puertos

Cuando queremos realizar una acción en una máquina, es crucial conocer los puertos abiertos de dicha máquina, ya que por ahí pueden existir vulnerabilidades que podamos explotar.

#### Paso 1: Abrir la Terminal en Kali Linux

Primero, abrimos nuestra terminal en Kali Linux.

#### Paso 2: Comando Básico de Nmap

Para realizar un escaneo básico, usamos el siguiente comando:

```bash
nmap 1.2.3.4
```

#### Paso 3: Explicación de Parámetros de Nmap

Vamos a detallar algunos parámetros útiles de Nmap para mejorar nuestros escaneos:

- **`-p-`**: Escanea todos los puertos rápidamente y muestra los puertos abiertos.
- **`--open`**: Muestra únicamente los puertos que están abiertos.
- **`-sS`**: Realiza un escaneo SYN rápido.
- **`-sC`**: Utiliza un conjunto de scripts para realizar el escaneo, proporcionando información adicional.
- **`-sV`**: Detecta las versiones de los servicios que están corriendo en los puertos abiertos.
- **`--min-rate`**: Establece la velocidad de envío de paquetes por segundo. Para el eJPT, se recomienda usar `2000` paquetes por segundo en lugar de `5000`, que puede ser muy agresivo.
- **`-n`**: Desactiva la resolución DNS, lo cual puede ser innecesario en algunos casos.
- **`-vvv`**: Triple verbose, permite ver en pantalla todo lo que Nmap va encontrando.
- **`-Pn`**: Desactiva el ping, útil si el firewall de la máquina víctima está bloqueando el tráfico ICMP.
- **`-oN`**: Guarda el resultado del escaneo en un archivo especificado.

#### Comando Completo

Un ejemplo de comando completo utilizando varios de estos parámetros sería:


```bash
nmap -p- --open -sS -sC -sV --min-rate 2000 -n -vvv -Pn 1.2.3.4 -oN escaneo
```

### Interpretación del Resultado

Con el output de este comando, obtendremos mucha información detallada sobre la máquina objetivo. Por ejemplo, si estamos escaneando una máquina Windows 7, podríamos identificar que está corriendo una versión desactualizada del Service Pack 1 de Microsoft-ds, lo cual puede ser una entrada para explotar vulnerabilidades.

### Próximos Pasos

En las siguientes clases, aprenderemos a ejecutar scripts con Nmap que nos indiquen si una máquina es vulnerable a las principales vulnerabilidades conocidas.