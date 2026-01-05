# Linux Security Lab: Users, Groups & RBAC

Laboratorio avanzado de administración de sistemas Linux enfocado en el **Control de Acceso Basado en Roles (RBAC)**, seguridad de sistemas de archivos y segregación de funciones.



## Objetivos del Proyecto
* **Gestión de Identidades:** Diseño de una jerarquía lógica de grupos (Supervisores, Editores, Lectores).
* **Seguridad de Archivos:** Implementación de permisos **POSIX** (incluyendo bits especiales como **SGID** y **Sticky Bit**).
* **Control Fino:** Uso de **ACLs** para gestionar excepciones de acceso sin comprometer la estructura de grupos.
* **Mínimo Privilegio:** Configuración granular del archivo `sudoers` para limitar la capacidad de administración.
* **Auditoría de Acceso:** Verificación de denegación de servicios desde entornos externos (Kali Linux).

##  Entorno del Laboratorio
El entorno está compuesto por tres máquinas virtuales aisladas en una red interna para simular un entorno corporativo real:

| Rol | Sistema Operativo | Función |
| :--- | :--- | :--- |
| **Servidor** | Ubuntu Server 22.04 | Host central de recursos y gestión de usuarios. |
| **Cliente** | Debian 12 | Estación de trabajo para administración remota vía SSH. |
| **Auditor** | Kali Linux | Pruebas de seguridad, escaneo de red y validación de permisos. |

##  Tecnologías y Herramientas
* **Virtualización:** VirtualBox
* **Redes:** Configuración de adaptadores *Host-Only* para aislamiento total.
* **Servicios:** SSH (acceso remoto), Bash Scripting (automatización).
* **Seguridad:** `getfacl/setfacl`, `visudo`, `chown/chmod`.

---

##  Guía del Laboratorio
El proyecto está dividido en módulos incrementales para facilitar su reproducción:

1. [**Despliegue de Red y VMs**](lab/01-vms.md): Configuración de las interfaces y conectividad.
2. [**Diseño de Usuarios y Grupos**](lab/02-usuarios-grupos.md): Lógica de departamentos y roles.
3. [**Permisos y Colaboración**](lab/03-permisos-posix.md): Estructura de directorios y bits especiales.
4. [**ACLs y Excepciones**](lab/04-acl.md): Casos de uso avanzados de acceso.
5. [**Privilegios Sudo**](lab/05-sudoers.md): Delegación de tareas administrativas.

> [!IMPORTANT]
> **Aviso de Seguridad:** Este laboratorio ha sido diseñado con fines exclusivamente educativos. Las contraseñas utilizadas son genéricas y el entorno está aislado de internet.

---

## Conclusiones Generales
Este laboratorio demuestra la importancia de una planificación previa de la estructura de grupos antes de la implementación. La combinación de permisos tradicionales y ACLs permite un equilibrio entre seguridad estricta y flexibilidad operativa.