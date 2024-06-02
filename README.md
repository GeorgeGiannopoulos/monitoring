## Monitor Server and Application

### What can you use Monitoring for?

**Monitoring** is a containerized application that provides a comprehensive monitoring solution for your systems. It leverages several open-source tools working together to `collect`, `visualize`, `store`, and `alert` on various system metrics and logs. Here's a breakdown of what **Monitoring** can do for you:

- **System Monitoring**: Gain insights into critical system health aspects like `CPU`, `memory`, `disk usage`, and `network performance` through **Node Exporter**.
- **Container Monitoring**: Monitor containerized workloads using **cAdvisor** to track `CPU`, `memory`, and `network utilization` of individual containers.
- **Log Aggregation and Analysis**: Collect logs from various sources, including `Docker containers`, and store them in **Loki** for centralized analysis and troubleshooting.
- **Data Visualization**: Visualize collected metrics using **Grafana**'s interactive dashboards. Create custom dashboards to monitor specific system components or applications.
- **External Service Monitoring**: Extend your monitoring capabilities beyond internal application metrics with **blackbox-exporter**. Probe endpoints using HTTP, HTTPS to monitor factors like response time, availability, and status codes.
- **Alerting**: Set up alerts in **Prometheus** to notify you when critical system metrics exceed predefined thresholds. **Alertmanager** groups and routes these alerts to configured notification channels (`email`, `Slack`, etc.).

Overall, **Monitoring** empowers you to proactively monitor your systems' health, identify potential issues early on, and ensure smooth operation of your applications.

### How to

This **Monitoring** application consists of several services working together to collect, visualize, and alert on system metrics:

#### Visualization

**Grafana**: provides a web interface for visualizing metrics collected by Prometheus. You can create dashboards to monitor various system health aspects.

Navigate to [https://<domain-name>/grafana](https://<domain-name>/grafana) or [http://localhost:3000](http://localhost:3000)

#### Data Collectors

**Prometheus**: scrapes metrics from various sources (like Node Exporter) and stores them for later retrieval and analysis.

Navigate to [http://localhost:9090](http://localhost:9090)

**Loki**: ingests and stores logs from various sources, allowing you to search and analyze them.

Navigate to [http://localhost:3100](http://localhost:3100)

#### System Metrics Exporters

**Node Exporter**: exposes system metrics like CPU, memory, disk usage, and more, which Prometheus can scrape.

Navigate to [http://localhost:9100](http://localhost:9100)

**cAdvisor**: collects container-specific metrics like CPU, memory, and network usage, which Prometheus can scrape.

Navigate to [http://localhost:8080](http://localhost:8080)

**Promtail**: collects logs from various sources (like Docker containers) and sends them to Loki for storage.

#### Multi-Target Exporters

**blackbox-exporter**: probe endpoints using HTTP, HTTPS to monitor factors like response time, availability, and status codes.

Navigate to [http://localhost:9115/metrics](http://localhost:9115/metrics)

#### Alerting

**Alertmanager**: receives alerts from Prometheus, groups them based on rules, and sends notifications through configured

Navigate to [http://localhost:9093/#/alerts](http://localhost:9093/#/alerts)

---

**NOTE**: For security reason these consoles are locked and not accessible from outside the server network.

To connect to them, a port tunneling must established by running the following command to a terminal:

```bash
ssh -L 3000:localhost:3000 \
    -L 9090:localhost:9090 \
    -L 3100:localhost:3100 \
    -L 9100:localhost:9100 \
    -L 8080:localhost:8080 \
    -L 9093:localhost:9093 \
    -L 9115:localhost:9115 \
    <username>@<server-ip>
```

# How to Configure

### Grafana

Change the following in **./grafana/config.ini**

- `instance_name`: Add a name for your grafana instance
- `domain`: Add servers domain name
- `admin_user`: Admin username to access grafana's UI
- `admin_password`: Admin password to access grafana's UI

Change the following in **./grafana/dashboards/container_logs.json**

- Replace `addherepattern` with a pattern of docker names to monitor

### Blackbox-Exporter

Change the following in **./prometheus/prometheus.yml**

- `google.com`: Add a list with the domains to probe

### AlertManager

Change the following in **./alertmanager/config.ini**

- `slack_api_url`: Add slack channel's hook token to send notifications
- `to`: Add a list with emails to send notifications
- `from`: Add email to send notifications
- `auth_username`: Add email username to send notifications
- `auth_password`: Add email password to send notifications
