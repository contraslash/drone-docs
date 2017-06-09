+++
date = "2017-04-15T14:39:04+02:00"
title = "Configurar Apache"
url = "es/setup-with-apache"

[menu.install]
  identifier = "setup-with-apache-es"
  parent = "install_server"
  weight = 5
+++

Esta guía provee una pequeña vista para instalar el servidor Drone detrás de un servidor web Apache2. Este es un ejemplo de configuración:

```nohighlight
ProxyPreserveHost On

RequestHeader set X-Forwarded-Proto "https"

ProxyPass /ws/ ws://localhost:8000/ws/
ProxyPassReverse /ws/ ws://localhost:8000/ws/

ProxyPass / http://127.0.0.1:8000/
ProxyPassReverse / http://127.0.0.1:8000/
```

Debes tener los siguientes módulos de Apache instalados:

```nohighlight
a2enmod proxy
a2enmod proxy_http
a2enmod proxy_wstunnel
```

Debes configurar Apache para definir `X-Forwarded-Proto` cuando usas https.

```diff
ProxyPreserveHost On

+RequestHeader set X-Forwarded-Proto "https"

ProxyPass /ws/ ws://localhost:8000/ws/
ProxyPassReverse /ws/ ws://localhost:8000/ws/

ProxyPass / http://127.0.0.1:8000/
ProxyPassReverse / http://127.0.0.1:8000/
```

Debes configurar apache para habilitar el paso a conexión segura.

```diff
ProxyPreserveHost On

RequestHeader set X-Forwarded-Proto "https"

+ProxyPass /ws/ ws://localhost:8000/ws/
+ProxyPassReverse /ws/ ws://localhost:8000/ws/

ProxyPass / http://127.0.0.1:8000/
ProxyPassReverse / http://127.0.0.1:8000/
```
