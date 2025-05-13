# Respaldo Inicial y Limpieza del Script SQL

Este documento explica paso a paso cómo realizar un respaldo inicial de una base de datos Oracle, limpiarlo y prepararlo para restauración y versionado. Se utilizaron Oracle Cloud, Docker y SQL Developer de Oracle.

---

## Requisitos Previos

- Docker instalado y funcionando.
- Oracle SQL Developer instalado.
- Tener acceso a tu base de datos Oracle (remota o en la nube).
- Saber exportar un respaldo desde SQL Developer.
- Conocimientos básicos de Git.

---

## Paso 1: Crear el Respaldo desde SQL Developer

1. Abre Oracle SQL Developer.
2. Conéctate a la base de datos del proyecto (por ejemplo, en Oracle Cloud).
3. Da clic derecho sobre el esquema (usuario) que deseas respaldar y selecciona **Exportar**.
4. En el asistente de exportación:
   - Selecciona solo objetos como: `Tables`, `Views`, `Sequences`, `Indexes`, etc.
   - Desactiva cualquier opción que exporte datos (`INSERT`, `Data`, etc.).
   - Elige como formato de salida: `SQL Script`.
   - Guarda el archivo como `respaldo.sql`.

Resultado: tendrás un archivo `.sql` con las estructuras de tu base de datos.

---

## Paso 2: Limpiar el Script Exportado

### A. Eliminar las sentencias `INSERT INTO`

- Abre `respaldo.sql` en tu editor de texto.
- Borra todas las líneas que comienzan con `INSERT INTO`.

Y reemplazarlo con nada (vacío).

---

### B. Eliminar cláusulas innecesarias como `TABLESPACE` y `COLLATE`

- En las sentencias `CREATE TABLE` o `CREATE INDEX`, elimina lo siguiente:
- `TABLESPACE users` (o cualquier otro tablespace).
- Cualquier cláusula `COLLATE` (si está presente).

Ejemplo antes:

```
sql
CREATE TABLE usuarios (
  id NUMBER PRIMARY KEY
) TABLESPACE users;
```

Después:

```
CREATE TABLE usuarios (
    id NUMBER PRIMARY KEY
);
```

## Paso 3: Probar restauración en Entorno Local

1. Levantar Oracle en Docker 
docker run -d -p 1521:1521 -e ORACLE_PASSWORD=TuContraseñaSegura gvenzl/oracle-free:slim-faststart

Espera a que el contenedor esté completamente iniciado.

El servicio estará disponible en:

    - Usuario: SYSTEM
    - Contraseña: TuContraseñaSegura
    - Verificar el puerto

2. Conectarse desde SQL Developer

En SQL Developer, crea una nueva conexión con:

    - Usuario: system
    - Contraseña: la definida en el comando Docker
    - Seleccionar SID

Ejecuta el script .sql limpio.

Si se ejecuta sin errores, tu estructura ha sido restaurada correctamente.

## Resultado Final

Ahora tienes un script limpio y funcional, que representa únicamente la estructura de tu base de datos. Este script puede versionarse en Git como V0__base.sql dentro de una carpeta sql-migrations.




