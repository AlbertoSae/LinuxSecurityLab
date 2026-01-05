# Módulo 04: ACL (Access Control Lists)

Este módulo aborda la resolución de escenarios complejos donde los permisos POSIX tradicionales resultan insuficientes. Las ACL permiten asignar permisos a múltiples usuarios o grupos sin alterar la propiedad principal del directorio.



## ¿Por qué ACL?
En los módulos anteriores, vimos que el directorio `/empresa/rrhh` pertenece al grupo `rrhh`. Sin embargo, los requerimientos del laboratorio exigen:

**Acceso Total:** El `sysadmin` debe poder gestionar cualquier carpeta.

**Lectura Cruzada:** Los supervisores de cada departamento (`_super`) deben poder leer la información de los otros departamentos para tareas de coordinación, sin ser dueños de las carpetas.

---

## Preparación del Sistema
Las ACL deben estar soportadas por el sistema de archivos.

```bash
# Instalación de herramientas de gestión
sudo apt update && sudo apt install acl -y

# Verificación de soporte en el montaje
mount | grep -i acl
```
---
## Implementación de Casos Prácticos
### Caso 1: Privilegios Totales para el Sysadmin
Para garantizar que el sysadmin tenga control total sobre toda la estructura de la empresa:

```bash
# Otorgar rwx al usuario sysadmin de forma recursiva en toda la raíz del lab
sudo setfacl -R -m u:sysadmin:rwx /empresa
```
### Caso 2: Lectura Interdepartamental (Supervisores)
Para que los supervisores puedan consultar documentos de otras áreas (ej. que los supervisores de Administración puedan leer RRHH y Sistemas):
```bash
# Permitir que el grupo administracion lea RRHH
sudo setfacl -m g:administracion:rx /empresa/rrhh

# Permitir que el grupo rrhh lea Sistemas
sudo setfacl -m g:rrhh:rx /empresa/sistemas

# Permitir que el grupo sistemas lea Administración
sudo setfacl -m g:sistemas:rx /empresa/administracion
```
---
## Pruebas de Auditoría
| Usuario   | Directorio     | Grupo Origen        | Resultado                                               |
|-----------|----------------|---------------------|---------------------------------------------------------|
| sysadmin  | /empresa/rrhh  | sistemas            | ÉXITO (RWX) vía ACL de usuario                          |
| ana       | /empresa/rrhh  | administracion      | ÉXITO (R-X) vía ACL de grupo                            |
| maria     | /empresa/rrhh  | administracion_rw   | DENEGADO (Solo los _super tienen la ACL)                |

---
## Conclusiones
### Granularidad:
Hemos logrado que los supervisores tengan visibilidad total de la empresa sin comprometer la integridad de los datos (ya que solo pueden leer, no borrar ni editar fuera de su área).

### Control de Auditoría:
El sysadmin queda registrado como un usuario con permisos explícitos, facilitando la auditoría de quién tiene acceso a qué.

