# Azure-WebApp-HighAvailability
Implementación de una infraestructura en Azure para una aplicación web con alta disponibilidad, seguridad y monitoreo.

# Azure WebApp High Availability Project

## 📖 Descripción del Proyecto
Este proyecto implementa una infraestructura en **Microsoft Azure** para una aplicación web, garantizando alta disponibilidad, escalabilidad y seguridad mediante el uso de varios servicios de Azure. El objetivo es proporcionar una solución resiliente y eficiente para aplicaciones críticas.

## ⚙️ Servicios Utilizados
- **Azure Virtual Network (VNet)**: Estructura de red para conectar recursos en Azure.
- **Máquinas Virtuales (Windows Server 2016)**: Corren la aplicación web.
- **Azure Load Balancer**: Distribuye tráfico entre las VMs para asegurar alta disponibilidad.
- **Azure Application Gateway**: Administra el tráfico HTTP/S y protege contra amenazas con WAF.
- **Azure Monitor y Log Analytics**: Monitorización de recursos y generación de alertas.
- **Azure Backup**: Garantiza la protección de datos mediante copias de seguridad.

## 🛠️ Pasos de Implementación

### 1. Configuración de la Red Virtual (VNet)
- Se creó una **Red Virtual** con el rango de IP `10.0.0.0/16`, dividiendo en dos subredes:
  - **Subred 1**: `10.0.1.0/24` para las máquinas virtuales.
  - **Subred 2**: Reservada para bases de datos u otros servicios.

### 2. Despliegue de Máquinas Virtuales
- **VM1** y **VM2** fueron creadas en la subred de `10.0.1.0/24`, ambas corriendo **Windows Server 2016** y configuradas con IIS para manejar solicitudes HTTP.

### 3. Configuración del Load Balancer
- Un **Azure Load Balancer** fue implementado para distribuir el tráfico entrante en el puerto 80 entre las dos máquinas virtuales.
- Se configuraron **Health Probes** para verificar la salud de las VMs.

### 4. Configuración del Application Gateway
- Se implementó un **Application Gateway** que maneja el tráfico HTTP/S a nivel de capa 7.
- Se configuró un **Web Application Firewall (WAF)** para proteger la aplicación contra amenazas comunes como SQL injection y ataques XSS.

### 5. Monitoreo y Backup
- **Azure Monitor** fue habilitado para monitorear el rendimiento de las VMs.
- Se configuraron alertas para uso elevado de CPU.
- **Azure Backup** se utilizó para realizar copias de seguridad de las VMs.

## 🚀 Resultados
- **Alta Disponibilidad**: La aplicación sigue operativa incluso si una de las máquinas virtuales falla.
- **Escalabilidad**: La infraestructura está preparada para escalar en función del tráfico.
- **Seguridad**: Protección avanzada contra amenazas con el WAF del Application Gateway.

## 📊 Diagrama de Arquitectura
Incluye un diagrama de la infraestructura creada:

![Azure Architecture Diagram](ruta-a-la-imagen.png)

## 💡 Lecciones Aprendidas
Durante este proyecto, aprendí a:
- Configurar un entorno altamente disponible en Azure.
- Utilizar correctamente el **Load Balancer** y el **Application Gateway** en conjunto.
- Implementar **Azure Monitor** y recibir alertas en tiempo real.

## 🔗 Enlaces Relacionados
- [Documentación oficial de Azure](https://docs.microsoft.com/en-us/azure)
- [Guía de Load Balancer en Azure](https://docs.microsoft.com/en-us/azure/load-balancer)

## 📝 Licencia
Este proyecto está bajo la licencia MIT - [Mira la licencia](LICENSE).
