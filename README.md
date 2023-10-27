traefik
---

Configure and run Traefik from docker image as systemd.service.


### Links
- <https://traefik.io>
- <https://github.com/traefik/traefik>
- <https://hub.docker.com/_/traefik/>


### Variables
- **`traefik_docker_image`** *(type=string, default=traefik:latest)* - Docker image for using into systemd.service.

- **`traefik_docker_network`** *(type=string, default="bridge")* - Connect a container to a network as `docker run --network=...`.
- **`traefik_docker_publish_ports`** *(type=list, default=[])* - List of strings for publish a container’s ports to the host as `docker run --publish=...`.

- **`traefik_docker_envs`** *(type=list, default=[])* - List of objects in format `{KEY: value}` for set environment variables as `docker run --env=...`.
- **`traefik_docker_labels`** *(type=list, default=[])* - List of objects in format `{key: value}` for set meta data on a container as `docker run --label=...`.

- **`traefik_uid`** & **`traefik_gid`** *(type=number, default=80)* - System user and group for run Treafik and store data.

- **`traefik_main_config`** *(type=string, mandatory)* - Multiline string with content for Traefik main config (`/etc/traefik/config.yml`).
- **`traefik_dynamic_config_files`** *(type=list, default=[])* - List of objects in format `{filename: ..., content: ...}`, where `filename` - filename in `/etc/traefik/config.d/` and `content` - multiline string with config file content.
- **`traefik_ssl_files`** *(type=list, default=[])* - List of objects in format `{filename: ..., content: ...}`, where `filename` - filename in `/etc/traefik/ssl/` and `content` - multiline string with certificate (certificate chain) or private key, needed for HTTPS.


### Examples
```yaml
traefik_docker_image: traefik:v2.10.5
traefik_docker_network: internal
traefik_docker_publish_ports: ["80:80", "443:443"]
traefik_docker_environments:
  LEGO_DISABLE_CNAME_SUPPORT: "true"
traefik_docker_labels:
  traefik.enable: "true"
  traefik.http.routers.traefik.service: "api@internal"
  traefik.http.routers.traefik.entrypoints: "http"
  traefik.http.routers.traefik.rule: "Host(`traefik.example.com`)"
  traefik.http.routers.traefik.middlewares: "local-ipwhitelist@file"
traefik_main_config: |
  global:
    checkNewVersion: false
    sendAnonymousUsage: false
  api: {}
  ping: {}
  providers:
    file:
      directory: /etc/traefik/config.d
      watch: true
    docker:
      endpoint: unix:///var/run/docker.sock
      exposedByDefault: false
      network: {{ traefik_docker_network }}
  entryPoints:
    http:
      address: :80
  metrics:
    prometheus: {}
  accessLog: {}
  log:
    level: ERROR
traefik_dynamic_config_files:
  - filename: http_middlewares.yml
    content: |
      http:
        middlewares:
          local-ipwhitelist:
            ipWhiteList:
              sourceRange: [127.0.0.0/8, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16]
```
