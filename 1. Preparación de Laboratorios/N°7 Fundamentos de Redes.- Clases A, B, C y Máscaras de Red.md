
---
- TAG: #eJPT #Preparación #Examen 
---
## Entendiendo la Máscara de Red y Tipos de Redes

En este repositorio, aprenderemos qué es una máscara de red, los diferentes tipos de redes según su tamaño y cómo identificar cada tipo de red.

### Tipos de Redes

Existen tres tipos principales de redes según el tamaño:

1. **Red Tipo C**
2. **Red Tipo B**
3. **Red Tipo A**

### Red Tipo C

Las redes tipo C son redes domésticas, que típicamente tienen direcciones IP como `192.158.2.28` con una máscara de red de `255.255.255.0`, representada como `/24`.

- **Máscara de Red**: La máscara `255.255.255.0` indica que los primeros tres octetos (`192.158.2`) representan la porción de red, y el último octeto (`.28`) representa la porción de host.
- **Hosts**: En una red tipo C, puedes tener hasta 254 dispositivos, ya que el último número de host (`.255`) se reserva para broadcast, permitiendo la comunicación con todos los dispositivos de la red.

En los exámenes y prácticas, este tipo de dirección IP es común, así que asegúrate de identificar bien la máscara de red para saber a qué tipo de red te enfrentas.

### Red Tipo B

Las redes tipo B son más grandes y se utilizan en empresas. Estas redes tienen direcciones IP donde los primeros dos octetos (`255.255`) representan la porción de red y los últimos dos octetos (`0.0`) representan los hosts.

- **Máscara de Red**: La máscara `255.255.0.0` indica que los primeros dos octetos son la porción de red y los últimos dos octetos son la porción de host.
- **Hosts**: En una red tipo B, puedes tener muchos más dispositivos que en una red tipo C. Estas redes típicamente terminan en `/16`, lo que permite alojar una gran cantidad de equipos.

### Red Tipo A

Las redes tipo A son las más grandes y se utilizan en grandes organizaciones o para infraestructura de red a gran escala. En estas redes, solo el primer octeto (`255`) representa la porción de red, y los últimos tres octetos (`0.0.0`) representan los hosts.

- **Máscara de Red**: La máscara `255.0.0.0` indica que solo el primer octeto es la porción de red.
- **Hosts**: Con esta configuración, puedes tener una cantidad enorme de dispositivos. Estas redes se representan con `/8`.