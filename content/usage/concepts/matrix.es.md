+++
date = "2017-04-15T14:39:04+02:00"
title = "Matrices de contrucción"
url = "es/matrix-builds"

[menu.usage]
  weight = 10
  identifier = "matrix-builds-es"
  parent = "usage_concepts"
+++

Drone ha integrado soporte para matrices de contrucción. Drone ejecuta una tarea de compilación por cada combinación en la matriz, permitiendo a tu compilación probar un único commit en múltiples configuraciones.

Ejemplo de definición de matrix:

```yaml
matrix:
  GO_VERSION:
    - 1.4
    - 1.3
  REDIS_VERSION:
    - 2.6
    - 2.8
    - 3.0
```

La definición de matriz puede contener solo combinaciones específicas:

```yaml
matrix:
  include:
    - GO_VERSION: 1.4
      REDIS_VERSION: 2.8
    - GO_VERSION: 1.5
      REDIS_VERSION: 2.8
    - GO_VERSION: 1.6
      REDIS_VERSION: 3.0
```

# Interpolación

Las variables de la matriz son interpoladas en el YAML usando la sintaxis `${VARIABLE}`, antes de que el yaml sea analizado. Este es un ejemplo de archivo YAML usando parámetros de interpolación en la matriz:

```yaml
pipeline:
  build:
    image: golang:${GO_VERSION}
    commands:
      - go get
      - go build
      - go test

services:
  database:
    image: ${DATABASE}

matrix:
  GO_VERSION:
    - 1.4
    - 1.3
  DATABASE:
    - mysql:5.5
    - mysql:6.5
    - mariadb:10.1
```

Ejemplo de configuración YAML inyectando parámetros en la matriz:

```diff
pipeline:
  build:
-   image: golang:${GO_VERSION}
+   image: golang:1.4
    commands:
      - go get
      - go build
      - go test
+   environment:
+     - GO_VERSION=1.4
+     - DATABASE=mysql:5.5

services:
  database:
-   image: ${DATABASE}
+   image: mysql:5.5
```

# Ejemplos

Ejemplo de matriz de compilación basado en la etiqueta de la imagen de Docker:

```yaml
pipeline:
  build:
    image: golang:${TAG}
    commands:
      - go build
      - go test

matrix:
  TAG:
    - 1.7
    - 1.8
    - latest
```

Ejemplo de matriz de compilación basada en la imager de Docker

```yaml
pipeline:
  build:
    image: ${IMAGE}
    commands:
      - go build
      - go test

matrix:
  IMAGE:
    - golang:1.7
    - golang:1.8
    - golang:latest
```
