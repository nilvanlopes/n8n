# n8n with Docker Swarm

This repository contains a Docker Swarm stack for [n8n](https://n8n.io/), a free and open-source workflow automation tool.

## Prerequisites

* Docker and Docker Swarm
* [Traefik](https://traefik.io/) as a reverse proxy
* A domain name for n8n

## Installation

1.  Fork and clone this repository:

    ```bash
    git clone https://github.com/your-username/n8n.git
    cd n8n
    ```

2.  Create a `.env` file from the example:

    ```bash
    cp .env.example .env
    ```

3.  Edit the `.env` file and fill in the required values.

4.  Deploy the stack:

    ```bash
    docker stack deploy -c docker-compose.yml n8n
    ```

## Usage

After deploying the stack, you can access the n8n UI at the domain you configured in the `docker-compose.yml` file (e.g., `https://n8n.your-domain.com`).

## Configuration

The following environment variables need to be set in the `.env` file:

| Variable                  | Description                                            |
| ------------------------- | ------------------------------------------------------ |
| `POSTGRES_DB`             | The name of the PostgreSQL database.                   |
| `POSTGRES_USER`           | The username for the PostgreSQL database.              |
| `POSTGRES_PASSWORD`       | The password for the PostgreSQL database.              |
| `N8N_HOST`                | The hostname of your n8n instance.                     |
| `N8N_PROTOCOL`            | The protocol to use (e.g., `https`).                   |
| `N8N_PORT`                | The port to use (e.g., `5678`).                        |
| `N8N_PROXY_HOPS`          | The number of proxy hops.                              |
| `WEBHOOK_URL`             | The webhook URL for your n8n instance.                 |
| `GENERIC_TIMEZONE`        | The timezone to use for your n8n instance.             |
| `DB_TYPE`                 | The database type (should be `postgresdb`).            |
| `DB_POSTGRESDB_HOST`      | The hostname of the PostgreSQL database.               |
| `DB_POSTGRESDB_PORT`      | The port of the PostgreSQL database.                   |
| `DB_POSTGRESDB_DATABASE`  | The name of the PostgreSQL database.                   |
| `DB_POSTGRESDB_USER`      | The username for the PostgreSQL database.              |
| `DB_POSTGRESDB_PASSWORD`  | The password for the PostgreSQL database.              |
| `EXECUTIONS_MODE`         | The executions mode for n8n (e.g., `queue`).           |
| `QUEUE_BULL_REDIS_HOST`   | The hostname of the Redis instance.                    |
| `QUEUE_BULL_REDIS_PORT`   | The port of the Redis instance.                        |

## Traefik Labels

The `docker-compose.yml` file includes labels for Traefik to automatically configure the reverse proxy. You will need to change the `Host` rule to match your domain.

```yaml
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.n8n.rule=Host(`n8n.your-domain.com`)"
        - "traefik.http.routers.n8n.entrypoints=websecure"
        - "traefik.http.routers.n8n.tls.certresolver=cloudflare"
        - "traefik.http.services.n8n.loadbalancer.server.port=5678"
        - "traefik.docker.network=traefik-public"
```
