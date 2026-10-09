# AWS Academy: Insert, Update, and Delete Data in a Database

## 1. Descripción del laboratorio

En este laboratorio practiqué operaciones básicas de manipulación de datos en una base de datos relacional utilizando MariaDB y servicios de Amazon Web Services (AWS).

El objetivo fue aprender a insertar nuevos registros, actualizar información existente, eliminar registros y restaurar los datos originales.

## 2. Objetivos de aprendizaje

- Conectarse a una instancia EC2 mediante Session Manager.
- Acceder a MariaDB desde una terminal Linux.
- Consultar bases de datos y tablas.
- Insertar registros utilizando `INSERT`.
- Modificar registros utilizando `UPDATE`.
- Eliminar registros utilizando `DELETE`.
- Restaurar los datos desde un archivo SQL.
- Verificar la recuperación de la información.

## 3. Servicios y tecnologías utilizados

| Tecnología | Uso |
|---|---|
| Amazon EC2 | Instancia utilizada para acceder al entorno del laboratorio. |
| AWS Systems Manager Session Manager | Conexión a la instancia sin utilizar una conexión SSH tradicional. |
| MariaDB | Sistema de gestión de bases de datos relacionales. |
| SQL | Lenguaje utilizado para consultar y modificar información. |
| Linux | Entorno de terminal utilizado para ejecutar los comandos. |

## 4. Desarrollo del laboratorio

### Paso 1: Conexión a la instancia

Accedí a la consola de AWS, ingresé a Amazon EC2 y seleccioné la instancia `Command Host`.

Luego utilicé Session Manager para abrir una terminal y ejecutar los comandos necesarios.

### Paso 2: Conexión a MariaDB

Utilicé el siguiente comando para conectarme al servidor de base de datos:

```bash
mysql -u root --password='[CONTRASEÑA]'
```

*Nota: la contraseña real no se incluye en este documento por seguridad.*

### Paso 3: Verificación de la base de datos

Comprobé las bases de datos disponibles:

```sql
SHOW DATABASES;
```

Luego consulté los registros de la tabla de países:

```sql
SELECT * FROM world.country;
```

Al inicio, la tabla estaba vacía.

### Paso 4: Inserción de registros

Utilicé `INSERT` para agregar dos países: Irlanda y Australia.

```sql
INSERT INTO world.country VALUES
('IRL','Ireland','Europe','British Islands',
70273.00,1921,3775100,76.8,75921.00,73132.00,
'Ireland/Éire','Republic',1447,'IE');

INSERT INTO world.country VALUES
('AUS','Australia','Oceania','Australia and New Zealand',
7741220.00,1901,18886000,79.8,351182.00,392911.00,
'Australia','Constitutional Monarchy, Federation',135,'AU');
```

Verifiqué los registros insertados:

```sql
SELECT * FROM world.country
WHERE Code IN ('IRL', 'AUS');
```

**Resultado:** ambos países se insertaron correctamente.

### Paso 5: Actualización de datos

Primero cambié la población de los registros a cero:

```sql
UPDATE world.country
SET Population = 0;
```

Después modifiqué la población y la superficie:

```sql
UPDATE world.country
SET Population = 100, SurfaceArea = 100;
```

Finalmente, utilicé `SELECT` para verificar los cambios.

**Aprendizaje:** cuando una sentencia `UPDATE` no incluye una cláusula `WHERE`, se modifican todos los registros de la tabla.

### Paso 6: Eliminación de registros

Desactivé temporalmente las comprobaciones de claves foráneas en la sesión:

```sql
SET FOREIGN_KEY_CHECKS = 0;
```

Después eliminé los registros de la tabla:

```sql
DELETE FROM world.country;
```

Verifiqué el resultado:

```sql
SELECT * FROM world.country;
```

**Resultado:** la tabla quedó vacía, pero su estructura se mantuvo.

### Paso 7: Restauración de los datos

Salí de MariaDB y comprobé que existiera el archivo de respaldo:

```bash
ls /home/ec2-user/world.sql
```

Luego restauré los datos mediante:

```bash
mysql -u root --password='[CONTRASEÑA]' < /home/ec2-user/world.sql
```

Volví a conectarme a MariaDB y seleccioné la base de datos:

```sql
USE world;
```

Comprobé las tablas restauradas:

```sql
SHOW TABLES;
```

Las tablas disponibles fueron:

- `city`
- `country`
- `countrylanguage`

Finalmente, consulté los países:

```sql
SELECT * FROM country;
```

**Resultado:** se restauraron los datos originales y la tabla `country` mostró 237 registros.

## 5. Comandos SQL aprendidos

| Comando | Función |
|---|---|
| `SHOW DATABASES` | Muestra las bases de datos disponibles. |
| `SELECT` | Consulta registros. |
| `INSERT` | Inserta nuevos registros. |
| `UPDATE` | Modifica registros existentes. |
| `DELETE` | Elimina registros. |
| `USE` | Selecciona una base de datos. |
| `SHOW TABLES` | Muestra las tablas de una base de datos. |
| `SET FOREIGN_KEY_CHECKS` | Controla las comprobaciones de claves foráneas en la sesión. |

## 6. Buenas prácticas de seguridad

- Revisar los registros antes de ejecutar operaciones de modificación o eliminación.
- Utilizar `WHERE` en `UPDATE` y `DELETE` cuando se necesita afectar solo determinados registros.
- Mantener copias de respaldo antes de realizar cambios importantes.
- No publicar contraseñas, claves SSH ni credenciales de AWS en GitHub.
- Verificar los resultados después de cada operación.

## 7. Conclusión

Este laboratorio me permitió practicar las operaciones fundamentales de manipulación de datos en MariaDB dentro de un entorno AWS.

Aprendí a insertar, consultar, actualizar y eliminar registros, además de restaurar una base de datos desde un archivo SQL.

También comprendí la importancia de verificar los cambios y mantener respaldos para recuperar información cuando sea necesario.

## 8. Estado final

- [x] Conexión a EC2 mediante Session Manager.
- [x] Conexión a MariaDB.
- [x] Inserción de Irlanda y Australia.
- [x] Actualización de población y superficie.
- [x] Eliminación de registros.
- [x] Restauración de los datos originales.
- [x] Verificación de las tres tablas y los 237 registros de países.

---

**Proyecto:** AWS Academy — Práctica de bases de datos  
**Área:** Cloud Computing / Bases de datos / SQL



## Evidencias del laboratorio

### Restauración de la base de datos

Se restauró la base de datos original y se verificó que la tabla `country` contuviera 237 registros.

![Restauración de la base de datos](imagenes/03-restauracion.png)