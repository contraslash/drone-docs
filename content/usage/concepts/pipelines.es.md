+++
date = "2017-04-15T14:39:04+02:00"
title = "Flujos de trabajo"
url = "es/pipelines"

[menu.usage]
  weight = 4
  identifier = "pipelines-es"
  parent = "usage_concepts"
+++

La sección de flujo de trabajo define una lista de pasos de construcción, pruebas y despliegue de tu código. Los pasos de la línea de trabajo son ejecutados secuencialmente en el orden en que son definidos. Si un paso retorna un código de salida distinto a 0, el flujo de trabajo inmediatamente aborta y retorna el estado de fallo.

Ejemplo de flujo de trabajo:

```yaml
pipeline:
  backend:
    image: golang
    commands:
      - go build
      - go test
  frontend:
    image: node
    commands:
      - npm install
      - npm run test
      - npm run build
```

En el ejemplo anterior, nosotros definimos dos pasos, `frontend` y `backend`. Los nombres de estos pasos son completamente arbitrarios.

# Pasos de construcción

Los pasos de construcción de tu flujo son pasos que ejecutan arbitrariamente comandos dentro de un contenedor docker. Los comandos son ejecutados usando el espacio de trabajo como el directorio de trabajo.

```diff
pipeline:
  backend:
    image: golang
    commands:
+     - go build
+     - go test
```

No hay magia aquí. Los comandos anteriores son convertidos en un script de línea de comandos. Los comandos escritos anteriormente con convertidos en el siguiente script:

```diff
#!/bin/sh
set -e

go get
go build
go test
```

El script anterior es ejecutado como el `entrypoint` de docker. El siguiente comando docker  es un ejemplo (incompleto) de como el script es ejecutado:

```
docker run --entrypoint=build.sh golang
```

{{% alert info %}}
Por favor note que únicamente los pasos de construcción pueden definir comandos. No puedes definir comandos con plugins o servicios.
{{% /alert %}}

# Ejecución en paralelo

Drone soporta la ejecución de pasos en paralelo, para ejecuciones en la misma máquina. Los pasos paralelos son configurados utilizando el atributo `group`. Esto le indica al ejecutor del flujo de trabajo el nombre del grupo en paralelo.

Ejemplo de configuración en paralelo:

```diff
pipeline:
  backend:
+   group: build
    image: golang
    commands:
      - go build
      - go test
  frontend:
+   group: build
    image: node
    commands:
      - npm install
      - npm run test
      - npm run build
  publish:
    image: plugins/docker
    repo: octocat/hello-world
```

En el ejemplo anterior, los pasos `frontend` y `backend` son ejecutados en paralelo. El ejecutor de flujo de trabajos no va a ejecutar el paso `publish` hasta que el grupo termine.

# Ejecución condicional

Drone provee la habilidad de limitar condicionalmente los pasos de compilación en tiempo de ejecución. El siguiente ejemplo limita la ejecución del plugin Slack basado en una rama:

```diff
pipeline:
  slack:
    image: plugins/slack
    channel: dev
+   when:
+     branch: master
```

# Ejecución fallida

Drone usa el código de salida del contenedor para determinar el éxito o fallo de una compilación. Las salidas que no son 0 hacen que el flujo termine y finalice inmediatamente.

Existen algunos casos de uso para ejecutar pasos del flujo en fallos, como enviar notificaciones de compilaciones fallidas. Usa la propiedad `status` para sobre escribir el comportamiento por defecto y ejecutar pasos aunque el estado de compilación sea fallido:

```diff
pipeline:
  slack:
    image: plugins/slack
    channel: dev
+   when:
+     status: [ success, failure ]
```
