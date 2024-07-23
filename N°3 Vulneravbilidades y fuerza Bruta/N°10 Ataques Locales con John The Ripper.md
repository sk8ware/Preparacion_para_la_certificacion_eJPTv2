
---
- TAG: #Ataques #Locales #John_The_Ripper #Hash-identifier
---
Continuamos con los ataques de fuerza bruta, pero en esta ocasión lo vamos a realizar con la herramienta **John The Ripper** pero de local, Como archivos que vienen protegidos con contraseña, bases de datos como **Kypass** igualmente protegidas por contraseñas, Tambien estaremos aprendiendo un poco de **Cracking** a tipos de **Hashes**

Primero empezamos creando un archivo `.zip` desde nuestra máquina **Windows Anfitrión** y abrimos la aplicación de **Winrar** para poder comprir archivos. 

En blocs anteriores habiamos aprendido a compartir datos por el protocolo **http**, pero para primero debemos saber la dirección **IP** de nuestra máquina Windows.
- Abrimos el **cmd**
- Ejecutamos `ipconfig`
- Compartimos el archivo en local
```python
python -m http.server 80 
```

Ahora desde nuestra máquina **Kali Linux** Obtenemos el archivo con `wget`

```python
wget 1.2.3.4/Archivo.zip
```

Recuerden escribir siempre correcto el nombre del archivo si les aparecera un error 404

Una vez dentro del `Thunar` Abrimos el archivo y si lo tratamos de extraer nos pedira una contraseña, la cual ya la habiamos asignado 

Ahora vamos a crackear esta contraseña con **John The Ripper**, Esto debemos realizar atraves de la consola, Para ello le pasaremos la función `zip2john` seguido del archivo que queramos crackear el hash

Extraemos el **hash**

```bash
zip2john archivo.zip > hash
```

 