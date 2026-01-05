# Módulo 01: Despliegue de Red y Máquinas Virtuales

Este módulo detalla la infraestructura base utilizada para el laboratorio, garantizando un entorno aislado y controlado para las pruebas de privilegios y seguridad.

##  Arquitectura del Entorno
Se ha configurado una **Red Interna (Internal Network)** en VirtualBox bajo el nombre `lab_net`. Esta configuración garantiza que las máquinas se comuniquen entre sí en un segmento privado, sin salida a internet ni contacto con la red del anfitrión (Host).

### Inventario de Máquinas Virtuales

| Máquina | Sistema Operativo | RAM | CPUs | Almacenamiento | Interfaz de Red |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **server-lab** | Ubuntu (64-bit) | 3072 MB | 3 | 25.00 GB | Internal Network ('lab_net') |
| **cliente-lab** | Debian (64-bit) | 3072 MB | 4 | 25.00 GB | Internal Network ('lab_net') |
| **kali-lab** | Kali Linux | 2048 MB | 2 | 25.00 GB | Internal Network ('lab_net') |

## Gestión de Direccionamiento (DHCP)
A diferencia de configuraciones con IP estática, este laboratorio utiliza el **servidor DHCP integrado de VirtualBox** para la red interna. Esto simula un entorno donde los nodos obtienen su identidad de red de forma automática al iniciar.

### Identificación de nodos
Al ser un direccionamiento dinámico, el primer paso en cada sesión de trabajo es identificar la IP asignada mediante el comando:

```bash
ip a | grep inet
```
---
## Comprobación de conectividad

Una vez identificadas las IPs (ej. `10.0.2.15` y `10.0.2.16`), se verifica la visibilidad entre el cliente **Debian** y el servidor **Ubuntu** mediante el protocolo **ICMP**:

```bash
# Prueba de eco desde el cliente Debian hacia el servidor Ubuntu
ping -c 4 <IP_ASIGNADA_AL_SERVIDOR>
```
---
## Acceso Remoto (Administración)

La administración del servidor se realiza de forma segura a través de SSH desde la máquina Debian. Esto permite trabajar sobre el servidor desde un terminal remoto, simulando la operativa real de un administrador de sistemas.

### Comando de conexión desde el terminal de Debian
```bash
ssh usuario_admin@<IP_DINAMICA_SERVIDOR>
```
---
## Pasos de Implementación

#### Configuración de VirtualBox:
En la sección de Red de cada VM, seleccionar Red Interna y nombrar el canal exactamente como lab_net.

#### Habilitación de DHCP:
El servidor DHCP de VirtualBox gestiona automáticamente el direccionamiento para este segmento.

#### Instalación de SO:
Instalación estándar de Ubuntu Server (CLI), Debian (GUI) y Kali Linux (Pentesting Tools).

#### Instalación y Activación de SSH en el Servidor:
```bash
sudo apt update && sudo apt install openssh-server -y
sudo systemctl enable --now ssh
 ```
---
## Conclusiones

#### Aislamiento:
El uso de una Red Interna es crítico en laboratorios de seguridad para evitar que herramientas de escaneo (como las de Kali) interactúen accidentalmente con la red real.

#### Flexibilidad:
El uso de DHCP facilita el despliegue rápido, aunque requiere la identificación de IPs mediante 'ip a' antes de iniciar la administración remota.

#### Escalabilidad:
Esta base permite añadir nuevos clientes (como el nodo de Kali) simplemente uniéndolos al mismo nombre de red interna.
