# Spoolman & Monitoring Stack

This project provides a Docker Compose setup for running Spoolman and OpenSpoolman, along with a monitoring stack (Prometheus, Grafana, Node Exporter) and Caddy as a reverse proxy with local HTTPS support.

## Services

- **Spoolman**: Filament inventory management system.
- **OpenSpoolman**: Open-source spool management.
- **Prometheus**: Monitoring system and time series database.
- **Grafana**: Observability and data visualization platform.
- **Node Exporter**: Exporter for hardware and OS metrics.
- **Caddy**: Web server and reverse proxy (handles SSL/TLS).

## Prerequisites

- Docker
- Docker Compose

## Setup

1.  **Configure Environment Variables**

    Copy the example environment file to `.env`:

    ```bash
    cp .env.example .env
    ```

    Edit `.env` and set the following variables:
    - `DATA_DIR`: The absolute path on your host machine where data will be stored.
    - `PRINTER_ACCESS_CODE`: Access code for your 3D printer (for OpenSpoolman).
    - `PRINTER_IP`: IP address of your 3D printer.
    - `GRAFANA_PASSWORD`: Admin password for Grafana.

2.  **Start the Stack**

    Run the following command to start all services:

    ```bash
    docker-compose up -d
    ```

## Accessing Services

The services are exposed via Caddy with local domains. You may need to add these domains to your local `hosts` file pointing to the machine running Docker (e.g., `127.0.0.1` or the server's LAN IP).

- **Spoolman**: [https://spoolman.local](https://spoolman.local)
- **OpenSpoolman**: [https://openspoolman.local](https://openspoolman.local)
- **Prometheus**: [https://prometheus.local](https://prometheus.local)
- **Grafana**: [https://grafana.local](https://grafana.local)

## Configuration

- **Prometheus**: Configured in `prometheus.yml`. Node Exporter metrics are scraped every 5 minutes.
- **Caddy**: Configured in `Caddyfile`. Handles local certificates and reverse proxying.
