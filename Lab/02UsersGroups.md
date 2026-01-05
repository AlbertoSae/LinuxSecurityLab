# Módulo 02: Diseño de Identidades (Usuarios y Grupos)

Este módulo describe la arquitectura de seguridad lógica implementada en el servidor, basada en el modelo de **Control de Acceso Basado en Roles (RBAC)**.

## Diseño de Grupos y Roles
Se ha establecido una jerarquía de tres niveles para cada departamento para garantizar el principio de mínimo privilegio:

* **_super:** Supervisores con acceso total y capacidad de gestión.
* **_rw:** Personal operativo con permisos de lectura y escritura.
* **_ro:** Consultores o auditores con permisos de solo lectura.

### Matriz de Usuarios y Grupos

| Departamento | Grupo (GID) | Rol | Usuarios |
| :--- | :--- | :--- | :--- |
| **Administración** | `administracion` (1001) | Supervisor | ana, juan |
| | `administracion_rw` (1004) | Operativo RW | maria |
| | `administracion_ro` (1005) | Lectura RO | sergio |
| **RRHH** | `rrhh` (1002) | Supervisor | laura, pedro |
| | `rrhh_rw` (1006) | Operativo RW | andrea |
| | `rrhh_ro` (1007) | Lectura RO | roberto |
| **Sistemas** | `sistemas` (1003) | Supervisor | sysadmin |
| | `sistemas_rw` (1008) | Operativo RW | carlos |
| | `sistemas_ro` (1009) | Lectura RO | elena |

## Política de Contraseñas
Para este laboratorio se ha definido una política de caducidad de credenciales para mejorar la postura de seguridad:
* **Vigencia:** 90 días.
* **Fecha de referencia:** 07/11/2025.
* **Formato de credencial:** `nombre + GID` (Ejemplo: `ana1001`).

---

## Implementación Técnica (Bash)

### 1. Creación de Grupos con GID específicos
```bash
sudo groupadd -g 1001 administracion
sudo groupadd -g 1004 administracion_rw
sudo groupadd -g 1005 administracion_ro
# ... repetir para rrhh y sistemas
```
### 2. Creación de Usuarios y asignación de grupos
```bash
# Ejemplo Departamento Administración
sudo useradd -m -g administracion -s /bin/bash ana
sudo useradd -m -g administracion_rw -s /bin/bash maria
sudo useradd -m -g administracion_ro -s /bin/bash sergio

# Configuración de caducidad (90 días)
sudo chage -M 90 ana
```
### 3. Verificación de la estructura
   Para comprobar que los usuarios están en sus grupos correctos:
```bash
getent group | grep -E 'administracion|rrhh|sistemas'
id ana
```
---
## Estructura de Directorios Proyectada
La creación de usuarios soporta la siguiente estructura de archivos en /empresa:
```bash
/EMPRESA
├── Administración
│   ├── documentos (Acceso _rw)
│   └── informes (Acceso _ro)
├── RRHH
│   ├── candidatos (Acceso _ro)
│   └── nominas (Acceso _rw)
└── Sistemas
```
---
## Conclusiones
### Segregación de funciones: 
El uso de sufijos _rw y _ro permite escalar el laboratorio fácilmente añadiendo nuevos empleados sin modificar los permisos de las carpetas.

### Gestión de IDs: 
La asignación manual de UID/GID asegura la consistencia en caso de migrar estos datos a otros sistemas o servicios de directorio (LDAP).

### Seguridad Activa: 
La política de caducidad de contraseñas con chage demuestra una gestión de identidades proactiva.
