
---
- TAG: #eJPT #Preparación #Examen 
----
# Configuración de la Máquina Atacante con Kali Linux

Una vez instaladas las máquinas vulnerables, es momento de configurar nuestra máquina atacante. Usaremos Kali Linux, aunque cualquier otra distribución de pentesting (como Parrot OS) también es válida.

## Instalación de Kali Linux

Vamos a instalar Kali Linux en formato de VirtualBox. Las credenciales de acceso por defecto serán `kali:kali`.

### Paso 1: Descarga de Kali Linux

Primero, descargamos la máquina virtual desde la página oficial de Kali Linux: [Kali Linux Downloads](https://www.kali.org/).

### Paso 2: Descompresión del Archivo

Una vez descargado, descomprimimos el archivo. Dentro de la carpeta encontraremos dos archivos, uno de color rojo y otro celeste.

### Paso 3: Importación a VirtualBox

Abrimos VirtualBox, le damos a "Añadir" y seleccionamos el archivo celeste dentro de la carpeta descomprimida. Esto creará la máquina virtual en VirtualBox.

### Paso 4: Configuración del Adaptador de Red

Antes de iniciar Kali, configuramos el adaptador de red a **Adaptador Puente**. Esto permite que la máquina se comunique dentro de nuestra red privada, esencial para futuros ataques y pruebas.

### Paso 5: Inicio y Ajustes Iniciales

Arrancamos nuestra máquina Kali y realizamos los siguientes ajustes:

- **Pantalla**: En la barra de configuración, seleccionamos "Ver en Pantalla Completa", luego expandimos, minimizamos y volvemos a abrir para ajustar la pantalla correctamente.
- **Teclado**: Cambiamos el teclado de inglés a español. Vamos a Configuración de Kali, seleccionamos "Teclado", desactivamos "Usar Configuración del Sistema", agregamos el idioma español y eliminamos el inglés.

### Paso 6: Actualización del Sistema

Actualizamos los paquetes y repositorios del sistema:


```zsh
sudo apt update sudo apt upgrade
```

Es importante realizar esta práctica regularmente para evitar problemas futuros.

### Paso 7: Limpieza de Dependencias Huérfanas

Ejecutamos el siguiente comando para eliminar dependencias huérfanas:

```zsh
sudo apt autoremove
```

Para mayor seguridad, realizamos una actualización completa del sistema:

```zsh
sudo apt dist-upgrade
```

Y luego nuevamente limpiamos dependencias:

```zsh
sudo apt autoremove
```

### Paso 8: Reinicio Final

Finalmente, reiniciamos la máquina para aplicar todos los cambios:

```zsh
reboot
```

¡Con esto, nuestra máquina Kali está lista para comenzar con el hacking ético! ¡Vamos a ello, mi gente!