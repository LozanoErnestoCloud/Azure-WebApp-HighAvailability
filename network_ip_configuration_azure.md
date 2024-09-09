# Asignación de Direcciones IP a una Red y Subred en Azure

La asignación de direcciones IP a una red y subred en Azure (o cualquier otro entorno de red) es crucial para garantizar que las máquinas virtuales y los servicios se puedan comunicar de manera eficiente. Aquí te explico paso a paso cómo hacerlo correctamente:

## 1. Direcciones IP y la CIDR Notation
En Azure, las redes virtuales y subredes se definen usando un rango de direcciones IP. Este rango se especifica utilizando **CIDR Notation (Classless Inter-Domain Routing)**, que tiene este formato:

<dirección IP>/<número de bits de la máscara de red>

Por ejemplo, `10.0.0.0/16` significa que el rango de IPs empieza en `10.0.0.0` y la máscara de red tiene 16 bits, lo que da un total de **65,536 direcciones IP** posibles (10.0.0.0 - 10.0.255.255).

El número después de la barra (`/16`) representa la cantidad de bits que se usan para la **red** y determina cuántas direcciones están disponibles para las **subredes** y las **máquinas**.

## 2. Creación de la Red Virtual (VNet)
Cuando creas una **Red Virtual (VNet)** en Azure, seleccionas el rango de IPs que va a cubrir esa red. Un ejemplo común sería:

- **VNet Range**: `10.0.0.0/16`

Este rango te da un bloque de 65,536 IPs que puedes dividir en subredes más pequeñas.

## 3. Creación de Subredes
Las **subredes** son divisiones más pequeñas de una red. Una vez que defines el rango para la VNet, tienes que asignar rangos más pequeños para las subredes dentro de ese rango. Por ejemplo, en una VNet con el rango `10.0.0.0/16`, puedes definir subredes como:

- **Subred 1**: `10.0.1.0/24` (Esto da 256 IPs, de `10.0.1.0` a `10.0.1.255`).
- **Subred 2**: `10.0.2.0/24` (También 256 IPs, de `10.0.2.0` a `10.0.2.255`).

### ¿Por qué el "/24"?
El `/24` significa que los primeros 24 bits de la dirección IP son fijos para la red (en este caso, `10.0.1`) y los últimos 8 bits se pueden usar para los hosts (máquinas virtuales, balanceadores de carga, etc.). Esto te da hasta **256 direcciones IP** para esa subred (`10.0.1.0` - `10.0.1.255`).

### Reservas de IP en una subred
Azure **reserva 5 direcciones IP** en cada subred por razones internas. Por ejemplo, en la subred `10.0.1.0/24`, las direcciones IP reservadas son:

- `10.0.1.0`: Dirección de la red.
- `10.0.1.1`: Dirección para el servicio de puerta de enlace predeterminada.
- `10.0.1.2-3`: Direcciones reservadas por Azure.
- `10.0.1.255`: Dirección de difusión (broadcast).

Esto significa que en lugar de tener 256 IPs disponibles, realmente solo tendrás **251 IPs disponibles** para tus máquinas y recursos.

## 4. Ejemplo Práctico:
Si tienes un rango de VNet como `10.0.0.0/16`, y quieres crear dos subredes dentro de esa VNet, podrías hacer lo siguiente:

- **VNet**: `10.0.0.0/16` (rango de la red principal)
  
  Luego, defines dos subredes:
  
  - **Subred 1 (para máquinas virtuales)**: `10.0.1.0/24`
  - **Subred 2 (para base de datos u otros servicios)**: `10.0.2.0/24`

Aquí estás dividiendo el rango `10.0.0.0/16` en bloques más pequeños de `/24`. Cada uno de estos bloques contiene 256 direcciones IP, pero como ya mencioné, solo tendrás disponibles **251 direcciones IP** en cada subred debido a las reservas de Azure.

## 5. Consejos para Elegir el Tamaño de la Subred:
- **Tamaño de la subred**: Dependerá de cuántos recursos esperas desplegar en esa subred. Si planeas tener muchas VMs o servicios en una subred, elige un bloque más grande (como `/24` o incluso `/22` si necesitas más IPs). Si solo necesitas unas pocas VMs, un bloque más pequeño como `/28` o `/29` puede ser suficiente.
  
- **Rangos separados**: Asegúrate de que las subredes no se solapen. Por ejemplo, si una subred es `10.0.1.0/24`, la siguiente debe comenzar después de `10.0.1.255`, como `10.0.2.0/24`.

## 6. Consideraciones para las IP Públicas:
Si necesitas que alguna máquina virtual sea accesible desde Internet, puedes asignarle una **IP Pública**. Las direcciones IP públicas son gestionadas por Azure y se pueden asignar a tus VMs o a otros recursos como balanceadores de carga.
