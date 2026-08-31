<div align="center">

<img src="assets/workflow-diagram.png" alt="AR Prioritization and Underpayment Recovery Engine workflow" width="920" />

</div>

---

## Overview

Revenue Cycle teams need a practical way to identify claims that are likely underpaid and worth follow-up. This project builds a public-data proof of concept that estimates what a claim's payment should have been, flags claims that appear materially underpaid, ranks which underpaid claims AR teams should review first, and surfaces which states, HCPCS codes, and provider types drive the largest recovery opportunity.

No PHI or patient-level records are used — the pipeline runs entirely on public CMS Medicare and Physician Fee Schedule/RVU data.

---

## Key Features

- **Expected payment estimation** — builds expected-payment tables from CMS RVU reference data and joins them against actual CMS payment
- **Recovery-priority model** — a LightGBM classifier trained to flag high-recovery-priority claims, with threshold tuning for different precision/recall targets
- **AR priority workqueue** — scores and ranks every underpaid claim into Critical, High, Medium, and Standard tiers
- **Anomaly detection** — an Isolation Forest layer flags unusual underpayment patterns beyond the standard variance model
- **Regression validation** — a supplementary ML regressor cross-checks the CMS formula benchmark against actual allowed amounts
- **Live claim checker** — enter a claim's details and get an instant recovery-priority score with recommended action
- **FastAPI + React dashboard** — a live API backend with an interactive dashboard for the priority queue, underpayment reports, and claim checking

---

## Application Screenshots

<div align="center">

### AR Priority Queue
*Filterable, ranked queue of underpaid claims with recovery estimates and confidence scores.*

<img src="assets/priority-queue.png" alt="AR Priority Queue" width="720" />

<br /><br />

### Underpayment Report
*Recovery opportunity broken down by state, HCPCS code, provider type, and payer.*

<img src="assets/underpayment-report.png" alt="Underpayment Report" width="720" />

<br /><br />

### Claim Checker
*Enter a single claim's details and get an instant recovery-priority score and recommended action.*

<img src="assets/claim-checker.png" alt="Claim Checker" width="720" />

</div>

---

## Model Results

Best model: **LightGBM**

| Model | Mean PR-AUC | Mean ROC-AUC | Mean F1 |
|---|---:|---:|---:|
| LightGBM | 0.8688 | 0.8791 | 0.7479 |
| Hist Gradient Boosting | 0.8663 | 0.8771 | 0.7622 |
| Random Forest | 0.8561 | 0.8653 | 0.7449 |
| Gradient Boosting | 0.8546 | 0.8639 | 0.7284 |
| Logistic Regression | 0.6921 | 0.7078 | 0.6060 |

**Final holdout metrics:** Test PR-AUC 0.8754 · Test ROC-AUC 0.8857 · Test F1 0.7577, trained on 6,142,472 rows across 23 features.

**Regression validation** (supplementary cross-check against actual CMS allowed amounts):

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| HistGradientBoosting Regressor | 14.48 | 53.20 | 0.9268 |
| CMS Formula Benchmark | 28.07 | 99.38 | 0.7446 |
| Linear Regression | 29.34 | 109.83 | 0.6881 |

The CMS fee schedule formula remains the primary expected-payment benchmark because it's audit-defensible; the ML regressor is a supplementary validation layer, not the pricing engine.

---

## Recovery Opportunity Summary

| Metric | Value |
|---|---:|
| Underpaid queue rows | 6,056,133 |
| Estimated recovery | $14,993,979,972.64 |
| Critical tier rows | 9,114 |
| High tier rows | 173,218 |
| Top state by recovery | CA |
| Top HCPCS by recovery | 66984 |
| Top provider type | Diagnostic Radiology |

Some surgical-code rows are flagged for modifier review, since the public CMS PUF doesn't expose global surgery, bilateral, assistant-surgeon, or MPPR modifiers — client-facing totals should account for this.

---


