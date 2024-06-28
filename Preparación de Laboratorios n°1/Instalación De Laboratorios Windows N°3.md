
----
- TAG: #eJPT #Preparación #Examen 
----
## Instalación de Máquina Víctima Windows 7 para Prácticas de Ethical Hacking

En esta sección, aprenderemos a instalar y configurar una máquina víctima Windows 7, que será utilizada para practicar diversas vulnerabilidades en un entorno controlado.

### Descarga de la Máquina Víctima

Para comenzar, descargaremos la imagen de la máquina víctima desde el siguiente enlace: [Windows 7 Victim Machine](https://drive.google.com/file/d/11oWEHwqu9AOKNwBU483abdGhnxJaZKT5/view?usp=sharing).

### Configuración en VirtualBox (o Virtualizadores Similares)

Esta guía asume el uso de VirtualBox, pero también puedes utilizar otros virtualizadores como VMware o gnome-boxes.

1. **Instalación del Virtualizador**: Asegúrate de tener instalado VirtualBox u otro software de virtualización.
    
2. **Instalación de la Máquina Víctima**:
    
    - Descarga el archivo y ábrelo en VirtualBox.
    - Sigue los pasos de instalación estándar.
    - Durante la configuración, asegúrate de habilitar el adaptador de red en modo **puente** para conectividad adecuada.
3. **Configuración Post-Instalación**:
    
    - Una vez completada la instalación, inicia la máquina virtual.
4. **Acceso Inicial**: 
    
    - La contraseña inicial para la máquina víctima es `123123`.
5. **Verificación de Configuración de Red**:
    
    - Abre la consola de comandos (cmd) dentro de Windows e introduce el comando `ipconfig`.
    - Verifica que la dirección IP esté asignada correctamente.
6. **Prueba de Conectividad**:
    
    - Desde la consola de comandos, realiza un ping a otra máquina en tu red o a una dirección IP externa para confirmar la conectividad.


```cmd
ping tu_dirección_ip
```

### Uso Ético y Educativo

Con estos pasos completados, tendrás tu máquina víctima configurada y lista para practicar ethical hacking en un entorno controlado y seguro, siempre con fines educativos y éticos.

¡Disfruta aprendiendo y explorando nuevas técnicas de seguridad! 
