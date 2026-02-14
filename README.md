# 🌍 Terraform Drift Watcher - Infrastructure Drift Detector

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg?logo=python&logoColor=white)  
![Terraform](https://img.shields.io/badge/Terraform-State-844FBA?logo=terraform)  
![AWS](https://img.shields.io/badge/Cloud-AWS-FF9900?logo=amazon-aws)  
![CI/CD](https://img.shields.io/badge/CI/CD-Drift_Check-2088FF?logo=githubactions)  

**Terraform Drift Watcher** is a **Python 3** tool that simulates detection of **infrastructure drift**:  
- Compares **Terraform state** (desired) with **cloud state** (live)  
- Detects **missing, changed, and extra** resources  
- Designed for **Google Colab / CI/CD** without needing Terraform CLI  

----------------------------------

## 🛠 Tech & Languages

| Layer        | Tech / Format   | Notes                               |
|--------------|-----------------|-------------------------------------|
| Language     | **Python 3.10+**| Stdlib only                         |
| Desired      | Terraform State | JSON export from `terraform show -json` |
| Live State   | JSON mock       | Simulates cloud provider state      |
| CI/CD        | GitHub Actions  | Run drift checks on PRs             |

-----

## 📦 Repository Structure

terraform-drift-watcher/
├─ tfstate.json # Desired state (Terraform)
├─ cloud.json # Simulated cloud state
├─ watcher.py # Drift detection script
└─ README.md

yaml
Copy code

---

## ▶️ Run in Google Colab

```python
from watcher import DriftWatcher

watcher = DriftWatcher("tfstate.json", "cloud.json")
print(watcher.check_drift())
📄 Input Examples
Terraform State (tfstate.json)
json
Copy code
{
  "resources": [
    {"name": "vm-1", "type": "aws_instance", "config": {"size": "t2.micro"}},
    {"name": "db-1", "type": "aws_rds", "config": {"engine": "postgres"}}
  ]
}
Cloud State (cloud.json)
json
Copy code
{
  "resources": [
    {"name": "vm-1", "type": "aws_instance", "config": {"size": "t2.medium"}},
    {"name": "lb-1", "type": "aws_lb", "config": {"type": "public"}}
  ]
}
🧪 Example Output
json
Copy code
{
  "missing": ["db-1"],
  "changed": ["vm-1"],
  "extra": ["lb-1"]
}
🐳 Docker
Build:

bash
Copy code
docker build -t drift-watcher:latest .
Run:

bash
Copy code
docker run --rm -v $(pwd):/work drift-watcher:latest \
  python watcher.py
☸️ Use Cases
Detect drift between Terraform and Cloud in CI pipelines

Alert teams before applying destructive changes

Run scheduled drift scans as part of platform monitoring

Teach Terraform + drift detection in Colab demos
------
👤 Author
Siddharth Raut — DevOps / Platform Engineer

📧 Email: siduk2500@gmail.com

💼 LinkedIn: linkedin.com/in/siddharth-raut-
