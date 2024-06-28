
----
- TAG: #eJPT #Preparación #Examen 
----
Para este apartado vamos a realizar la instalación de nuestra futura maquina victima Windows 7, ya que hay que estaremos practicando algunas vulnerabilidades con esta maquina

A contunación les dejare el enlace de descarga de nuestra maquina victima:
https://drive.google.com/file/d/11oWEHwqu9AOKNwBU483abdGhnxJaZKT5/view?usp=sharing

Yo lo estaré instalando en **gnome-boxes**, si ustedes desean lo pueden realizar en diferentes virtualizadores con Vmware o VirtualBox.

Si no lo tiene instalado en Linux, es tan sencillo como hacer :

```zsh
sudo apt install gnome-boxes
```

Esperan que complete la instalación y lo podrán abrir.

Una vez Instalado simplemente procedemos a darle doble click en el archivo y continuar con la instalación.

Realizamos unas pequeñas configuraciones y fijarnos en el apartado de red este habilitado el **adaptador puente**.

Listo después de eso empezamos con la inicialización de nuestra maquina Windows dandole doble click.

Al iniciar el sistema operativo veremos que nos pedirá la contraseña la cual esta asignada con `123123` sencillito y pa dentro xd 

Una vez dentro de la maquina vamos a revisar si la configuración ip esta asignada correctamente 
Ingresamos al modulo de cmd y escribimos `ipconfig`

Si observamos nuestra dirección ip es por que se instalo correctamente

Ahora si desde nuestra consola de linux le enviamos un **ping** deberíamos tener respuesta 

```cmd
ping (tu_dirección_ip)
```

Listo con esto ya tendríamos nuestra maquina lista para las próximas clases estarla vulnerando de manera ética en un entorno controlado, siempre con fines educativos :)

qemu-img convert -O qcow2 windows7.vmdk windows7.qcow2
