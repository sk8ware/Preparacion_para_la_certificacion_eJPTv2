
---
- TAG: #eJPT #Preparación #Examen 
----
## Buenas Prácticas en Linux

### Uso de Usuarios y Privilegios

Para trabajar de manera segura en tu sistema Linux, es esencial seguir ciertas buenas prácticas, especialmente al manejar usuarios y permisos. Aquí te explicamos cómo hacerlo:

- **Usa siempre un usuario sin privilegios por defecto**: En nuestro caso, utilizaremos el usuario `kali@kali`. Este usuario tiene los permisos necesarios para la mayoría de las tareas diarias sin comprometer la seguridad del sistema.
    
- **Elevación de Privilegios con `sudo`**: Cuando necesites ejecutar un comando que requiere permisos elevados, simplemente añade `sudo` al inicio del comando. Por ejemplo:

```zsh    
sudo apt-get update
```
    
- Si te encuentras con un error de "Permiso Denegado", probablemente necesites usar `sudo`.
    
- **Cambio a Usuario Root**: Puedes convertirte en el usuario root para ejecutar comandos sin tener que añadir `sudo` cada vez. Hazlo con cuidado, ya que como root, tienes el poder de modificar y borrar cualquier cosa en el sistema.

```bash
sudo su
```
    
**Advertencia**: Estar logueado como root puede ser peligroso. Un comando como `rm *` ejecutado en la carpeta raíz podría borrar todo el sistema. Al usar un usuario sin privilegios como `kali`, se te pedirá confirmación para acciones críticas, lo cual añade una capa de seguridad.


### Instalación de Paquetes y Librerías

En Kali Linux, la instalación de paquetes puede hacerse de dos maneras: globalmente o localmente. La elección afecta la disponibilidad de los paquetes para diferentes usuarios.

1. **Instalación Global (Sistema Completo)**:
	    
    - Usa comandos como `apt-get` o `apt` para instalar paquetes a nivel del sistema. Esto hace que los paquetes estén disponibles para todos los usuarios, incluido root.
		
   ```
   sudo apt-get install nombre_del_paquete`
    ```    
    
2. **Instalación Local (Usuario Específico)**:
	    
    - Para instalar librerías solo para tu usuario, puedes usar gestores de paquetes específicos del lenguaje, como `pip` para Python. Esto instalará la librería en el entorno actual de Python, que puede ser local para tu usuario o global.
        
```zsh
pip install nombre_de_la_librería
```


### Preguntas Comunes

- **¿La descarga se realiza solo para el usuario actual?**:
    
    - Si usas un gestor de paquetes del sistema como `apt`, la instalación es global y estará disponible para todos los usuarios. Si usas un gestor de paquetes específico de un lenguaje y lo instalas de manera local, solo estará disponible para el usuario actual.
	
- **¿Se crea una copia automáticamente para el usuario root?**:
    
    - No. La instalación global hace que el paquete esté disponible para todos, pero no se duplicará. Si la instalación es local, no estará disponible para root a menos que también se instale específicamente para él.
	
- **¿Tengo que descargar la misma librería o paquete como root?**:
    
    - Solo si la instalación es específica para el usuario. Si es una instalación global, no necesitarás descargarla nuevamente para root.