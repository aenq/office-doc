# Dashboard

Gets you notified when new versions of your Docker containers are available and lets you react the way you want.

#### WUD is built on 3 concepts:

> `WATCHERS` query your Docker hosts to get the containers to watch

> `REGISTRIES` query the Docker registries to find available updates

> `TRIGGERS` perform actions when updates are available

## Dashboard Office Interface

![image](dashboard.png)

# FAQ

## Core dumped on Raspberry PI

If at startup you face an issue looking like

```
#
# Fatal error in , line 0
# unreachable code
#
#
#
#FailureMessage Object: 0x7eace25c
```

Add the `--security-opt seccomp=unconfined` option to your docker command
Example

```
docker run ... --security-opt seccomp=unconfined fmartinou/whats-up-docker
```

# Watcher API

This API allows to query the state of the watchers.

?> Need to add a new Watcher?  
[Take a look at the documentation.](/watchers/)

## Get all Watchers

This operation lets you get all the configured watchers.

```bash
curl http://wud:3000/api/watchers

[
   {
      "id":"docker.local",
      "type":"docker",
      "name":"local",
      "configuration":{
         "socket":"/var/run/docker.sock",
         "port":2375,
         "cron":"0 * * * *",
         "watchbydefault":true
      }
   }
]
```

## Get a Watcher by id

This operation lets you get a specific Watcher.

```bash
curl http://wud:3000/api/watchers/docker.local

[
   {
      "id":"docker.local",
      "type":"docker",
      "name":"local",
      "configuration":{
         "socket":"/var/run/docker.sock",
         "port":2375,
         "cron":"0 * * * *",
         "watchbydefault":true
      }
   }
]
```

# Authentication

WUD allows `Anonymous` access by default.

You can enable 1 or multiple authentication strategies using `WUD_AUTH_*` env vars.

!> Please pay attention that all API routes & all UI views will be authenticated.

Currently, the following strategies are supported:

?> [**Basic**](configuration/authentications/basic/)

?> [**Openid Connect**](configuration/authentications/oidc/)

# Quick start

## Run the Docker image

The easiest way to start is to deploy the official _**WUD**_ image.

<!-- tabs:start -->

#### **Docker Compose**

```yaml
version: "3"

services:
  whatsupdocker:
    image: fmartinou/whats-up-docker
    container_name: wud
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - 3000:3000
```

#### **Docker**

```bash
docker run -d --name wud \
  -v "/var/run/docker.sock:/var/run/docker.sock" \
  -p 3000:3000 \
  fmartinou/whats-up-docker
```

<!-- tabs:end -->

?> Please notice that wud is available on multiple container registries \
\- Docker Hub: `fmartinou/whats-up-docker` \
\- Github Container Registry: `ghcr.io/fmartinou/whats-up-docker`

## Open the UI

[Open the UI](http://localhost:3000) in a browser and check that everything is working as expected.

## Add your first trigger

?> Everything ok? \
It's time to [**add some triggers**](configuration/triggers/)!

## Going deeper...

?> Need to fine configure how WUD must watch your containers? \
Take a look at the [**watcher documentation**](configuration/watchers/)!

?> Need to integrate other registries (ECR, GCR...)? \
Take a look at the [**registry documentation**](configuration/registries/).

## Ready-to-go examples

?> You can find here a **[complete configuration example](configuration/?id=complete-example)** illustrating some common WUD options.
