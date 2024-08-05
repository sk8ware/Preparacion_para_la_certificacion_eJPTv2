
----
- TAG: #eJPT #Preparación #Examen 
----
Antes de comenzar con las prácticas de hacking, es fundamental tener preparados nuestros laboratorios, tanto el de la máquina víctima como el de la máquina atacante. Aquí hay tres conceptos clave que debes entender y utilizar correctamente:

1. **Adaptador Puente en VirtualBox**
2. **NAT**
3. **Red Interna para Pivoting**

### Adaptador Puente

El adaptador puente es esencial en pruebas de penetración porque permite que herramientas como MetaSploitable sean visibles para otras máquinas en la red. Con esta configuración, las máquinas virtuales comparten la misma red que la máquina anfitriona y obtienen direcciones IP similares, diferenciándose solo en el último número.

![[Pasted image 20240628101226.png]]

### NAT (Network Address Translation)

NAT, o Traducción de Dirección de Red, permite que varias máquinas virtuales compartan la misma dirección IP pública mientras mantienen direcciones IP únicas internamente. Cuando una máquina virtual envía una petición a través de la red, la dirección IP visible es la de la máquina anfitriona. Por ejemplo, si tu máquina anfitriona es Windows y tu máquina virtual es Ubuntu, la IP registrada en un servidor, como el de Google, será la de tu máquina Windows.

Esto añade una capa de seguridad útil, ya que oculta las direcciones IP internas de las máquinas virtuales.

![[Pasted image 20240628101725.png]]

### Red Interna

En VirtualBox y VMware, la opción de red interna (también conocida como RED NAT) permite crear una red aislada dentro del entorno de virtualización. Las máquinas en esta red no tienen acceso a Internet y sólo pueden comunicarse entre sí.

En este repositorio, configuraremos redes internas en VirtualBox para escenarios de pivoting. Por ejemplo, podríamos tener una máquina atacante con Kali Linux utilizando un adaptador puente para la primera interfaz de red y una segunda interfaz en una red interna con una dirección IP diferente. La máquina víctima, por otro lado, tendría una única interfaz en la red interna, asegurando que sólo sea accesible desde la máquina atacante y no desde ninguna otra máquina en tu red.

Esta configuración es crucial cuando queremos que la máquina víctima esté completamente aislada del resto de la red, garantizando que solo la máquina atacante tenga visibilidad y acceso.


![[Pasted image 20240628102953.png]]