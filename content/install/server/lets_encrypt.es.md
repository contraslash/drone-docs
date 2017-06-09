+++
date = "2017-04-15T14:39:04+02:00"
title = "Configurar Lets Encrypt"
url = "es/configure-lets-encrypt"

[menu.install]
  identifier = "configure-lets-encrypt-es"
  parent = "install_server"
  weight = 8
+++

Drone soporta configuración automática de SSQL y actualizaciones usando let's encrypt. Puedes habilitar let's encrypt realizando las siguientes configuraciones en la configuración de tu servidor:

```diff
services:
  drone-server:
    image: drone/drone:{{% version %}}
    ports:
+     - 80:80
+     - 443:443
    volumes:
      - /var/lib/drone:/var/lib/drone/
    restart: always
    environment:
      - DRONE_OPEN=true
      - DRONE_HOST=${DRONE_HOST}
      - DRONE_GITHUB=true
      - DRONE_GITHUB_CLIENT=${DRONE_GITHUB_CLIENT}
      - DRONE_GITHUB_SECRET=${DRONE_GITHUB_SECRET}
      - DRONE_SECRET=${DRONE_SECRET}
+     - DRONE_LETS_ENCRYPT=true
```

Nota que drone usa el nombre de anfitrión de la variable de ambiente `DRONE_HOST`. Por ejemplo si la variable es `DRONE_HOST=https://foo.com` el certificado será solicitado para `foo.com`

<!-- Una vez habilitado puedes visitar tu sitio usando http y https. No hay planes inmediatos de redireccionar desde http a https, pero lo estaremos considerando para un futuro lanzamiento.-->

# Cache de certificados

Drone escribe los certificados en el siguiente directorio:

```
/var/lib/drone/golang-autocert
```

# Actualizaciones de certificados

Drone usa la librería oficial de Acme para Go para manejar las actualizaciones de certificados. No es necesaria configuración adicional o manejo requerido.
