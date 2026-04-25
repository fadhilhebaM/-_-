# 🎓 Summer Training Assignment System
### نظام توزيع التدريب الصيفي — كلية التميز، جامعة بغداد 2026


## 📌 Overview

A fully interactive, standalone web application for assigning **112 students** across **5 academic departments** to summer training institutions across Iraq. The system uses **geographic distance calculation** (Haversine formula) to automatically suggest the nearest training institution to each student's governorate, while allowing manual overrides.

Built as a **single HTML file** — no server, no database, no installation required.

---

## ✨ Features

### 🤖 Smart Auto-Assignment
- Calculates real geographic distance between each student's governorate and all registered training institutions
- Uses the **Haversine formula** for accurate great-circle distance computation
- Automatically assigns each student to the **nearest institution**
- Distance displayed in kilometers with color indicators:
  - 🟢 Green — under 50 km
  - 🟡 Amber — 50–250 km
  - 🔴 Red — over 250 km

### ✏️ Manual Override
- Click the **تغيير (Change)** button next to any student to manually reassign them
- Manual assignments are visually highlighted in amber
- Option to revert any student back to the auto-assigned institution
- Full **change history log** with timestamps and undo support

### 📊 Four Interactive Tabs
| Tab | Description |
|-----|-------------|
| 📋 جدول التوزيع | Full assignment table with filters, search, and edit buttons |
| 📊 ملخص الجهات | Summary cards per institution with mini bar charts by department |
| ⚙️ إدارة الجهات | Add or remove training institutions dynamically |
| 🕓 سجل التعديلات | Audit log of all manual changes with undo capability |

### 🔍 Filtering & Search
- Filter by **academic department**
- Filter by **governorate**
- Live search by **student name** or **institution name**

### ⬇️ Excel Export
- Exports a multi-sheet `.xlsx` file using **SheetJS**:
  - **Sheet 1** — Full assignment table (with manual override indicator)
  - **Sheet 2** — Summary per institution (count, boarding students)
- The linked Excel version uses `COUNTIF` / `COUNTIFS` formulas so Sheets 2 & 3 update automatically when Sheet 1 is edited

---

## 🗂️ Files

```
├── توزيع_التدريب_الصيفي_2026.html        ← Main interactive app (open in any browser)
├── توزيع_التدريب_الصيفي_2026_مرتبط.xlsx  ← Linked Excel (formulas auto-update across sheets)
└── README.md                              ← This file
```

---


## 📐 Data & Algorithm

### Students
- **112 students** across 5 departments at the College of Excellence (كلية التميز), University of Baghdad
- Departments: نظم المعلومات التطبيقية · علم البيانات · إدارة الأعمال · محاسبة ومصارف · فلسفة وعلم الاجتماع
- Student data includes: name, department, governorate, boarding status

### Training Institutions (default: 22)
Includes ministries, state companies, and governorate offices across Iraq:

| City | Institutions |
|------|-------------|
| Baghdad بغداد | Ministry of Planning, ICT Ministry, Finance, Trade, Asiacell, Zain, Rafidain Bank, CMC |
| Basra البصرة | South Oil Company, Port of Basra |
| Others | Governorate offices for: Dhi Qar, Najaf, Diwaniyah, Karbala, Babel, Wasit, Diyala, Salah al-Din, Kirkuk, Anbar, Sulaymaniyah, Nineveh |

### Distance Calculation
```
Haversine Formula:
a = sin²(Δlat/2) + cos(lat1) × cos(lat2) × sin²(Δlon/2)
d = 2R × atan2(√a, √(1−a))     where R = 6,371 km
```
Each student is matched to the institution with the **minimum Haversine distance** from their governorate's centroid coordinates.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| HTML5 / CSS3 | UI structure and styling |
| Vanilla JavaScript (ES6+) | All logic, state management, rendering |
| [SheetJS (xlsx)](https://sheetjs.com/) | Excel file export (loaded via CDN) |
| [Google Fonts — Cairo](https://fonts.google.com/specimen/Cairo) | Arabic typography |
| Haversine formula | Geographic distance computation |

> **No frameworks. No build tools. No dependencies to install.**
> The only external resources are loaded from CDN (SheetJS + Google Fonts) and work offline if cached.

---

## 🏗️ Project Structure (inside the HTML)

```
DATA LAYER
├── GOV_COORDS      — Lat/Lon for each Iraqi governorate
├── STUDENTS[]      — 112 student records
└── ministries[]    — Training institutions (mutable at runtime)

LOGIC LAYER
├── hav()           — Haversine distance function
├── nearest()       — Find closest institution for a governorate
├── getAssigned()   — Compute full assignment list (auto + overrides)
└── getSummary()    — Group students by institution

STATE
├── tab             — Active tab
├── fDept/fGov      — Active filters
├── search          — Search string
├── overrides{}     — Manual assignments {studentId: ministryName}
├── history[]       — Change log entries
└── modal           — Currently open student modal

UI LAYER
└── render()        — Single-function full re-render (no virtual DOM)
```

---

## 🎨 Design System

| Token | Value | Usage |
|-------|-------|-------|
| `--navy` | `#1E2761` | Primary, headers |
| `--teal` | `#0EA5A0` | Actions, highlights |
| `--amber` | `#EF9F27` | Warnings, manual overrides |
| `--green` | `#3DB370` | Success, short distances |
| `--coral` | `#E8593C` | Alerts, long distances |

---

## 📋 Excel File — Formula Linking

The companion Excel file `توزيع_التدريب_الصيفي_2026_مرتبط.xlsx` contains:

- **Sheet 1 (جدول التوزيع)** — Master data. Column F (الجهة) is the editable source of truth.
- **Sheet 2 (ملخص الجهات)** — Uses `COUNTIF` and `COUNTIFS` formulas referencing Sheet 1. Updates automatically.
- **Sheet 3 (قائمة الجهات)** — Uses `COUNTIF` referencing Sheet 1. Updates automatically.

Editing any institution name in **Sheet 1, Column F** will instantly reflect in Sheets 2 and 3 — no manual refresh needed.

---

## 👩‍💼 About

Developed for **Dr. Heba Mohammed Fadhil**  
Assistant Professor, Data Science Department  
College of Excellence (كلية التميز) — University of Baghdad  
Academic Year 2025–2026

---

## 📄 License

This project is intended for **academic and administrative use** within the College of Excellence, University of Baghdad.  
Not licensed for commercial redistribution.
