# 🖥️ Linux Monitoring Lab - Prometheus + Grafana

This project is a hands-on, self-hosted monitoring lab I built to simulate real-world Linux system monitoring. It showcases my ability to design, configure, and manage a multi-node monitoring stack using industry standard tools like **Prometheus**, **Grafana**, and **Node Exporter**.

It reflects the kind of work expected from a Linux System Administrator and was built entirely from the ground up in my own lab environment to reinforce RHCE-level skills.

---

## Why I Built This

- To simulate enterprise-grade Linux server monitoring across multiple hosts
- To gain real hands-on experience with Prometheus and Grafana
- To apply systemd management, service configuration, and custom labeling in Prometheus
- To practice structuring infrastructure for visibility, automation, and scalability
- To build a GitHub-ready portfolio project that reflects real sysadmin responsibilities

---

## Technologies Used

- **Prometheus**: Metric collection from Linux systems.
- **Node Exporter**: Installed on each remote host to expose system-level metrics.
- **Grafana**: Visualization and dashboards.
- **Systemd**: For managing exporter services.
- **SSH & SCP**: For remote configuration and data collection.

---

## What I Did (Summary)

- Manually set up a Prometheus server to scrape metrics from multiple Linux VMs. Automation comes later
- Manually installed Node Exporter on each monitored VM and enabled it with systemd. Automation comes later.
- Configured `prometheus.yml` and applied 'alias' labels. Allowing Grafana to display clean hostnames instead of IPs.
- Created and customized Grafana dashboards to track:
  - CPU usage
  - Memory usage
  - Disk usage
  - Disk I/O
  - Network traffic
- Exported the Grafana dashboard to a `.json` file and stored it in version control.

---

## Prometheus Configuration (Sanitized)
Note: IPs are masked intentionally to protect internal network details. In my lab, these were internal bridge IPs assigned to each VM.
[Prometheus Targets](screenshots/prometheus-targets.png)

Below is the manual configuration of the job: node_exporters
```yaml
- job_name: "node_exporters"
  static_configs:
    - targets: ['192.168.x.x:9100']
      labels:
        alias: "rhel9-vm01"
    - targets: ['192.168.x.x:9100']
      labels:
        alias: "rocky-vm02"
    - targets: ['192.168.x.x:9100']
      labels:
        alias: "prolug"
```

## Service Management with systemd

In the manual setup phase, Node Exporter was configured to run as a persistent background service using custom `systemd` unit files. These files were written and stored in the [`/node_exporter/`](node_exporter/) directory and deployed to each remote Linux VM. This involved:

- Creating a dedicated `node_exporter` system user
- Writing a systemd unit file at `/etc/systemd/system/node_exporter.service`
- Reloading the `systemd` daemon
- Enabling the service to start on boot
- Starting the service with `systemctl`

Next you will see how these steps were handled via Ansible and Ansiblei Galaxy roles. 

---
## Ansible and Galaxy Roles

Two Ansible playbooks were created to demonstrate how the manual setup process can be fully automated using the official [`prometheus.prometheus`](https://galaxy.ansible.com/prometheus/prometheus) collection from Ansible Galaxy.


To install the required collection to the default collections path, run:

```bash
ansible-galaxy collection install -r ansible/requirements.yml
```
If you are not using a requirements file:

```bash
ansible-galxy collection install prometheus.prometheus
```


How the requirements.yml file should look:
```yaml
collections:
  - name: prometheus.prometheus
```
**File:** [`ansible/requirements.yml`](ansible/requirements.yml)


## View the playbooks and configs:

- [install_node_exporter.yml](ansible/playbooks/install_node_exporter.yml)
- [install_prometheus.yml](ansible/playbooks/install_prometheus.yml)
- [node_exporter.yml (vars)](ansible/vars/node_exporter.yml)
- [prometheus.yml (vars)](ansible/vars/prometheus.yml)

---


## 📊 Grafana Dashboard

The dashboard displays:

- [CPU usage] (screenshots/cpu_usage.png)
- [Memory usage percent] (screenshots/memory_usage.png)
- [Disk usage per mount] (screenshots/disk_usage.png)

**Dashboard JSON file:**  
[`grafana_json/grafana-system-health-dashboard.json`](grafana_json/grafana-system-health-dashboard.json)

To import in Grafana:

1. Go to **+ → Import**
2. Upload the JSON file from this repo
3. Select your Prometheus data source
4. Click **Import**

---

## Project Summary

This project demonstrates my ability to:

- Design and implement a full Linux server monitoring stack from scratch.
- Use Ansible to automate the deployment of Prometheus and Node Exporter.
- Monitor multiple remote hosts using monitoring tools.
- Visualize system health using Grafana dashboards.
- Apply best practices like `alias` labeling for clean observability.
- Sanitize, document, and structure a technical project for GitHub presentation.

I built this project as part of RHCE-level training and to demonstrate to future employers the kind of structured, maintainable systems I can deploy and manage.

---
## Next Steps
- Add Prometheus alert rules for CPU, memory, and disk thresholds.
- Set up Grafana notifications for email, or webhook alerts.


## Possible Additions

Beyond core system metrics, here are a few additional things that could be monitored to improve system security and performance:

- Failed SSH login attempts (e.g., using Fail2Ban or journald parsing)
- High CPU or memory usage by specific processes
- Disk I/O latency or filesystem bottlenecks

