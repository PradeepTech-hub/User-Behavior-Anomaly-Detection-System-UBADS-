# 🛡️ User Behavior Anomaly Detection System (UBADS)

> A research-oriented cybersecurity platform for collecting **real, authorized Windows security/activity logs**, engineering behavioral features, and preparing the foundation for **machine-learning-based user behavior anomaly detection**.

UBADS is designed to identify unusual user activity that may indicate **insider threats, compromised accounts, privilege misuse, or abnormal behavioral patterns**.

The system follows a strict **real-data-first** approach: genuine authorized Windows activity is collected and stored locally, while synthetic data is restricted exclusively to an isolated **Demo Lab** and is never mixed with the real research dataset.

---

## 📌 Project Overview

Modern organizations generate large volumes of authentication, file, process, and device activity. Manually analyzing this activity can make it difficult to identify unusual behavior early.

**UBADS** provides a foundation for a User and Entity Behavior Analytics (UEBA) system by:

- Collecting authorized Windows security/activity events
- Normalizing and validating collected events
- Removing duplicate events
- Storing real activity locally in SQLite
- Classifying human and non-human Windows accounts
- Engineering behavioral features
- Measuring whether enough real data exists for ML
- Providing a FastAPI backend
- Visualizing activity through a React SOC dashboard
- Providing an isolated synthetic Demo Lab
- Preparing the architecture for future anomaly detection
- Preparing for future multi-endpoint ingestion and Azure deployment

The project is intentionally developed in phases so that every stage is verified before moving to the next one.

---

# 🎯 Objectives

The main objectives of UBADS are:

1. Collect real Windows security/activity logs from authorized computers.
2. Normalize and validate raw Windows events.
3. Store activity securely in a local SQLite database.
4. Build behavioral profiles for users over time windows.
5. Determine whether sufficient real data exists for machine learning.
6. Develop an anomaly detection model in a later phase.
7. Provide a SOC-style dashboard for security monitoring.
8. Support future multi-computer endpoint ingestion.
9. Maintain a strict privacy boundary around collected data.
10. Provide a foundation for future cloud deployment.

---

# 🔐 Authorization & Privacy

UBADS is designed around an explicit authorization and privacy principle.

### Real Data

Only activity from **authorized Windows computers** is collected and used as the real research dataset.

Real collected data:

- Is stored locally
- Is excluded from Git
- Is not committed to the repository
- Is treated as sensitive activity data
- Is used for feature engineering and future ML research

### Demo Data

The dashboard contains a separate **Demo Lab** containing hard-coded synthetic scenarios.

Demo data:

- Is clearly labeled `DEMO DATA`
- Is clearly labeled `SIMULATED RESULT`
- Is completely isolated from the real dataset
- Is never inserted into `data/app.db`
- Is never used as real-world evidence

### Privacy Boundary

UBADS does **not** collect or expose:

- ❌ Passwords
- ❌ Password hashes
- ❌ Keystrokes
- ❌ Screenshots
- ❌ Webcam data
- ❌ Microphone data
- ❌ Private messages
- ❌ Browser passwords
- ❌ Browser cookies
- ❌ Document contents

The system works with normalized security/activity metadata only.

---

# 🏗️ System Architecture

```text
┌──────────────────────────────────────┐
│     Authorized Windows Computer      │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│          Windows Log Collector       │
│        Python + pywin32              │
│                                      │
│ Authentication                       │
│ File Activity                        │
│ Process Activity                     │
│ Device / USB Activity                │
└──────────────────┬───────────────────┘
                   │
          Normalize / Validate
                   │
                   ▼
┌──────────────────────────────────────┐
│             SQLite Database          │
│                                      │
│ events                               │
│ behavior_features                    │
│ endpoints                            │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│            ML Service                │
│                                      │
│ Feature Engineering                  │
│ User Classification                  │
│ Baselines                            │
│ Data Quality                         │
│ ML Readiness                         │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│             FastAPI Backend          │
│                                      │
│ REST API                             │
│ Authentication Foundation            │
│ Endpoint Management                  │
│ Error Handling                       │
│ Health / Readiness                   │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│       React + Vite + Tailwind        │
│          SOC / UEBA Dashboard        │
│                                      │
│ LIVE DATA        DEMO LAB            │
│ Real Events      Synthetic Scenarios │
└──────────────────────────────────────┘
