+++
date = "2017-04-15T14:39:04+02:00"
title = "Setup with Caddy"
url = "setup-with-caddy"

[menu.install]
  identifier = "setup-with-caddy"
  parent = "install_server"
  weight = 4
+++

This guide provides a brief overview for installing Drone server behind the [Caddy webserver](https://caddyserver.com/). This is an example caddyfile proxy configuration:

```nohighlight
drone.mycomopany.com {
    proxy / localhost:8000 {
        websocket
        transparent
    }
}
```

You must configure the proxy to enable websocket upgrades:

```diff
drone.mycomopany.com {
    proxy / localhost:8000 {
+       websocket
        transparent
    }
}
```

You must configure the proxy to include `X-Forwarded` headers using the `transparent` directive:

```diff
drone.mycomopany.com {
    proxy / localhost:8000 {
        websocket
+       transparent
    }
}
```
