+++
date = "2017-04-15T14:39:04+02:00"
title = "Configurar Nginx"
url = "es/setup-with-nginx"

[menu.install]
  identifier = "setup-with-nginx-es"
  parent = "install_server"
  weight = 3
+++

Esta guía provee una introducción breve para la instalación del servidor Drone detrás de un servidor web NginX. Para una configuración mas avanzada, por favor consulte la [documentación oficial](https://www.nginx.com/resources/admin-guide/) de NginX.

Ejemplo de configuración:

```nginx
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80;
    server_name drone.example.com;

    location / {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;

        proxy_pass http://127.0.0.1:8000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_buffering off;

        chunked_transfer_encoding off;
    }

    location ~* /ws {
        proxy_pass http://127.0.0.1:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 86400;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;
    }
}
```

Debes declara la configuración de las cabeceras proxy a `X-Forwarded`:

```diff
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80;
    server_name drone.example.com;

    location / {
+       proxy_set_header X-Forwarded-For $remote_addr;
+       proxy_set_header X-Forwarded-Proto $scheme;

        proxy_pass http://127.0.0.1:8000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_buffering off;

        chunked_transfer_encoding off;
    }

    location ~* /ws {
        proxy_pass http://127.0.0.1:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 86400;
+       proxy_set_header X-Forwarded-For $remote_addr;
+       proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Debes configurar NginX para actualizar a conexión segura de websockets.

```diff
+map $http_upgrade $connection_upgrade {
+    default upgrade;
+    ''      close;
+}

server {
    listen 80;
    server_name drone.example.com;

    location / {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_pass http://127.0.0.1:8000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_buffering off;

        chunked_transfer_encoding off;
    }

    location ~* /ws {
        proxy_pass http://127.0.0.1:8000;
        proxy_http_version 1.1;
+       proxy_set_header Upgrade $http_upgrade;
+       proxy_set_header Connection "upgrade";
        proxy_read_timeout 86400;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
