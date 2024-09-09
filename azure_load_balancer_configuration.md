# Configurar un Load Balancer

El objetivo del **Azure Load Balancer** es distribuir el tráfico entrante de red entre varias máquinas virtuales, asegurando alta disponibilidad y escalabilidad. Vamos a ver los pasos:

## 1. Crear un Azure Load Balancer
- Inicia sesión en el **Azure Portal**.
- En la barra de búsqueda, busca **"Load Balancer"** y selecciona **"Create"**.
- Configura los siguientes parámetros:

   - **Subscription**: Elige tu suscripción de Azure.
   - **Resource Group**: Usa uno existente o crea uno nuevo.
   - **Nombre**: Asigna un nombre para el Load Balancer (por ejemplo, `myLoadBalancer`).
   - **Region**: Elige la misma región donde están tus VMs.
   - **SKU**: Elige **Standard** para mayor resiliencia y soporte a escalabilidad automática. También puedes usar **Basic** si es un entorno de prueba.
   - **Type**: Selecciona **Public** para hacer el balanceador accesible desde Internet (para una aplicación pública). Selecciona **Internal** si el tráfico será solo interno entre tus servicios.

- Haz clic en **Next: Frontend IP Configuration**.

## 2. Configurar la IP de Frontend
Esta es la IP pública a través de la cual se accede al Load Balancer.

- **Frontend IP**: Crea una nueva IP pública para el Load Balancer o selecciona una existente.
- **IP Address Version**: Usa **IPv4** (la más común).
- Dale un nombre a esta IP pública (ej. `myFrontendIP`).
  
- Haz clic en **Next: Backend Pools**.

## 3. Configurar Backend Pools (Grupo de VMs)
El **Backend Pool** define a qué recursos (máquinas virtuales) se distribuye el tráfico.

- Haz clic en **Add a Backend Pool**.
   - **Nombre**: Asigna un nombre al pool (ej. `myBackendPool`).
   - **Backend Pool Configuration**: Selecciona **IP Address** (esto es típico cuando trabajas con máquinas virtuales).
   - **Virtual Network**: Elige la red virtual donde se encuentran tus máquinas virtuales.
   - **Associated to**: Selecciona las máquinas virtuales que deseas incluir en el grupo de backend. Debes agregar al menos dos máquinas para probar la alta disponibilidad.

- Haz clic en **Add** para crear el backend pool.

## 4. Revisar y Crear el Load Balancer
Revisa todas las configuraciones que has hecho y asegúrate de que todo está correcto.

- Haz clic en **Create** para desplegar el Load Balancer.

## 5. Configurar Health Probes
Los **Health Probes** son importantes para monitorear el estado de las máquinas virtuales. Si una VM falla, el Load Balancer dejará de enviar tráfico a esa VM.

- Haz clic en **Add a Health Probe**.
   - **Nombre**: Ponle un nombre al probe (ej. `myHealthProbe`).
   - **Protocol**: Elige el protocolo que deseas monitorear (usualmente **TCP** o **HTTP**).
   - **Port**: Especifica el puerto que deseas monitorear (por ejemplo, **80** para HTTP o **443** para HTTPS).
   - **Intervalo de Sondeo**: Especifica cuántos segundos deben pasar entre cada sondeo (por ejemplo, **5 segundos**).
   - **Unhealthy Threshold**: El número de intentos fallidos antes de marcar una VM como inactiva (por ejemplo, **2 intentos fallidos**).

- Haz clic en **Add** para crear el health probe.

## 6. Configurar las Reglas de Balanceo de Carga (Load Balancing Rules)
Las reglas de balanceo definen cómo el Load Balancer distribuye el tráfico entre las VMs.

- Haz clic en **Add a Load Balancing Rule**.
   - **Nombre**: Asigna un nombre a la regla (ej. `httpRule` para HTTP).
   - **Frontend IP Address**: Selecciona la IP del frontend que configuraste previamente.
   - **Backend Pool**: Selecciona el backend pool que creaste en el paso anterior.
   - **Protocol**: Selecciona el protocolo (por ejemplo, **TCP**).
   - **Port**: Define el puerto de entrada (por ejemplo, **80** para HTTP).
   - **Backend Port**: Define el puerto que usará la VM (generalmente será el mismo, como **80** para HTTP).
   - **Session Persistence**: Selecciona **None** para balanceo de carga simple, o **Client IP** si deseas que las sesiones de un cliente permanezcan en la misma VM.
   - **Idle Timeout (minutes)**: Configura el tiempo de espera para desconexiones inactivas (usualmente **4 minutos** está bien para HTTP).
   - **Floating IP**: Déjalo en **Disabled** (a menos que estés usando un escenario de clúster, como SQL AlwaysOn).

- Haz clic en **Add** para crear la regla.

## Prueba del Load Balancer
Una vez que el Load Balancer esté configurado, puedes hacer pruebas para verificar que el tráfico se distribuye correctamente entre las máquinas virtuales:

1. **Conectar a las VMs**: Asegúrate de que cada VM tenga una aplicación web simple o un servicio que responda al tráfico HTTP. Puedes usar un servidor web básico como **NGINX**, **Apache** (en Linux) o **IIS** (en Windows) para hacer pruebas.
2. **Probar el acceso**: Navega a la **IP pública del Load Balancer**. El tráfico debería ser distribuido entre las VMs del backend.
3. **Prueba de alta disponibilidad**: Apaga una de las VMs y verifica si el Load Balancer redirige el tráfico automáticamente a las otras máquinas.

## Configuración Adicional: Escalabilidad
Una vez configurado el Load Balancer, puedes configurar la escalabilidad automática agregando más VMs al Backend Pool si el tráfico lo requiere. Esto se puede hacer mediante un **Virtual Machine Scale Set (VMSS)** para automatizar la creación de nuevas instancias en función del tráfico.
