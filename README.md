# ⚔️ WikiDuel: Icons Edition

> An interactive semantic comparison game built on the **Wikimedia Structured Dataset**, challenging players to deduce which cultural icon possesses a greater encyclopedic footprint and verified citation backing.

[![Live Demo](https://img.shields.io/badge/Demo-Live%20App-emerald?style=for-the-badge)](https://<YOUR_GITHUB_USERNAME>.github.io/wikiduel/)
[![Platform](https://img.shields.io/badge/Stack-Polars%20%7C%20HTML5%20%7C%20TailwindCSS-amber?style=for-the-badge)](#tech-stack)
[![Dataset](https://img.shields.io/badge/Data-Wikimedia%20Structured%20Parquet-blue?style=for-the-badge)](#the-data-pipeline)

---

## 📌 Overview

**WikiDuel** transforms massive open-source knowledge graphs into a fast, gamified head-to-head experience. Players evaluate iconic figures across **Cricket**, **Global Football**, **Cinema**, and **Science & Tech**, guessing which entity holds a deeper factual footprint on Wikipedia.

Instead of arbitrary vanity metrics (like social followers), WikiDuel uses genuine structural features extracted directly from Wikipedia’s underlying graph—including primary academic/news citations, section depths, and verified infobox claims.

---

## 🔄 Project Architecture & Workflow

The project is architected to respect strict memory, compute, and data-integrity boundaries:

┌─────────────────────────────────────────────────────────────┐
│ 1. Raw Wikimedia Shard (~44 GB total / ~250 MB single shard) │
│    - enwiki_namespace_0_*.parquet                           │
└──────────────────────────────┬──────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ 2. Kaggle Polars ETL Pipeline                               │
│    - Scans schema: ["name", "main_entity", "references", ..]│
│    - Filters high-prominence public figures                 │
│    - Computes Composite Citation Depth Score                │
│    - Resolves Wikimedia Commons image URLs via MD5 hashing  │
└──────────────────────────────┬──────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ 3. Static Knowledge Slice (< 20 KB JSON)                    │
│    - wiki_duel_cards.json (25-30 curated icons)             │
│    - 100% offline, zero API latency, zero backend server    │
└──────────────────────────────┬──────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────────┐
│ 4. Client-Side Duel Engine (Vercel / GitHub Pages)          │
│    - Dynamic category filtering                             │
│    - Instant verification & streak state management         │
│    - Post-answer contextual breakdown & source citation gist│
└─────────────────────────────────────────────────────────────┘

---

## 📊 How the "Citations & Depth" Score Works

To adhere to the competition’s rule against arbitrary dummy data, each entity's metric is systematically calculated using structural weights from the Wikipedia Structured Content schema:

$$\text{Score} = (2 \times \text{References}) + (4 \times \text{Sections}) + (8 \times \text{Infobox Attributes})$$

* **Verified References (`references`):** Footnoted secondary and primary sources (e.g., ESPN Cricinfo scorecards, FIFA match logs, Royal Society archives, Nobel Prize committee dossiers).
* **Section Depth (`sections`):** Encyclopedic subsections detailing discrete career milestones, tournament histories, and historical controversies.
* **Infobox Graph Attributes (`infoboxes`):** Structured key-value claims (caps, goals, awards, honors, birth data).

### Example Breakdown

| Icon | Category | Citations & Depth | Key Driving Citations |
| :--- | :--- | :--- | :--- |
| **Albert Einstein** | Science & Tech | **790** | Princeton Collected Papers, Nobel archives, general relativity proofs |
| **Lionel Messi** | Football | **740** | 8 Ballon d'Or dossiers, 2022 World Cup logs, UEFA match appendices |
| **Cristiano Ronaldo** | Football | **720** | Portuguese FPF archives, 130+ international goals, transfer logs |
| **Sachin Tendulkar** | Cricket | **610** | Wisden archives spanning 4 decades, Bharat Ratna gazette records |
| **Virat Kohli** | Cricket | **520** | 350+ ESPN Cricinfo logs, 80 international centuries, captaincy records |

---

## 🛠️ Tech Stack

* **Data Extraction & Transformation:** [Polars](https://pola.rs/) (Python) inside Kaggle Notebooks for fast parallelized Parquet scanning.
* **Frontend:** HTML5, Modern Vanilla JavaScript (ES6+), [Tailwind CSS](https://tailwindcss.com/) (via CDN).
* **Data Store:** Lightweight static JSON (`wiki_duel_cards.json`, ~15 KB).
* **Hosting:** GitHub Pages / Vercel (Client-side static deployment).

---

## 🚀 Local Setup & Quickstart

### 1. Clone the Repository
```bash
git clone [https://github.com/](https://github.com/)VaibhavGovekar/wikiduel.git
cd wikiduel