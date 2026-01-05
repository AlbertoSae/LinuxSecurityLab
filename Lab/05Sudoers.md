# Módulo 05: Control de Privilegios y Sudoers

Este último módulo detalla la configuración del archivo `/etc/sudoers` para delegar tareas administrativas a usuarios específicos, evitando el uso compartido de la cuenta `root`.



## Objetivo de Seguridad
Implementar el **Principio de Mínimo Privilegio**, permitiendo que los usuarios realicen tareas de mantenimiento necesarias para su rol sin comprometer la integridad total del sistema.

---
##  Configuración mediante `visudo`
Toda modificación se ha realizado utilizando el comando `sudo visudo`, que valida la sintaxis antes de guardar el archivo para evitar bloqueos del sistema.

### 1. Privilegios Totales (Sysadmin)
El usuario `sysadmin` es el administrador principal del laboratorio. Se le otorga capacidad de ejecución de cualquier comando en cualquier nodo.

```bash
# Definición en /etc/sudoers
sysadmin ALL=(ALL:ALL) ALL
```
### 2. Delegación a Supervisores de Departamento
   Para que los supervisores puedan gestionar sus propios servicios o consultar logs del sistema sin ser administradores totales:

```bash
# Los supervisores de Sistemas pueden reiniciar el servidor web sin contraseña
%sistemas ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart apache2, /usr/bin/systemctl status apache2

# Los supervisores de Administración pueden consultar los logs de autenticación
%administracion ALL=(ALL) /usr/bin/tail -f /var/log/auth.log
```
---
## Pruebas de Escalada de Privilegios

| Usuario        | Comando ejecutado            | Resultado | Motivo                                         |
|----------------|------------------------------|-----------|------------------------------------------------|
| sysadmin       | sudo su -                    | ÉXITO     | Tiene permisos ALL                             |
| ana (Admin)    | sudo tail /var/log/auth.log  | ÉXITO     | Permitido explícitamente en sudoers            |
| ana (Admin)    | sudo apt update              | DENEGADO  | Comando no autorizado para su rol              |
| maria (RW)     | sudo ...                     | DENEGADO  | El usuario no está en el archivo sudoers       |

---
## Auditoría de Comandos
Cada vez que un usuario utiliza sudo, el sistema registra el evento. Podemos auditar estos intentos (exitosos y fallidos) en:
```bash
# Ver quién ha intentado usar privilegios
sudo cat /var/log/auth.log | grep sudo
```
---
## Conclusiones Finales del Laboratorio
Tras completar los 5 módulos, hemos transformado un sistema Linux estándar en un entorno corporativo seguro:

* **Aislamiento de Red:** Entorno estanco con Red Interna.

* **Identidad Lógica:** Grupos por departamentos y niveles de acceso.

* **Seguridad Base:** Permisos POSIX con SGID para colaboración.

* **Flexibilidad:** ACLs para accesos cruzados y supervisión.

* **Control de Poder:** Delegación segura mediante Sudoers.
