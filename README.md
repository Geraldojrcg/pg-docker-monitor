# dockprom

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [cAdvisor](https://github.com/google/cadvisor),
[NodeExporter](https://github.com/prometheus/node_exporter) and alerting with [AlertManager](https://github.com/prometheus/alertmanager).

**_If you're looking for the Docker Swarm version please go to [stefanprodan/swarmprom](https://github.com/stefanprodan/swarmprom)_**

## Install

Clone this repository on your Docker host, cd into dockprom directory and run compose up:

```bash
git clone https://github.com/stefanprodan/dockprom
cd dockprom

ADMIN_USER=admin ADMIN_PASSWORD=admin ADMIN_PASSWORD_HASH=JDJhJDE0JE91S1FrN0Z0VEsyWmhrQVpON1VzdHVLSDkyWHdsN0xNbEZYdnNIZm1pb2d1blg4Y09mL0ZP docker-compose up -d
```

**Caddy v2 does not accept plaintext passwords. It MUST be provided as a hash value. The above password hash corresponds to ADMIN_PASSWORD 'admin'. To know how to generate hash password, refer [Updating Caddy to v2](#Updating-Caddy-to-v2)**

Prerequisites:

- Docker Engine >= 1.13
- Docker Compose >= 1.11

## Updating Caddy to v2

Perform a `docker run --rm caddy caddy hash-password --plaintext 'ADMIN_PASSWORD'` in order to generate a hash for your new password.
ENSURE that you replace `ADMIN_PASSWORD` with new plain text password and `ADMIN_PASSWORD_HASH` with the hashed password references in [docker-compose.yml](./docker-compose.yml) for the caddy container.

Containers:

- Prometheus (metrics database) `http://<host-ip>:9090`
- Prometheus-Pushgateway (push acceptor for ephemeral and batch jobs) `http://<host-ip>:9091`
- AlertManager (alerts management) `http://<host-ip>:9093`
- Grafana (visualize metrics) `http://<host-ip>:3000`
- NodeExporter (host metrics collector)
- cAdvisor (containers metrics collector)
- Caddy (reverse proxy and basic auth provider for prometheus and alertmanager)
