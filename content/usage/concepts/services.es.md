---
date: 2017-04-15T14:39:04+02:00
title: Servicios
url: es/services

next_steps:
  - file: mysql.md
    name: Example using MySQL
  - file: postgres.md
    name: Example using Postgres
  - file: redis.md
    name: Example using Redis

menu:
  usage:
    weight: 6
    identifier: services-es
    parent: usage_concepts
---

Drone provee una sección de servicios en el archivo YAML usada para definir contenedores de servicio. La siguiente configuración compone una base de datos y un servidor de cache.

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

services:
  database:
    image: mysql

  cache:
    image: redis
```

Los servicios son accesados usando nombres de host personalizados. En el ejemplo anterior, el servicio mysql es asignado con el nombre de dominio `database` y estará disponible en `database:3306`.

# Configuración

Los contenedores de servicios generalmente exponen variables de configuración personalizables en el inicio, como nombres de usuario por defecto, contraseñas y puertos. Por favor vea la documentación oficial de cada imagen para aprender mas.

```diff
services:
  database:
    image: mysql
+   environment:
+     - MYSQL_DATABASE=test
+     - MYSQL_ALLOW_EMPTY_PASSWORD=yes

  cache:
    image: redis
```

# Inicialización

Los contenedores de servicios requieren tiempo para inicializarse y comenzar a aceptar conecciones. Si no eres capaz de conectarte con un servicio, deberías esperar algunos segudos o implementar una marcha atrás.

```diff
pipeline:
  test:
    image: golang
    commands:
+     - sleep 15
      - go get
      - go test

services:
  database:
    image: mysql
```
