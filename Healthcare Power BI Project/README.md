# Healthcare Analytics Dashboard — Power BI

An end-to-end Power BI analytics project built on a simulated hospital network, covering patient encounters, treatment costs, insurance claims, and care-quality outcomes.

## 1. Project Overview

This project is a self-directed Power BI assignment built to demonstrate end-to-end analytics capability across a healthcare provider network — covering patient encounters, treatment costs, insurance claims, and care-quality outcomes. The goal was to design a clean star-schema data model, build a robust DAX measure library, and surface the metrics a hospital operations or finance leadership team would actually use to run the business.

The dashboard simulates the kind of reporting a Health System Analytics / BI team would deliver to Hospital Administrators, Revenue Cycle Management, and Care Quality stakeholders — tying clinical activity (encounters, diagnoses, length of stay) directly to financial performance (treatment cost, claims, reimbursement) and patient experience (satisfaction, readmissions).

## 2. Business Problem It Solves

Healthcare organizations sit on encounter-level data spread across patients, providers, hospitals, and claims — but leadership rarely has one place to see cost, quality, and claims performance together. This dashboard was built to answer questions such as:

- Which hospitals and providers are driving the highest treatment cost and patient volume?
- How efficient is the claims process — what share of claims are approved, denied, or still pending?
- Are readmissions and length of stay within acceptable ranges, and where are the outliers?
- How satisfied are patients (CSAT), and does that correlate with cost or readmission patterns?
- How is patient volume and cost trending over time, month over month and by quarter?

## 3. Data Model

The model follows a star schema — one fact table of patient encounters connected to four dimension tables, plus a dedicated date table for time intelligence.

| Table | Role | Contents |
|---|---|---|
| Fact_Patient_Encounters | Fact | One row per patient encounter: diagnosis & procedure codes, length of stay, treatment cost, insurance-covered amount, patient-pay amount, claim status, readmission flag, satisfaction score, admission/discharge dates |
| Dim_Patient | Dimension | Patient demographics — name, gender, age (with age bins/age groups), city, state, zip, chronic condition |
| Dim_Provider | Dimension | Provider name, specialty, years of experience |
| Dim_Hospital | Dimension | Hospital name, facility type, city, state, bed count |
| Calendar | Date table | Continuous date table with Year, Quarter, Month Name/No, Day Name, Year-Month and Week Number for time intelligence |

**Relationships:** the fact table connects Many-to-One to Hospital, Patient, and Provider on their respective ID keys, and to Calendar on Admission Date (active) — with a second, deliberately inactive relationship on Discharge Date so length-of-stay style analysis can use `USERELATIONSHIP` when needed. All relationships filter in a single direction from dimension to fact, keeping the model predictable and fast.

## 4. DAX Measures Built

A 16-measure library was built to support the report, grouped by theme:

**Volume & Scale**

| Measure | What it tells the business |
|---|---|
| Total Patient | Distinct count of patients treated across the network |
| Total Encounters | Total number of patient encounters recorded |
| Total Hospital | Number of hospitals in the network |
| Total Provider | Distinct count of treating providers |

**Cost & Financial Performance**

| Measure | What it tells the business |
|---|---|
| Total Treatment Cost | Sum of treatment cost across all encounters |
| Total Claim Amount | Treatment cost restricted to Approved claims only — the actual reimbursable spend |
| Avg Encounters Value | Average cost per encounter — a core unit-economics metric |
| Avg Cost per Hospital | Total treatment cost divided across the hospital network — spots high-cost facilities |

**Claims Efficiency**

| Measure | What it tells the business |
|---|---|
| Claim Approval Rate | % of encounters with an Approved claim status |
| Claim Denied Rate | % of encounters with a Denied claim status |
| Claim Pending Rate | Derived as 1 − (Approval Rate + Denied Rate) — claims still in process |

**Care Quality & Experience**

| Measure | What it tells the business |
|---|---|
| Readmission Rate | % of encounters flagged as a readmission — a key quality/cost indicator |
| Avg Stay Length | Average length of stay in days per encounter |
| Avg CSAT Score | Average patient satisfaction score per encounter |

**Network Efficiency (per-hospital ratios)**

| Measure | What it tells the business |
|---|---|
| Avg Patient per Hospital | Average patient load per facility — capacity/staffing signal |
| Avg Encounters per Hospital | Average encounter volume per facility — throughput signal |

## 5. Key Insights the Dashboard Surfaces

- **Claims performance at a glance** — a single, always-current view of claims health (approval, denial, pending rates) so revenue-cycle teams can spot bottlenecks before they hit cash flow.
- **Cost hot-spots by facility and specialty** — total and average treatment cost broken down by hospital, provider specialty, and diagnosis, making it easy to flag facilities or specialties running above the network average.
- **Quality vs. experience correlation** — readmission rate and average length of stay tracked side-by-side with satisfaction score, connecting clinical quality signals to the patient experience.
- **Trends over time** — patient volume, encounter counts, and cost trended by month/quarter using the Calendar table, supporting seasonality and growth analysis.
- **Hospital capacity utilization** — bed count, patient load, and encounter volume compared per hospital to highlight over- or under-utilized facilities.
- **Patient demographics & risk mix** — patient segmentation by age group and chronic condition to understand which populations drive the most encounters and cost.

## 6. Skills Demonstrated

- Star-schema data modeling with correct cardinality, cross-filter direction, and a dedicated date table
- Intermediate-to-advanced DAX: `CALCULATE` with `FILTER`, `DIVIDE` for safe ratios, measure branching (a measure built on top of other measures), and an inactive-relationship pattern for alternate date analysis
- KPI design translating raw transactional data into business-ready rates and averages (claims, readmissions, utilization)
- End-to-end thinking — from data model design through measure architecture to the business questions each metric answers

## Repository Contents

- `Dataset/` — source data used to build the model
- `Healthcare Analytics Report.pbix` — the Power BI report file
- `Healthcare Analytics Requriment Document.pdf` — the project's requirements/brief
