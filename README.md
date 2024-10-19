# Monitoring Stack with Prometheus, Alertmanager, Grafana, and Traefik

This project sets up a monitoring and alerting stack using **Prometheus**, **Alertmanager**, and **Grafana** with **Traefik** for reverse proxying. The stack is designed to monitor system resources such as CPU and memory usage, trigger alerts when certain thresholds are exceeded, and visualize the metrics on Grafana dashboards.

## Features

- **Prometheus**: Collects metrics and evaluates alerting rules.
- **Alertmanager**: Manages alerts triggered by Prometheus, sending notifications via email.
- **Grafana**: Visualizes metrics and allows dashboard creation.
- **Traefik**: Provides reverse proxying with automated SSL using Let's Encrypt.
- **Node Exporter**: Exposes machine metrics (CPU, memory, disk usage, etc.).

## Project Structure
```
├── alertmanager
│   └── alertmanager.yml
├── docker-compose.yaml
├── prometheus
│   ├── alert.rules.yml
│   └── prometheus.yml
└── README.md
```


## Prerequisites

- **Docker**: Ensure Docker is installed on your system.
- **Docker Compose**: Make sure Docker Compose is available.
- **.env File**: Create a `.env` file in the root directory and define the required environment variables.

### Example `.env` file

```bash
# SMTP Configuration
# SMTP Configuration
SMTP_SMARTHOST=
SMTP_FROM=
SMTP_AUTH_USERNAME=
SMTP_AUTH_PASSWORD=
SMTP_REQUIRE_TLS=

# Email Config
ALERT_EMAIL_TO=

# Traefik Domains
TRAEFIK_PROM_HOST=
TRAEFIK_ALERT_HOST=
TRAEFIK_GRAFANA_HOST=

# Grafana Admin Credentials
GRAFANA_ADMIN_USER=
GRAFANA_ADMIN_PASSWORD=

# SMTP Server Environment
RELAY_HOST=
RELAY_PORT=
RELAY_USERNAME=
RELAY_PASSWORD=
```

## Services
- **Prometheus**: Collects metrics from the Node Exporter and evaluates alerting rules.
- **Alertmanager**: Sends email notifications when alerts are triggered.
- **Grafana**: Provides dashboards for visualizing the metrics collected by Prometheus.
- **Node Exporter**: Collects resource metrics from the host machine.
- **Traefik**: Acts as a reverse proxy and manages SSL certificates via Let's Encrypt.

## Setup and Usage
### Clone the repository
```bash
git clone https://github.com/ghulammuzz/alert-monitoring-stack.git
cd your-repo-name
```
### Configure environment variables
Create a `.env` file based on the example provided above.

### Start the stack
Run the following command to start all services:

```bash
docker-compose up -d
```
This will start Prometheus, Alertmanager, Node Exporter, SMTP server, and Grafana, all connected through the Traefik reverse proxy.

### Access services
Prometheus: http://ur.svc
Alertmanager: http://alert.svc.com
Grafana: http://grafana.svc.com

### Set up Grafana
1. Open Grafana in your browser.

2. Log in with the credentials defined in the .env file (default: admin/admin).

3. Add Prometheus as a data source:

    - Go to Configuration -> Data Sources -> Add Data Source.
    - Choose Prometheus.
    - Set the URL to http://prometheus:9090.
4. Create or import dashboards to visualize metrics.

### Customize alerting rules
You can modify the alerting rules in prometheus/alert.rules.yml to monitor specific metrics and thresholds for your environment. You can modify the HTML template stored in the `alertmanager/templates/custom_email.tmpl` file to change the email content. Follow these steps to enable HTML email notifications:

Create the custom email template at `alertmanager/templates/custom_email.tmpl`. An example template could look like this:

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        body { font-family: Arial, sans-serif; }
        h2 { color: #D32F2F; }
        p { font-size: 14px; }
    </style>
</head>
<body>
    <h2>{{ .Status }}: {{ .CommonLabels.alertname }}</h2>
    <p><strong>Instance:</strong> {{ .CommonLabels.instance }}</p>
    <p><strong>Description:</strong> {{ .CommonAnnotations.description }}</p>
    <p><strong>Value:</strong> {{ .Value }}</p>
    <p>This alert was generated at {{ .StartsAt }}.</p>
</body>
</html>

```

### Email Notifications
Alertmanager is configured to send email notifications when alerts are triggered. Make sure the SMTP configuration in the .env file is correct for your mail server.

### Stopping the Stack
To stop all running services, run:

```bash
docker-compose down
```
This will stop and remove all containers associated with the stack.