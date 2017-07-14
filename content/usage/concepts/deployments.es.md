+++
date = "2017-04-15T14:39:04+02:00"
title = "Despliegues"
url = "es/deployments"

[menu.usage]
  weight = 8
  identifier = "deployment-es"
  parent = "usage_concepts"
+++

Drone provee la habilidad de disparar despliegues. Cuando disparas un despliegue, tu flujo de trabajo es ejecutado con un evento de tipo `deployment`. Puedes usar este tipo de evento y ambiente objetivo para limiar la ejecución:

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

  publish:
    image: plugins/docker
    registry: registry.heroku.com
    repo: registry.heroku.com/my-staging-app/web
    when:
+     event: deployment
+     environment: staging

  publish_to_prod:
    image: plugins/docker
    registry: registry.heroku.com
    repo: registry.heroku.com/my-production-app/web
    when:
+     event: deployment
+     environment: production
```

El ejemplo anterior muestra como podemos configurar pasos del flujo de trabajo para que solo se ejecuten cuando se tenga un ambiente objetivo.

# Disparadores de despliegues

Los despliegues son disparados desde la utilidad de linea de comandos. Ellos son disparados desde una compilación existente. Esto es conceptualmente similar a promover compilaciones.

```text
drone deploy <repo> <build> <environment>
```

Promover la compilación especificada a tu ambiente de staging:

```text
drone deploy octocat/hello-world 24 staging
```

Promover la compilación especificada a tu ambiente de producción:

```text
drone deploy octocat/hello-world 24 production
```
