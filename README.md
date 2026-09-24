<div align="center">

<img src="assets/workflow-diagram.png" alt="AR Prioritization and Underpayment Recovery Engine workflow" width="920" />

</div>

---
## Live Demo
<p align="center">
  <a href="https://ar-prioritization-underpayment-reco.vercel.app/">
    <strong>Live Demo</strong>
  </a>
</p>
---

## Overview

Revenue Cycle teams need a practical way to identify claims that are likely underpaid and worth follow-up. This project builds a public-data proof of concept that estimates what a claim's payment should have been, flags claims that appear materially underpaid, ranks which underpaid claims AR teams should review first, and surfaces which states, HCPCS codes, and provider types drive the largest recovery opportunity.

No PHI or patient-level records are used — the pipeline runs entirely on public CMS Medicare and Physician Fee Schedule/RVU data.

---



## Aim

- Estimate what a claim's payment should have been using CMS RVU reference data as an audit-defensible benchmark
- Flag claims that appear materially underpaid relative to that expected payment
- Rank underpaid claims so AR teams know which ones to review first
- Surface which states, HCPCS codes, and provider types drive the largest recovery opportunity
- Give AR teams an instant, claim-level recovery-priority score they can act on directly

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



## Application Dashbaord

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
## Benefit

- **Recovery effort is prioritized, not scattered** — Critical/High/Medium/Standard tiers point AR teams at the claims worth chasing first instead of reviewing underpayments in submission order
- **Findings are audit-defensible** — expected payment is anchored to the CMS fee schedule formula, so flagged underpayments can be justified to payers and stakeholders, not just to a model
- **Recovery opportunity is actionable by segment** — breakdowns by state, HCPCS code, and provider type let teams target the largest sources of lost revenue instead of working claims one at a time
- **Anomalies aren't missed** — the Isolation Forest layer catches unusual underpayment patterns that a standard variance threshold alone would overlook
- **Instant answers on individual claims** — the live claim checker gives a recovery-priority score and recommended action on demand, without waiting for a batch run

---
