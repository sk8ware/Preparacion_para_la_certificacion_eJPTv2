
----
- TAG: #eJPT #Preparación #Examen
----

# Preparación para el eJPT

## Temas a Tratar

1. **Preparación de Laboratorios**
2. **Herramientas Básicas**
3. **Explotación Básica de Vulnerabilidades**
4. **Fuzzing Web y Ataques de Fuerza Bruta**
5. **Entendimiento de Protocolos de Red**
6. **Uso de Metasploit y msfvenom**
7. **Explotación del Protocolo SMB en Windows**
8. **Intrusión y Escalada de Privilegios**
9. **Pivoting en Entornos Windows y Linux**

## Estructura del Examen

El examen eJPT sigue la siguiente estructura:

- **Máquina Atacante**: Proporcionarán una máquina atacante sin conexión a internet, trabajando únicamente en red local.
- **Reconocimiento y Enumeración**: Encontrarás varias máquinas en la red, la mayoría Linux y algunas Windows. Las máquinas Windows suelen alojar otras máquinas virtuales.
- **Pivoting**: Puede ser necesario realizar pivoting dentro de máquinas Linux alojadas en máquinas Windows. Es crucial escanear y enumerar todas las máquinas y servicios disponibles. Cada bit de información es valiosa.

### Ejemplo de Escenario en el Examen

Es posible que te pregunten algo como: "¿Cuál es el servicio del puerto 80 de la máquina Linux en la red interna con IP 10.10.10.6?"

1. **Pivoting**: En este caso, puedes aplicar pivoting.
2. **Enumeración con Metasploit**: Usa Metasploit para enumerar y aplicar port forwarding para identificar el servicio corriendo en el puerto 80.

### Notas Adicionales

- **Diversidad de Máquinas**: No solo te encontrarás con máquinas Linux, también puede haber varias máquinas Windows.
- **Aprendizaje y Solución de Problemas**: Aprenderemos a resolver estos desafíos de diversas maneras, como nos muestra el pingüino de Mario.

![[Pasted image 20240625231744.png]]

También es muy probable que no sea solo en la maquina linux, sino en alguna maquina Linux como nos muestra el pinguino de mario.

![[Pasted image 20240625231913.png]]

Pero aprenderemos a solucionarlo de ambas maneras.