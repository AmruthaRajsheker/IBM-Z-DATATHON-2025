# AutoGovernZ by Amrutha Rajsheker

# AutoGovern Z

Continuous, automated AI governance for financial services: real-time bias and drift monitoring, multi-framework compliance scoring, and immutable audit trails — proven in a working Python prototype and designed to scale on IBM Z with watsonx.governance.

## Table of Contents
- Overview
- Key Capabilities
- Architecture
- Quickstart
- Running the Notebook (14-cell guide)
- Configuration
- Outputs
- Tech Stack
- Datasets
- Roadmap (IBM Z + watsonx)
- Repository Structure
- Contributing
- License

## Overview
AutoGovern Z is an AI-for-AI governance engine that continuously evaluates machine learning systems for fairness, drift, and regulatory compliance, and records all governance actions in a cryptographically verifiable audit trail. The project ships with a self-contained Python notebook/script that generates realistic synthetic data, trains models, runs governance checks, and produces dashboards and reports without external dependencies.

## Key Capabilities
- Bias and Fairness: Demographic parity, equal opportunity, equalized odds across protected attributes.
- Drift and Stability: PSI and KS tests for feature and prediction drift with thresholded alerts.
- Compliance Automation: Scoring aligned to Basel III, GDPR, and EU AI Act, with summary dashboards.
- Immutable Audit: SHA-256 hash-chained audit log with chain integrity verification.
- Dashboards and Reports: Visual summaries and regulator-ready textual reports.
- Extensible Design: Clear interfaces to plug in real data sources, explainability, and enterprise systems.

## Architecture
- Data Layer: Synthetic or external tabular data for loan approvals (features, protected attributes, target).
- Analytics Layer: BiasDetector, DriftDetector, ComplianceMonitor components with shared utilities.
- Orchestration: AutoGovernZ class to register models, run assessments, aggregate results.
- Trust Layer: AuditTrail for tamper-evident governance events and integrity checks.
- Presentation: Matplotlib/Seaborn/Plotly dashboards and text reports.

## Quickstart
Prerequisites: Python 3.10+, pip

1) Clone and install
```bash
git clone <your-repo-url>
cd autogovernz
pip install -r requirements.txt
```

2) Run the notebook (recommended)
- Open AutoGovernZ.ipynb and run all cells top to bottom.
- Or run the script version if provided:
```bash
python autogovernz.py
```

3) Review outputs
- Visuals display inline (notebook) and may be saved under ./out/.
- Textual compliance summaries and audit logs are printed and can be exported.

## Running the Notebook (14-cell guide)
1. Installation & Imports: Sets up libraries and suppresses warnings for clean runs.
2. DataLoader: Generates synthetic loan approval data with intentional, controllable bias.
3. BiasDetector: Implements demographic parity, equal opportunity, equalized odds.
4. DriftDetector: PSI and KS tests; includes comparison plots and metric bars.
5. ComplianceMonitor: Basel III, GDPR, EU AI Act score computation and statuses.
6. AuditTrail: SHA-256 hash chain of events; integrity verification utilities.
7. AutoGovernZ Orchestrator: Wires all components and model registry.
8. Model Training: Trains RandomForest and GradientBoosting on synthetic data.
9. Governance Assessment: Executes bias, drift, and compliance; logs audit entries.
10. Bias Visualization: Plots fairness metrics across protected groups.
11. Drift Demonstration: Creates drifted data, computes PSI/KS, visualizes shift.
12. Compliance Dashboard: Framework scores, component breakdowns, risk matrix.
13. Audit Visualization: Chain verification and event timelines.
14. Final Dashboards & Reports: Consolidated view and regulator-ready summaries.

## Configuration
Governance thresholds and options (example YAML):
```yaml
governance:
  fairness:
    demographic_parity_warn: 0.10
    demographic_parity_crit: 0.20
  drift:
    psi_warn: 0.10
    psi_crit: 0.20
  compliance:
    basel_min: 70
    gdpr_min: 60
    eu_ai_act_min: 70
reporting:
  save_dir: "./out"
  export_text: true
```

## Outputs
- Bias: Metric tables and bar charts per protected attribute and model.
- Drift: Distribution overlays, PSI/KS summaries, and thresholds.
- Compliance: Scores and statuses (Basel III, GDPR, EU AI Act) with heatmaps.
- Audit: Hash chain verification, event timeline, and latest entries.
- Reports: Human-readable compliance summaries with recommendations.

## Tech Stack
- Python, pandas, NumPy, SciPy
- scikit-learn (RandomForest, GradientBoosting, metrics)
- Matplotlib, Seaborn, Plotly (interactive)
- hashlib (SHA-256), datetime, json

Note on prototype scope:
- SHAP/LIME are optional and installable; example hooks exist but may not be executed by default.
- Enterprise integrations (watsonx.governance, z/OS Connect, Kafka, IBM Streams, DB2) are roadmap items.

## Datasets
- Primary: Synthetic Loan Approval dataset generated in-notebook via DataLoader (no external file required).
- Optional: UCI German Credit via OpenML (for validation), if you choose to enable it.
- Drift Scenarios: Created by shifting income/credit score distributions to simulate economic changes.

## Roadmap (IBM Z + watsonx)
- Deploy governance engine on IBM Z for high availability, hardware crypto, and isolation.
- Integrate with watsonx.governance for model registry, policy packs, lineage, and approvals.
- Ingest real-time streams via Kafka/IBM Streams; persist in DB2; expose APIs via z/OS Connect.
- Automate EU AI Act conformity workflows and auditor evidence packs.

## Repository Structure
Example structure (single-notebook repo):
```
.
├── AutoGovernZ.ipynb           # End-to-end prototype (14 cells)
├── autogovernz.py              # Optional script version (if included)
├── requirements.txt
├── configs/
│   └── default.yaml            # Optional governance thresholds
├── assets/                     # Diagrams/screenshots
└── out/                        # Generated reports/figures (gitignored)
```

## Example Usage
Train and assess a model:
```python
# Generate data
df = DataLoader().create_synthetic_loan_data(5000)
X = df[['age','income','credit_score','loan_amount']]
y = df['approved']

# Train model
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
Xtr, Xte, ytr, yte = train_test_split(X, y, test_size=0.3, random_state=42)
model = RandomForestClassifier(n_estimators=100, random_state=42).fit(Xtr, ytr)

# Register and assess
gov = AutoGovernZ()
gov.register_model('credit_model', model, risk_level='high')
gov.comprehensive_governance_assessment('credit_model', Xte, yte, df)

# View results
gov.display_governance_dashboard('credit_model')
```


Notes:
- This prototype is for research and demonstration; adapt thresholds and criteria to institutional policies.
- When using real data, ensure privacy, legal, and policy compliance.
