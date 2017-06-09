+++
date = "2017-04-15T14:39:04+02:00"
title = "Configurar Caddy"
url = "es/setup-with-caddy"

[menu.install]
  identifier = "setup-with-caddy-es"
  parent = "install_server"
  weight = 4
+++

Esta guía provee una breve introducción para instalar el servidor Drone detrás de un servidor web [Caddy](https://caddyserver.com/). Este puede ser un ejemplo de configuración del CaddyFile:

```nohighlight
drone.mycomopany.com {
    proxy / localhost:8000 {
        websocket
        transparent
    }
}
```

Debes configurar el proxy para habilitar el paso de websockets a conexión segura:

```diff
drone.mycomopany.com {
    proxy / localhost:8000 {
+       websocket
        transparent
    }
}
```

Debes configurar el proxy para incluir los encabezados `X-Forwarded` usando la directiva `transparent`:

```diff
drone.mycomopany.com {
    proxy / localhost:8000 {
        websocket
+       transparent
    }
}
```
