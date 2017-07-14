+++
date = "2017-04-15T14:39:04+02:00"
title = "Espacio de trabajo"
url = "es/workspace"

[menu.usage]
  weight = 2
  identifier = "workspace-es"
  parent = "usage_concepts"
+++

El espacio de trabajo define el volumen y directorio compartido por todos los pasos del flujo de trabajo. El espacio de trabajo por defecto sigue el siguiente patrón, basado en la url de tu repositorio.

```
/drone/src/github.com/octocat/hello-world
```

El espacio de trabajo puede ser personalizado usando un bloque en el archivo YAML:

```diff
+workspace:
+  base: /go
+  path: src/github.com/octocat/hello-world

pipeline:
  build:
    image: golang:latest
    commands:
      - go get
      - go test
```

El atributo base define el volumen base disponible para todos los pasos del flujo de trabajo. Esto asegura que tu código fuente, dependencias y binarios compilados persistan entre los pasos intermedios.

```diff
workspace:
+ base: /go
  path: src/github.com/octocat/hello-world

pipeline:
  deps:
    image: golang:latest
    commands:
      - go get
      - go test
  build:
    image: node:latest
    commands:
      - go build
```

Esto sería equivalente a los siguientes comandos docker:

```
docker volume create my-named-volume

docker run --volume=my-named-volume:/go golang:latest
docker run --volume=my-named-volume:/go node:latest
```

El atributo path define el directorio de trabajo de tu compilación. Esto es donde tu código es clonado y será el directorio de trabajo por defecto de cada paso de tu proceso de contrucción. El path debe ser relativo y es combinado con tu path base.

```diff
workspace:
  base: /go
+ path: src/github.com/octocat/hello-world
```

```text
git clone https://github.com/octocat/hello-world \
  /go/src/github.com/octocat/hello-world
```
