# Proyecto
📌 1. Arquitectura de Virtualización en Windows 11
🔹 Aislamiento de Núcleo y VBS

El Aislamiento de Núcleo (Core Isolation) y la Virtualization-Based Security (VBS) son funciones de seguridad de Windows 11 que utilizan virtualización para proteger procesos críticos del sistema operativo.

VBS crea un entorno virtual separado dentro del sistema.
Protege credenciales, integridad del kernel y evita ejecución de código malicioso.
Usa tecnologías como:
Hyper-V
HVCI (Hypervisor-Protected Code Integrity)
⚠️ Impacto en la virtualización
Puede generar conflictos con herramientas como VirtualBox o VMware.
Reduce el rendimiento de máquinas virtuales.
Impide el uso directo de VT-x/AMD-V por otros hipervisores.

👉 Recomendación:
Desactivar VBS si se requiere máximo rendimiento en GNS3.

🔹 Activación de VT-x / AMD-V

Estas tecnologías permiten virtualización por hardware:

Intel VT-x (procesadores Intel)
AMD-V (procesadores AMD)
🛠️ Procedimiento:
Reiniciar PC e ingresar a BIOS/UEFI.
Buscar:
Intel Virtualization Technology / SVM Mode
Activar la opción.
Guardar cambios.
✔️ Verificación en Windows:
Administrador de tareas → pestaña Rendimiento → CPU
Debe indicar: “Virtualización: Habilitada”

También se puede usar:

systeminfo

Buscar: Virtualization Enabled In Firmware: Yes
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/134f29a0-0e46-4f6c-864f-358364917889" />


📌 2. GNS3 VM: El Motor de Simulación
🔹 KVM (Kernel-based Virtual Machine)

KVM es un módulo del kernel de Linux que convierte el sistema en un hipervisor tipo 1.

📊 Importancia en GNS3:
Permite ejecutar dispositivos de red (routers, switches) con alto rendimiento.
Acelera la emulación usando virtualización real.
✔️ ¿Por qué debe estar en "True"?
Indica que la virtualización por hardware está disponible.
Si está en "False":
Emulación lenta
Alto consumo de CPU
Simulaciones poco realistas
🔹 Configuración de Recursos (CPU y RAM)

Asignar recursos correctamente es clave.

📌 Criterios:
No usar más del 50-70% de la CPU
Dejar mínimo 4 GB de RAM libres para Windows
🧠 Ejemplo:
Recurso	Recomendación
CPU	2–4 cores
RAM	4–8 GB
⚠️ Riesgos:
Demasiados recursos → Windows se vuelve lento
Muy pocos → GNS3 se congela
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/5f40423a-2193-4dd5-9874-fa247b35c934" />

📌 3. Integración con VirtualBox (Local)
🔹 Configuración de Red (Host-Only)

Permite comunicación entre:

PC (GUI GNS3)
GNS3 VM
🛠️ Pasos:
Ir a VirtualBox → File → Host Network Manager
Crear adaptador Host-Only
Configurar IP:
Ej: 192.168.56.1
Asignar este adaptador a la GNS3 VM
🔹 Modo Promiscuo

El modo promiscuo permite que una interfaz de red capture todo el tráfico, no solo el dirigido a ella.

📡 Importancia:
Necesario para simulaciones de red reales
Permite:
Sniffing (Wireshark)
Protocolos de capa 2 (ARP, STP)
⚙️ Configuración:

VirtualBox → Red → Adaptador →
Promiscuous Mode: Allow All
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/d2473a81-b9c8-4a36-8901-3faf04589af5" />


📌 4. Integración con VMware ESXi (Remoto)
🔹 Arquitectura Cliente-Servidor

En este modelo:

Laptop (Cliente) → Ejecuta GNS3 GUI
Servidor ESXi (Remoto) → Ejecuta GNS3 VM
🔗 Flujo:

GUI → API → GNS3 Server → ESXi → VM

✔️ Ventajas:
Mayor potencia (servidor físico)
Escalabilidad
Simulaciones grandes
🔹 Seguridad en vSwitch (ESXi)

En VMware ESXi, los vSwitch tienen políticas de seguridad clave:

🔒 Políticas:
Promiscuous Mode
Permite ver tráfico de otras VM
MAC Address Changes
Permite cambiar MAC dinámicamente
Forged Transmits
Permite enviar tráfico con MAC diferente
⚠️ Importancia en GNS3:
Deben estar en "Accept"
Caso contrario:
Tráfico bloqueado
Fallas en simulaciones
<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/44a524bc-581e-44ab-874c-a4bab4f45f67" />

📌 5. Matriz de Solución de Errores (Troubleshooting)
<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/3eacacda-344d-4181-b1c3-ef63cd5c867d" />
