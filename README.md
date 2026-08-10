# CloudPeak SaaS — Subscription & Churn Dashboard

A Power BI dashboard analyzing subscription revenue, customer churn, and account health for a fictional SaaS company (CloudPeak), built from a raw, intentionally messy dataset to simulate a real-world analytics workflow — from data cleaning through to a finished, decision-ready report.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-blue?style=flat)
![Status](https://img.shields.io/badge/status-complete-brightgreen)

---

## Project Overview

CloudPeak is a fictional SaaS company selling subscriptions across three plan tiers (Basic, Pro, Enterprise) to customers spread across four global regions. This project answers a core business question: **is the recurring revenue business healthy, and where is it most at risk?**

Rather than just reporting totals, the dashboard was built to uncover *why* those totals look the way they do — connecting churn behavior, plan tier, and top-account concentration into a single, actionable story.

## Business Questions Answered

- What is the company's actual current recurring revenue (MRR/ARR)?
- What percentage of customers churn, and does that rate change by plan tier?
- Which region generates the most revenue?
- Are the highest-paying customers concentrated in one plan tier — and does that create risk?

## Key Findings

| Metric | Result |
|---|---|
| Total MRR | ~$55,740/month |
| ARR (Annualized) | ~$668,840/year |
| Churn Rate | 28% |
| Active Customers | 289 |
| ARPA (Avg Revenue Per Account) | ~$192.86 |

- **Enterprise plan customers churn the most (33%)**, despite being the highest-value segment
- **All 10 of the top 10 highest-paying customers are on the Enterprise plan** — meaning the company's most valuable segment is also its most fragile
- Revenue is fairly evenly distributed across all 4 regions, with North America leading only modestly

This combination — highest value *and* highest churn risk concentrated in one plan — is the dashboard's central insight, and a genuine risk worth flagging to leadership.

## Dashboard Preview

The final report includes:
- 5 KPI cards (Total MRR, ARR, Churn Rate, Active Customers, ARPA)
- MRR by signup month (line chart)
- Churn rate by plan (bar chart)
- Total MRR by region (bar chart)
- Top 10 customers by MRR (table)
- Interactive slicers: Plan, Region, Status

*(Add a screenshot of your finished dashboard here — drag the exported PNG/PDF into this repo and reference it, e.g. `![Dashboard Screenshot](screenshots/dashboard.png)`)*

## Data Source

The dataset is a synthetically generated, intentionally messy export (`CloudPeak_SaaS_RAW.xlsx`) simulating real-world data quality issues commonly found in raw business exports:

- Inconsistent text formatting (e.g., "Basic" / "basic" / "BASIC")
- Region names mixed between acronyms and full names (e.g., "NA" / "North America")
- Numeric values stored as text with currency symbols (e.g., "$79.00", "79 USD/mo")
- Negative values representing refunds/credits
- Missing values in optional fields (Seats, Support Tickets, NPS Score)
- Duplicate and blank rows

## Tools & Skills Used

- **Power Query** — data cleaning, standardizing text, fixing data types, handling nulls and duplicates
- **DAX** — CALCULATE, DIVIDE, SUMMARIZE, ADDCOLUMNS, FIRSTNONBLANK, time intelligence
- **Data Modeling** — star schema design (fact + dimension tables), relationship management
- **Power BI Desktop** — report design, interactive slicers, visual formatting

## Data Model

**Fact table:** `Subscriptions` — one row per customer account
**Dimension tables:**
- `Plans` — unique plan tiers, connected 1-to-many to Subscriptions
- `Calendar` — a generated date table (not extracted from raw data) ensuring a complete, unbroken date range for accurate time-based analysis

```
Plans (1) ──────< Subscriptions >────── (1) Calendar
```

## Key DAX Measures

```dax
Total MRR = 
CALCULATE(SUM('Subscriptions'[MRR]), 'Subscriptions'[Status] = "Active")

Churn Rate = 
DIVIDE([Churned Customers], [Total Customers], 0)

ARR (Annualized) = 
[Total MRR] * 12

ARPA = 
DIVIDE([Total MRR], [Active Customers], 0)
```

**Note on Total MRR:** deliberately filtered to `Status = "Active"` only — a churned customer's historical MRR value still exists in the raw data, and summing it without this filter would overstate current recurring revenue. This is a common real-world SaaS reporting mistake that this measure specifically avoids.

## Data Cleaning Highlights

- Standardized inconsistent Plan and Region text values into clean categories
- Converted MRR from mixed text/currency format into a usable Decimal Number
- Preserved meaningful blank values (Seats, Support Tickets, NPS Score) rather than fabricating placeholder data, keeping `AVERAGE()`-based measures accurate
- Investigated and flagged negative MRR values (refunds/credits) rather than deleting them, preserving an accurate total
- Removed exact duplicate rows and blank rows using a verified, independently-checked process

## Repository Contents

```
├── CloudPeak_SaaS_RAW.xlsx        # Raw, messy source dataset
├── CloudPeak_SaaS_Dashboard.pbix  # Power BI report file
├── CloudPeak_SaaS_Dashboard.pdf   # Static PDF export of the dashboard
├── screenshots/                   # Dashboard preview images
└── README.md                      # This file
```

## How to Use

1. Clone or download this repository
2. Open `CloudPeak_SaaS_Dashboard.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
3. Use the slicers (Plan, Region, Status) to explore the data interactively
4. Refresh the data connection if using the raw `.xlsx` file with updated data

## About This Project

This project was built as part of a portfolio demonstrating end-to-end BI analyst skills: taking raw, imperfect data through cleaning, modeling, calculation, and visualization to a finished, decision-ready dashboard — mirroring the kind of ambiguous, real-world data a Data/BI Analyst encounters on the job.

---

**Author:** Aryan Mantrawadi
**Connect:** [LinkedIn](#) · [Portfolio](#)
