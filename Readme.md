# 📊 MallHive Monitoring & Observability

This repository powers the observability stack for the **MallHive e-commerce platform**. It brings together infrastructure, dashboards, alerting, and telemetry to ensure visibility into system health, user behavior, and performance.

---

## 🔍 What This Repo Covers

- Prometheus & Grafana setup (infra + dashboards)  
- CloudWatch alarms for AWS-native services  
- Optional Datadog monitors  
- Application log forwarding & traces (via Fluent Bit / OpenTelemetry)  
- Terraform modules per environment  
- Automation scripts for CI/CD  

---

## 🛠️ Tech Stack

| Area      | Tools Used                                  |
|-----------|----------------------------------------------|
| Metrics   | Prometheus, CloudWatch, Datadog (optional)   |
| Dashboards| Grafana (JSON-as-code)                      |
| Logging   | Fluent Bit → CloudWatch Logs                |
| Alerting  | Prometheus rules, CloudWatch alarms         |
| Tracing   | OpenTelemetry                               |
| CI/CD     | GitHub Actions + Terraform + Scripts        |

---

## 📁 Folder Overview

| Folder         | Description                                                       |
|----------------|-------------------------------------------------------------------|
| `terraform/`   | Infra modules for Prometheus, Grafana, Datadog, CloudWatch        |
| `k8s/`         | Raw manifests for observability stack (if not using Helm)         |
| `scripts/`     | Shell scripts for plan, apply, dashboard import, validation       |
| `.github/`     | GitHub Actions CI workflow                                        |
| `README.md`    | You’re reading it                                                 |

---

## ⚙️ Deploying Infra

Provision observability components per stack:

```bash
bash scripts/plan.sh grafana
bash scripts/apply.sh grafana
````

Or for Prometheus:

```bash
cd terraform/prometheus
terraform init
terraform apply
```

---

## 📈 Dashboards

JSON dashboards live in `terraform/grafana/dashboards/`
Each service has a dashboard: `user-auth.json`, `orders.json`, etc.

Use the import script to push them to Grafana:

```bash
bash scripts/import-dashboards.sh
```

Set your secrets first:

```bash
export GRAFANA_API_KEY=xxxxx
export GRAFANA_HOST=https://grafana.mallhive.io
```

---

## 🚨 Alerts

* **Prometheus rules**: `terraform/alerts/prometheus-rules.yaml`
* **CloudWatch alarms**: `terraform/cloudwatch/alarms.tf`
* **Datadog (optional)**: `terraform/datadog/monitors.tf`

All rules follow environment-specific thresholds (e.g. dev, staging, prod).

---

## 🔁 CI/CD: GitHub Actions

GitHub workflow: `.github/workflows/monitoring-ci.yml`

**What it does:**

* Validates Terraform and alert rules
* Plans infra changes (Grafana, Prometheus, etc.)
* Imports updated dashboards to Grafana
* Lints dashboard JSON and Prometheus alert YAML

**Trigger:** On PR or push to main

---

## 🧪 Scripts

| Script                 | Function                               |
| ---------------------- | -------------------------------------- |
| `plan.sh`              | Run terraform plan for any module      |
| `apply.sh`             | Run terraform apply for approved envs  |
| `import-dashboards.sh` | Push JSON dashboards via API           |
| `validate-json.sh`     | Check dashboard formatting             |
| `validate-alerts.sh`   | Check syntax of Prometheus alert rules |

---

## 🔐 Secrets Required

Set these in your GitHub repo:

| Secret                  | Description                  |
| ----------------------- | ---------------------------- |
| `GRAFANA_API_KEY`       | For importing dashboards     |
| `AWS_ACCESS_KEY_ID`     | For deploying infra          |
| `AWS_SECRET_ACCESS_KEY` | With limited IAM permissions |

---

## 🧩 Environments

Infra is deployed per env using `-var="env=dev"` or workspace-based segregation:

```bash
terraform workspace select dev
terraform apply
```

---

## 👥 Contributing

Observability is team-owned. You can:

* Add new dashboards (`terraform/grafana/dashboards/`)
* Tune or test alert rules (`terraform/alerts/`)
* Help improve tracing coverage (via OpenTelemetry)
* Improve workflows in `.github/workflows/`

Run local tests:

```bash
bash scripts/validate-json.sh
bash scripts/validate-alerts.sh
terraform fmt
```

---

## 📜 License

This repo follows the MallHive platform license (see root `LICENSE.md` or your organization’s open source policy).

Questions? Hit up `#mallhive-devops` or drop a PR. We’re always improving our signal-to-noise ratio.

---

Let me know if you want:

* ✅ Starter Terraform code inside `grafana/` or `prometheus/`
* ✅ Example dashboards for `user-auth`, `orders`, or infra nodes
* ✅ Actual `monitoring-ci.yml` file ready to run

Happy to scaffold all that next.

```

Let me know if you want this saved as a downloadable `.md` file or if you'd like help adding badges or visuals.
```
