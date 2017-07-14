+++
date = "2017-04-15T14:39:04+02:00"
title = "Plugins"
url = "es/using-plugins"

[menu.usage]
  weight = 7
  identifier = "plugins-es"
  parent = "usage_concepts"
+++

Los plugins son contenedores Docker que realizan tareas predefinidas y están configurados como pasos en tu flujo de trabajo. Los plugins pueden ser usados para desplegar código, publicar artefactos, enviar notificaciones y mas.

Ejemplo usando el plugin de Docker y Slack:

```yaml
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

  publish:
    image: plugins/docker
    repo: foo/bar
    tags: latest

  notify:
    image: plugins/slack
    channel: dev
```

# Aislamiento de Plugins

Los plugins son ejecutados en contenedores de Docker y están aislados de otros pasos en tu flujo de trabajo. Los plugins comparten su espacio de construcción, que es montado como un volumen, y por eso tienen acceso al código fuente.

# Almacen de Plugins

Los plugins son empaquetados y distribuidos como contenedores de Docker. Ellos son conceptualmente similares a librerías de Software (como npm), y pueden ser publicados y compartidos por la comunidad. Puedes encontrar una lista de plugins en
[http://plugins.drone.io](http://plugins.drone.io).
