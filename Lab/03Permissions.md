# Módulo 03: Control de Acceso Clásico (Permisos POSIX)

Este módulo describe la implementación de la estructura de directorios y la asignación de permisos tradicionales de Linux para garantizar que cada departamento acceda únicamente a su información.



## Estructura de Directorios y Propiedad
Se ha creado una estructura jerárquica en `/empresa`. La propiedad de las carpetas principales pertenece a `root`, pero el grupo se asigna al nivel `_super` de cada departamento.

| Directorio | Propietario | Grupo | Permisos (Octal) | Explicación |
| :--- | :--- | :--- | :--- | :--- |
| `/empresa` | `root` | `root` | `755` | Lectura pública, escritura solo root. |
| `/empresa/administracion` | `root` | `administracion` | `2770` | Full acceso a supervisores + SGID. |
| `/empresa/rrhh` | `root` | `rrhh` | `2770` | Full acceso a supervisores + SGID. |
| `/empresa/sistemas` | `root` | `sistemas` | `2770` | Full acceso a supervisores + SGID. |

## El bit especial: SGID (Set Group ID)
Para permitir el trabajo colaborativo, se ha aplicado el bit **SGID** (el `2` en `2770`).
* **Función:** Cualquier archivo o subdirectorio creado dentro de la carpeta heredará automáticamente el grupo del directorio padre (ej. `administracion`) en lugar del grupo primario del usuario que lo crea.
* **Resultado:** Si `ana` crea un informe, `juan` podrá editarlo inmediatamente porque ambos comparten el grupo del directorio.

## Comandos de Implementación

### 1. Creación y asignación de propietarios
```bash
sudo mkdir -p /empresa/{administracion,rrhh,sistemas}
sudo chown root:administracion /empresa/administracion
sudo chown root:rrhh /empresa/rrhh
sudo chown root:sistemas /empresa/sistemas
```