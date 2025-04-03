# 🖥️ Linux Monitoring Lab - Prometheus + Grafana

This project is a hands-on, self-hosted monitoring lab I built to simulate real-world Linux system monitoring. It showcases my ability to design, configure, and manage a multi-node monitoring stack using industry standard tools like **Prometheus**, **Grafana**, and **Node Exporter**.

It reflects the kind of work expected from a Linux System Administrator or DevOps Engineer, and was built entirely from the ground up in my own lab environment to reinforce RHCE-level skills.

---

## 📌 Why I Built This

- To simulate enterprise-grade Linux server monitoring across multiple hosts
- To gain real hands-on experience with Prometheus and Grafana
- To apply systemd management, service configuration, and custom labeling in Prometheus
- To practice structuring infrastructure for visibility, automation, and scalability
- To build a GitHub-ready portfolio project that reflects real sysadmin responsibilities

---

## 🧱 Technologies Used

- **Prometheus**: Metric collection from Linux systems.
- **Node Exporter**: Installed on each remote host to expose system-level metrics.
- **Grafana**: Visualization and dashboards.
- **RHEL/Rocky Linux 9**: As monitored nodes and control servers.
- **Systemd**: For managing exporter services.
- **SSH & SCP**: For remote configuration and data collection.

---

## 🧠 What I Did (Summary)

- Set up a Prometheus server to scrape metrics from multiple Linux VMs.
- Installed Node Exporter on each monitored VM and enabled it with systemd.
- Configured `prometheus.yml` and applied 'alias' labels. Allowing Grafana to display clean hostnames instead of IPs.
- Created and customized Grafana dashboards to track:
  - CPU usage
  - Memory usage
  - Disk usage
  - Disk I/O
  - Network traffic
- Exported the Grafana dashboard to a `.json` file and stored it in version control.
- Masked internal IPs in screenshots and config files for safe public sharing.
- Structured the project into clearly labeled folders (grafana_json, node_exporter, prometheus, screenshots).

---

## 🔐 Prometheus Configuration (Sanitized)
Note: IPs are masked intentionally to protect internal network details. In my lab, these were internal bridge IPs assigned to each VM.
[Prometheus Targets](screenshots/prometheus-targets.png)

```yaml
- job_name: "node_exporters"
  static_configs:
    - targets: ['192.x.x.x:9100']
      labels:
        alias: "rhel9-vm01"
    - targets: ['192.x.x.x:9100']
      labels:
        alias: "rocky-vm02"
    - targets: ['192..x.x:9100']
      labels:
        alias: "prolug"
```

## 📊 Grafana Dashboard

The dashboard displays:

- [CPU usage] (screenshots/CPU%20Usage)
- [Memory usage percent (total vs. available)] (screenshots/Memory%20Usage)
- [Disk usage per mount] (screenshots/Disk%20Usage)

**Dashboard JSON file:**  
[`grafana_json/grafana-system-health-dashboard.json`](grafana_json/grafana-system-health-dashboard.json)

To import in Grafana:

1. Go to **+ → Import**
2. Upload the JSON file from this repo
3. Select your Prometheus data source
4. Click **Import**

---


---

## 🚀 Next Steps


- Add Prometheus alert rules for CPU, memory, and disk thresholds.
- Set up Grafana notifications for Slack, email, or webhook alerts.
- Integrate Loki to centralize logs alongside metrics in Grafana.
- Explore container metrics using cAdvisor or node_exporter cgroups.
- Create an Ansible role to automate Node Exporter deployment.

These steps would further align the stack with what’s expected in enterprise-grade monitoring environments.

---

## 🧾 Project Summary

This project demonstrates my ability to:

- Design and implement a full Linux server monitoring stack from scratch.
- Monitor multiple remote hosts using Prometheus, Grafana and Node Exporter.
- Visualize system health clearly using Grafana dashboards.
- Apply best practices like `alias` labeling for clean observability.
- Sanitize, document, and structure a technical project for GitHub presentation.

It reflects real-world sysadmin skills and an understanding of open-source monitoring tools. Built entirely in a home lab, this project highlights my initiative, troubleshooting ability, and technical depth as a Linux System Administrator.


## 🔒 Additional Monitoring Ideas

Beyond core system metrics, here are a few additional things that could be monitored to improve system security and performance:

- Failed SSH login attempts (e.g., using Fail2Ban or journald parsing)
- High CPU or memory usage by specific processes
- Disk I/O latency or filesystem bottlenecks

The options are endless depending on the environment!
