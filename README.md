# 🛡️ Enterprise DevSecOps Security Scanning Suite & SonarQube Toolkit

[![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=for-the-badge&logo=sonarqube&logoColor=white)](https://www.sonarqube.org/)
[![Gitleaks](https://img.shields.io/badge/Gitleaks-Secret_Detection-red?style=for-the-badge&logo=git&logoColor=white)](https://github.com/gitleaks/gitleaks)
[![Checkov](https://img.shields.io/badge/Checkov-IaC_Compliance-1976D2?style=for-the-badge&logo=bridgecrew&logoColor=white)](https://www.checkov.io/)
[![Aqua Trivy](https://img.shields.io/badge/Aqua_Trivy-Container_Scan-0078D4?style=for-the-badge&logo=aquasec&logoColor=white)](https://trivy.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

An enterprise-ready **DevSecOps Security Scanning Toolkit & Pipeline Library** providing automated, Shift-Left security controls for **SonarQube Quality Gates**, **Gitleaks Secret Detection**, **Checkov IaC Compliance**, and **Aqua Trivy Vulnerability Scanning**.

---

## 🏛️ Repository Architecture

```
devsecops-security-scanning-suite/
│
├── configs/                                  # Enterprise Security Configuration Files
│   ├── sonarqube/
│   │   └── sonar-project.properties          # Exclusions, code coverage & quality gate rules
│   ├── gitleaks/
│   │   └── .gitleaks.toml                    # Azure & GitHub custom secret regex rules
│   └── checkov/
│       └── .checkov.yaml                     # Frameworks (Terraform, Bicep, Helm, K8s)
│
├── azure-pipelines/                          # Azure DevOps Pipelines & Step Templates
│   ├── master-devsecops-pipeline.yml         # End-to-end 3-stage security pipeline
│   └── templates/
│       ├── sonarqube-pipeline.yml            # SonarQube Prepare, Analyze & Publish tasks
│       ├── gitleaks-pipeline.yml             # Gitleaks execution with report publishing
│       └── checkov-iac-pipeline.yml          # Checkov IaC compliance scanner
│
├── github-actions-workflows/                 # GitHub Actions Workflows
│   ├── sonarqube-analysis.yml                # SonarSource official action with Quality Gate check
│   ├── gitleaks-secret-scan.yml              # Gitleaks action
│   └── checkov-iac-scan.yml                  # Checkov SARIF upload action
│
├── docker-runners/                           # Local Developer Environment
│   └── docker-compose-sonarqube.yml          # Local SonarQube LTS + PostgreSQL containers
└── README.md
```

---

## 🔍 1. SonarQube & SonarCloud Integration

Never release code with critical bugs, security hotspots, or low code coverage!

### Running SonarQube Locally in 1 Command:
```bash
cd docker-runners
docker compose -f docker-compose-sonarqube.yml up -d
```
Access the SonarQube web UI at: **`http://localhost:9000`** (Default: `admin` / `admin`).

### Azure DevOps Service Connection Setup:
1. In Azure DevOps Project Settings ➔ **Service connections** ➔ **New service connection**.
2. Select **SonarQube** (or SonarCloud).
3. Name it: `SonarQube-Enterprise-ServiceConnection`.
4. Enter Server URL and authentication token.

---

## 🔑 2. Gitleaks Secret Scanning

Detects and blocks credentials before they ever leave developer machines or reach main branches:
- **Azure Storage Connection Strings** (`AccountKey=...`)
- **Azure Service Principal Client Secrets** (`client_secret = ...`)
- **GitHub Personal Access Tokens** (`ghp_...`)
- **Private RSA / TLS Keys**

Configured in [**`configs/gitleaks/.gitleaks.toml`**](./configs/gitleaks/.gitleaks.toml).

---

## 🏗️ 3. Checkov Infrastructure as Code (IaC) Scanning

Scans Terraform, Microsoft Bicep, ARM templates, and Helm charts against CIS Microsoft Azure Foundations Benchmarks:
- Enforces Storage Account public access disabled.
- Blocks open RDP/SSH (0.0.0.0/0) inbound rules.
- Enforces Key Vault purge protection & soft delete.
- Ensures AKS clusters use managed identity & Azure CNI.

Configured in [**`configs/checkov/.checkov.yaml`**](./configs/checkov/.checkov.yaml).

---

## 👤 Author

Developed by **Faraz Ahmed** ([@farazahmedy](https://github.com/farazahmedy)).
Feel free to fork, adopt these pipeline templates, or open an issue!
