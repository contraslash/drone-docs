+++
date = "2017-04-15T14:39:04+02:00"
title = "Configurar Base de Datos"
url = "database-settings"

[menu.install]
  identifier = "database-settings-es"
  parent = "install_server"
  weight = 1
+++

Esta guía provee instrucciones para usar motores de almacenamiento. Nota que esto es opciona. El motor de almacenamiento por defecto está embebido en una base de datos SQLite, la cual no requiere ninguna instalación o configuración.

# Configurar MySQL

El ejemplo de a continuación muestra la configuración de una base de datos MySQL. Visita la [documentación](https://github.com/go-sql-driver/mysql#dsn-data-source-name) del manejador oficial de mysql para ver opciones de configuración y ejemplos.

```diff
version: '2'

services:
  drone-server:
    image: drone/drone:{{% version %}}
    environment:
+     DRONE_DATABASE_DRIVER: mysql
+     DRONE_DATABASE_DATASOURCE: root:password@tcp(1.2.3.4:3306)/drone?parseTime=true
```

# Configurar Postgres

El ejemplo de a continuación muestra la configuración de una base de datos postgres. Visita la [documentación](https://www.postgresql.org/docs/current/static/libpq-connect.html#LIBPQ-CONNSTRING) oficial de postgres para opciones de configuración y ejemplos.

```diff
version: '2'

services:
  drone-server:
    image: drone/drone:{{% version %}}
    environment:
+     DRONE_DATABASE_DRIVER: postgres
+     DRONE_DATABASE_DATASOURCE: postgres://root:password@1.2.3.4:5432/postgres?sslmode=disable
```

# Creación de base de datos

Drone no crea la base de datos automáticamente. Si estás usando MySQL o Postgres, debes crear la base de datos manualmente usando la sentencia `CREATE DATABASE `.

# Migración de base de datos

Drone automáticamente maneja la migración de base de datos, incluyendo la creación de tablas e índices. Las nuevas versiones de Drone automáticamente se actualizarán a menos que se especifique lo contrario en las notas de lanzamiento.

# Copias de seguridad de base de datos

Drone no ejecuta copias de seguridad de base de datos. Esto debería ser manejado con una aplicación externa proveída por tu proveedor de base de datos de elección.

# Archivado de base de datos

Drone no ejecuta archivados de datos, está considerado fuera del alcance del proyecto. Drone es mas bien conservador con la cantidad de datos que almacena, sin embargo, puedes esperar que los archivos de monitoreo crezcan considerablemente.
