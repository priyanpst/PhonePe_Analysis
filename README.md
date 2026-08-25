# PhonePe_Analysis
# 💳 PhonePe Payment Insights Dashboard

An interactive Power BI dashboard analyzing PhonePe-style digital payment transactions — built to uncover patterns in transaction volume, value, user behavior, and payment success/failure, with **DAX-driven dynamic insights** that update live as filters change.

![PhonePe Dashboard Preview](phonepe_dashboard_preview.png)

---

## 📌 Overview

This project simulates and analyzes **~300K digital payment transactions** across **108K unique users**, covering FY spend patterns, service categories, age demographics, and payment outcomes. The goal was to go beyond static charts and build a dashboard that *narrates* its own insights — so a stakeholder can filter by month or payment status and immediately read what changed, not just see it.

**Live KPIs at a glance:**
- 💰 Total Transaction Value: **₹3.47bn**
- 🔄 Total Transactions: **300K**
- 👥 Unique Users: **108K**
- ✅ Successful Rate: **96.00%**

---

## 🚀 Key Features

- **Executive KPI cards** — Total Transactions, Total Value, Unique Users, and Success Rate, each with live Month-over-Month growth (%)
- **Transactions Over Time** — dual-axis trend line comparing transaction count vs. transaction value across the year
- **Age Segment Contribution** — donut chart breaking down transaction share by Gen X, Millennials, Gen Z, and Boomers
- **Service Transaction Value Analysis** — bar chart comparing Loans, Insurance, Money Transfer, and Recharge/Bills by value
- **Top 5 Users by Transaction Value** — quick view into highest-spending users
- **Weekday vs. Weekend Usage** — donut chart splitting transaction share by day type
- **Dynamic, slicer-aware insight panel** — a natural-language summary (e.g. *"Loans service gives the highest transaction value"*) generated live from a DAX measure via Power BI's Insert Data Value feature, not hardcoded text
- **Interactive slicers** — filter the entire report by **Month** and **Payment Status** (Failed / Pending / Successful); every KPI, chart, and insight sentence recalculates instantly

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Data Visualization | Power BI Desktop |
| Data Modeling | Power BI Data Model (star-schema relationship) |
| Calculations | DAX (CALCULATE, DIVIDE, DATEADD, WEEKDAY, COUNT, DISTINCTCOUNT) |
| Data Prep | Power Query |

---

## 🧩 Data Model

| Table | Role |
|---|---|
| `All_Transactions` | Fact table — Transaction_ID, Amount, Service, Service Type, Payment_Status, Date |
| `All_Users` | Dimension — User_ID, Age, Age Segment, Join_Date |
| `Date_Table` | Calendar table for time-intelligence measures |
| `Measures (2)` | Central table holding all DAX calculations |

**Relationship:** `All_Transactions[User_ID]` → `All_Users[User_ID]` (many-to-one)

### Key DAX measures

```dax
Total Transaction Value = SUM(All_Transactions[Amount])

Total Transaction = COUNT(All_Transactions[Transaction_ID])

Successful Transaction =
    CALCULATE([Total Transaction], All_Transactions[Payment_Status] = "Successful")

Success Rate = DIVIDE([Successful Transaction], [Total Transaction])

Total Users = DISTINCTCOUNT(All_Users[User_ID])

Trans Value MOM% =
    DIVIDE([Total Transaction Value] - [Trans Value PM], [Trans Value PM])
```

---

## 💡 How the Dynamic Insights Work

Instead of hardcoding summary text, the insight panel pulls its sentence directly from live measures:

1. Every KPI is a DAX measure, so it inherits whatever filter context is active on the page.
2. Selecting **Failed**, **Pending**, or **Successful** on the Payment Status slicer filters `Payment_Status` before every measure evaluates — so KPI cards and the insight text recompute for that slice only.
3. The insight sentence itself uses Power BI's **Insert Data Value** feature to embed a live measure result (e.g. the top-performing service by transaction value) directly inside the sentence — so the narrative changes with the filter, not just the numbers behind it.

---

## 📂 Repository Structure

```
├── PhonePe_Analysis.pbix              # Power BI report file
├── phonepe_dashboard_preview.png      # Dashboard screenshot
└── README.md
```

---

## ▶️ How to Use

1. Clone or download this repository
2. Open `PhonePe_Analysis.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
3. Use the **Month** and **Payment Status** slicers at the top to explore how KPIs and insights change

---

## 📬 Connect

If you have feedback or ideas for extending this dashboard, feel free to open an issue or reach out — always happy to discuss data storytelling and Power BI techniques.
