+++
date = "2017-04-15T14:39:04+02:00"
title = "Webhooks"
url = "es/hooks"

[menu.usage]
  weight = 1
  identifier = "hooks-es"
  parent = "usage_concepts"
+++

Cuando activas tu repositorio, Drone automáticamente agrega un webhook a tu sistema de control de versiones (ej: Github). No hay configuración manual requerida.

Los webhooks son usados para disparar ejecuciones de flujos de trabajo. Cuando tu realizas un push a tu repositorio, abres un nuevo pull request, o creas una etiqueta, tu sistema de control de versiones automáticamente enviará un webhook a Drone que se encargará de disparar el flujo de trabajo.

<!-- # Recreate Webhooks

Drone provides the ability to recreate webhooks, in case they were accidentally removed or altered, using the command line utility.

```text
drone repo repair <repo>
drone repo repair octocat/hello-world
``` -->

# Saltarse Commits

Drone da la habilidad de saltarse commits individuales agregando `[CI SKIP]` al mensaje del commit. Nota que esto no filtra mayúsculas o minúsculas.

```diff
git commit -m "updated README [CI SKIP]"
```

# Saltarse ramas

Drone da la habilidad de saltarse commit basados en una rama de destino. El siguiente ejemplo se saltará un commit cuando la rama no sea master.

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

+branches: master
```

Ejemplo con múltiples ramas:

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

+branches: [ master, develop ]
```

Ejemplo usando globs

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

+branches: [ master, feature/* ]
```

Ejemplo incluyendo ramas:

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

+branches:
+  include: [ master, feature/* ]
```

Ejemplo excluyendo ramas:

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

+branches:
+  exclude: [ develop, feature/* ]
```
