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
---
## Comandos de Implementación

### 1. Creación y asignación de propietarios
```bash
sudo mkdir -p /empresa/{administracion,rrhh,sistemas}
sudo chown root:administracion /empresa/administracion
sudo chown root:rrhh /empresa/rrhh
sudo chown root:sistemas /empresa/sistemas
```

### 2. Aplicación de permisos restrictivos y SGID
```bash
# 2=SGID, 7=Propietario(rwx), 7=Grupo(rwx), 0=Otros(---)
sudo chmod 2770 /empresa/administracion
sudo chmod 2770 /empresa/rrhh
sudo chmod 2770 /empresa/sistemas
```

### 3. Subdirectorios específicos (Operativos)
```bash
sudo mkdir /empresa/rrhh/nominas
sudo chown root:rrhh_rw /empresa/rrhh/nominas
sudo chmod 2770 /empresa/rrhh/nominas
``` 
---
## Pruebas de Verificación

| Usuario             | Acción                              | Resultado esperado                                              |
|---------------------|-------------------------------------|-----------------------------------------------------------------|
| ana (admin_super)   | Crear archivo en /administracion    | ÉXITO                                                           |
| maria (admin_rw)    | Editar archivo en /administracion   | DENEGADO (Solo tiene acceso a sus subcarpetas)                  |
| laura (rrhh_super)  | Entrar en /administracion           | DENEGADO (Permiso 2770 bloquea a otros)                         |

## Conclusiones
### Aislamiento Departamental: 
El uso de permisos 770 asegura que ningún usuario ajeno al grupo pueda siquiera listar el contenido de la carpeta de otro departamento.

### Colaboración Automática:
El bit SGID evita que el administrador tenga que estar corrigiendo permisos de archivos nuevos constantemente.

### Limitación POSIX:
Se observa que con POSIX es difícil dar permiso a un tercer grupo (ej. que Sistemas lea RRHH) sin hacerlo público, lo que justifica el uso de ACLs en el siguiente módulo.