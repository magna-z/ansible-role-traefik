traefik
---

Configure and run Traefik from docker image as systemd.service.

### Links:
- <https://traefik.io>
- <https://github.com/traefik/traefik>
- <https://hub.docker.com/_/traefik/>


### Variables:
- **`traefik_docker_image`** *(type=string, default=traefik:latest)* - Docker image for run as systemd.service.
- **`traefik_docker_environments`** *(type=list, default=[])*
- **`traefik_docker_labels`** *(type=list, default=[])*

- **`traefik_docker_network`** *(type=string, default="bridge")*
- **`treafik_docker_publish_ports`** *(type=list, default=[])*

- **`traefik_uid`**, **`traefik_gid`** *(type=number, default=80)* - System user and group for run Treafik and store data.

- **`traefik_main_config`:** *(type=string, default="")*
- **`traefik_dynamic_config_files`** *(type=list, default=[])*
- **`traefik_ssl_files`** *(type=list, default=[])*
