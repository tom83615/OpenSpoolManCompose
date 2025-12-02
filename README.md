# Spoolman & Monitoring Stack

This project provides a Docker Compose setup for running Spoolman and multiple instances of OpenSpoolman (for multiple printers), along with a monitoring stack (Prometheus, Grafana, Node Exporter) and Caddy as a reverse proxy with local HTTPS support.

## Services

- **Spoolman**: Filament inventory management system (Shared Database).
- **OpenSpoolman**: Open-source spool management (Multiple Instances).
- **Prometheus**: Monitoring system and time series database.
- **Grafana**: Observability and data visualization platform.
- **Node Exporter**: Exporter for hardware and OS metrics.
- **Caddy**: Web server and reverse proxy (handles SSL/TLS and Static Portal).

## Prerequisites

- Docker
- Docker Compose

## Setup

1.  **Configure Global Environment Variables**

    Copy the example environment file to `.env`:

    ```bash
    cp .env.example .env
    ```

    Edit `.env` and set the following variables:
    - `DATA_DIR`: The absolute path on your host machine where data will be stored.
    - `GRAFANA_PASSWORD`: Admin password for Grafana.

2.  **Configure Printer Instances**

    This setup supports multiple printers. Configure each printer's environment file:

    - **Printer 1**: Edit `.env.printer1`
    - **Printer 2**: Edit `.env.printer2`

    For each file, set:
    - `PRINTER_ID`: Unique ID for the printer.
    - `PRINTER_NAME`: Display name (shown on the portal).
    - `PRINTER_ACCESS_CODE`: Access code for your 3D printer.
    - `PRINTER_IP`: IP address of your 3D printer.
    - `OPENSPOOLMAN_BASE_URL`: The local URL for this instance (e.g., `https://openspoolman1.local/`).

3.  **Start the Stack**

    Run the following command to start all services:

    ```bash
    docker-compose up -d
    ```

## Accessing Services

The services are exposed via Caddy with local domains. You need to add these domains to your local `hosts` file pointing to the machine running Docker (e.g., `127.0.0.1` or the server's LAN IP).

### Main Portal
- **OpenSpoolman Portal**: [https://openspoolman.local](https://openspoolman.local)
  - This static page allows you to select which printer instance to manage.
  - It dynamically loads printer names from your `.env.printer` files.
  - It preserves URL paths, so deep links work (e.g., `/spool` -> `/spool` on the specific instance).

### Individual Instances
- **Printer 1**: [https://openspoolman1.local](https://openspoolman1.local)
- **Printer 2**: [https://openspoolman2.local](https://openspoolman2.local)

### Other Services
- **Spoolman**: [https://spoolman.local](https://spoolman.local)
- **Prometheus**: [https://prometheus.local](https://prometheus.local)
- **Grafana**: [https://grafana.local](https://grafana.local)

## Configuration

- **Caddy**: Configured in `Caddyfile`.
  - Handles local certificates.
  - Serves the static portal at `openspoolman.local`.
  - Proxies requests to `openspoolman_1` and `openspoolman_2`.
  - Uses Caddy templates to inject environment variables into the static portal.
- **Prometheus**: Configured in `prometheus.yml`.
