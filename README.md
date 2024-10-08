# **Monitoring Project with Prometheus & Node Exporter**
This project demonstrates a cloud-based monitoring setup on an AWS EC2 instance using **Prometheus** and **Node Exporter** to track and visualize essential system metrics. It provides an example of effective DevOps monitoring, showcasing practical skills in setting up a monitoring solution, interpreting metrics, and optimizing for system health.

## **Table of Contents**
1. [Introduction](#introduction)
2. [Architecture Overview](#architecture-overview)
3. [Setup Instructions](#setup-instructions)
4. [Metrics Tracked](#metrics-tracked)
5. [Visualization](#visualization)
6. [Relevance to DevOps](#relevance-to-devops)
7. [Future Improvements](#future-improvements)

---

## **Introduction**
In a DevOps role, monitoring is crucial for maintaining system performance, ensuring uptime, and identifying issues before they impact users. This project leverages **Prometheus** for data collection and **Node Exporter** for system metrics to illustrate the power of real-time monitoring in a cloud environment. The skills demonstrated here are essential for any DevOps engineer, providing the foundation for building scalable and resilient systems.

## **Architecture Overview**
This setup captures **key system metrics** from an EC2 instance, streams them to Prometheus, and prepares the data for visualization. Below is an overview of the project architecture:

```plaintext
                +---------------------+
                |      EC2 Instance   |
                |  (Node Exporter)    |
                +---------------------+
                         |
                         | (Metrics via HTTP)
                         |
                +---------------------+
                |     Prometheus      |
                |     (Data Store)    |
                +---------------------+
                         |
                         | (Optional: Grafana for Visualization)
                         |
                +---------------------+
                |      Dashboard      |
                +---------------------+
```

- **Node Exporter**: Installed on the EC2 instance to expose system-level metrics.
- **Prometheus**: Collects and stores metrics from Node Exporter for querying and visualization.
- **Grafana (Optional)**: Used to create a more user-friendly visualization layer on top of Prometheus.

## **Setup Instructions**
To replicate this project, follow these steps to set up **Prometheus** and **Node Exporter** on an EC2 instance.

### **Step 1: Launch an EC2 Instance**
1. Launch a new EC2 instance using Amazon Linux 2 or Ubuntu.
2. Open ports **9090** (Prometheus) and **9100** (Node Exporter) in your security group settings.

### **Step 2: Install Prometheus**
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.54.1/prometheus-2.54.1.linux-amd64.tar.gz
tar xvfz prometheus-2.54.1.linux-amd64.tar.gz
sudo mv prometheus-2.54.1.linux-amd64 /etc/prometheus
sudo ln -s /etc/prometheus/prometheus /usr/local/bin/prometheus
```

### **Step 3: Install Node Exporter**
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.4.0/node_exporter-1.4.0.linux-amd64.tar.gz
tar xvfz node_exporter-1.4.0.linux-amd64.tar.gz
sudo mv node_exporter-1.4.0.linux-amd64/node_exporter /usr/local/bin
```

### **Step 4: Configure Prometheus**
Update the Prometheus configuration to include Node Exporter:
```yaml
- job_name: 'node_exporter'
  static_configs:
    - targets: ['localhost:9100']
```
Save your changes, and start Prometheus and Node Exporter:
```bash
nohup /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml &
nohup /usr/local/bin/node_exporter &
```

## **Metrics Tracked**
The setup captures a variety of system-level metrics essential for monitoring health and performance, including:
- **CPU Usage**: `node_cpu_seconds_total` - Monitors time spent in user, system, and idle states.
- **Memory Availability**: `node_memory_MemAvailable_bytes` - Tracks available memory to help prevent OOM (out-of-memory) errors.
- **Disk Usage**: `node_filesystem_avail_bytes` - Measures available disk space to avoid capacity issues.

Each metric is vital for identifying bottlenecks and ensuring system stability.

## **Visualization**
For a more detailed analysis, **Grafana** can be installed to visualize metrics from Prometheus. This setup allows you to create custom dashboards that display CPU usage, memory utilization, and disk availability in real-time. Below is an example of a basic Grafana dashboard configuration:

```plaintext
Dashboard Panels:
- CPU Usage (Line Graph)
- Memory Usage (Gauge)
- Disk Usage (Bar Chart)
```

**Example Query in Prometheus**:
```promql
rate(node_cpu_seconds_total{mode="idle"}[5m])
```

## **Relevance to DevOps**
This project reflects a fundamental aspect of DevOps: **monitoring and observability**. By setting up a monitoring system, you gain valuable insights into system performance, allowing for:
- **Proactive Issue Resolution**: Alerts and real-time metrics can notify you before issues escalate.
- **Resource Optimization**: Monitor and analyze resource utilization to identify performance improvements.
- **Scalability**: Set up this solution across multiple instances, laying the foundation for a fully scalable monitoring system.

These skills are directly applicable to roles focused on **system reliability**, **performance optimization**, and **cloud infrastructure management**.

## **Future Improvements**
Moving forward, this project can be expanded to include:
- **Additional Exporters**: Add cAdvisor for container metrics or the Blackbox Exporter for endpoint monitoring.
- **Alertmanager**: Configure alerts based on threshold violations, such as high CPU or memory usage.
- **Automated Scaling**: Integrate with AWS Auto Scaling to adjust resources based on Prometheus metrics.

---

This project serves as a hands-on demonstration of how to set up and maintain a monitoring system within an AWS cloud environment, emphasizing best practices and key DevOps principles.
