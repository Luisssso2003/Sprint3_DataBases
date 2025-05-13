
## Archivo 2: `Control de Versiones Manual`

# Control de Versiones Manual del Esquema de Base de Datos

Este documento describe cómo implementar un sistema manual de control de versiones para los cambios en el esquema de una base de datos Oracle, utilizando archivos `.sql` y Git.

---

## Requisitos

- Tener un proyecto con una base de datos Oracle (puede estar en Docker).
- Haber generado y limpiado un respaldo `.sql` de la estructura (ver archivo anterior).
- Git instalado y un repositorio configurado para tu proyecto.

---

## Paso 1: Establecer la Versión Base

### 1. Crear Carpeta para Migraciones

En tu proyecto, crea una carpeta donde guardarás los scripts de migración:

```bash
mkdir sql-migrations
```

### 2. Agregar el Script Base Limpio

Copia el script que limpiaste anteriormente (respaldo.sql limpio) a esta carpeta y renómbralo como:

`sql-migrations/V0__base.sql`

Este archivo representa el estado inicial del esquema.

## Paso 2: Crear la Primera Migración (V1)

### 1. Hacer el Cambio en la Base de Datos

En tu entorno local (Docker), conéctate con SQL Developer corriendo el comando:

docker run -d -p 1521:1521 -e ORACLE_PASSWORD=TuContraseñaSegura gvenzl/oracle-free:slim-faststart

### 2. Crear Script de Migración
Dentro de sql-migrations/, crea un nuevo archivo llamado:

`V1__base.sql`

Contenido del archivo:

`ALTER TABLE users ADD ("El nombre de la columna que desees crear" VARCHAR2(100));`

Con esto tendrás ambas versiones de los archivos base.
