# Ejercicio Práctico: "Despliegue de una Aplicación Web en Alta Disponibilidad con Escalabilidad"

## Escenario:
Una pequeña empresa de tecnología necesita alojar una aplicación web simple que sea accesible globalmente. Quieren que la aplicación esté disponible en todo momento y escale automáticamente en función del tráfico de usuarios. También quieren asegurarse de que el sistema esté seguro y monitoreado. El objetivo es crear una infraestructura que soporte esta aplicación web con alta disponibilidad y seguridad.

## Pasos del Ejercicio:

1. **Configurar una Red Virtual (VNet):**
   - Crea una **Red Virtual (VNet)** con dos subredes: una subred para las máquinas virtuales y otra subred para un servicio de base de datos futuro.
   - Asegúrate de que las subredes tengan un rango de direcciones IP adecuadas para que las VMs puedan comunicarse internamente.

2. **Desplegar dos Máquinas Virtuales (VMs) Windows/Linux:**
   - Crea dos máquinas virtuales en la primera subred. Puedes usar **Windows Server** o **Ubuntu** (lo que prefieras).
   - Configura los **Grupos de Seguridad de Red (NSG)** para las VMs, permitiendo solo el tráfico HTTP/HTTPS y SSH/RDP desde tu IP (para acceder a la administración).

3. **Configurar un Load Balancer:**
   - Crea un **Azure Load Balancer** para distribuir el tráfico entre las dos VMs.
   - Asegúrate de crear un **Health Probe** para verificar el estado de las VMs y desviar el tráfico si una de las VMs está fuera de servicio.
   - Configura las **Reglas de Equilibrio de Carga** para distribuir el tráfico HTTP (puerto 80) y HTTPS (puerto 443).

4. **Configurar Escalabilidad Automática (Auto-scaling):**
   - Usa un **Azure Virtual Machine Scale Set (VMSS)** o configúralo manualmente en el portal para que las VMs escalen automáticamente en función de la CPU o las conexiones de red.
   - Establece umbrales como: si el uso de CPU supera el 75% durante 5 minutos, añade una nueva VM; si baja por debajo del 25%, reduce las VMs.

5. **Añadir un Application Gateway (Opcional pero recomendado):**
   - Crea un **Azure Application Gateway** para tener balanceo de carga de capa 7, lo que te permitirá usar **WAF (Web Application Firewall)** para proteger tu aplicación de ataques comunes como SQL Injection y XSS.
   - Configura la ruta de acceso para que todo el tráfico HTTP sea redirigido a HTTPS.

6. **Configuración de Monitoreo y Alertas:**
   - Habilita **Azure Monitor** y **Log Analytics** para las máquinas virtuales y el balanceador de carga.
   - Crea alertas para que se te notifique si el uso de CPU, memoria o discos excede ciertos límites.

7. **Configurar Backup y Recovery:**
   - Usa **Azure Backup** para crear copias de seguridad regulares de las VMs.
   - Configura una política de recuperación para restaurar las VMs en caso de que fallen.

8. **Configurar un DNS Personalizado:**
   - Usa **Azure DNS** para configurar un nombre de dominio personalizado para la aplicación.
   - Apunta el dominio al **Load Balancer** o al **Application Gateway**.

## Resultados Esperados:
- Tu aplicación debe estar disponible globalmente con alta disponibilidad y escalabilidad automática.
- El tráfico debe ser distribuido uniformemente entre las VMs.
- Debes tener un firewall configurado para proteger la aplicación.
- Monitoreo y alertas deben estar activos para detectar cualquier problema de rendimiento.

## Recomendaciones para probar:
- Prueba la alta disponibilidad desconectando una de las VMs y asegurándote de que la otra siga sirviendo el tráfico.
- Genera una carga simulada en la aplicación para verificar que el escalado automático funciona correctamente.
