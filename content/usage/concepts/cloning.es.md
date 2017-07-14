+++
date = "2017-04-15T14:39:04+02:00"
title = "Clonando"
url = "es/cloning"

[menu.usage]
  weight = 3
  identifier = "cloning-es"
  parent = "usage_concepts"
+++

Drone automáticamente configura un paso de clonado si no está específicamente definida. Tu puedes configurar manualmente el paso de clonado en tu flujo de trabajo para personalización:

```diff
+clone:
+  git:
+    image: plugins/git

pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test
```

Ejemplo de configuración para sobre escribir la profuncidad:

```diff
clone:
  git:
    image: plugins/git
+   depth: 50
```

Ejemplo de configuración para usar un plugin de clonado personalizado:

```diff
clone:
  git:
+   image: octocat/custom-git-plugin
```
