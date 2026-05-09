# 🏛️ Prophecy India — Property Intelligence Engine

> *Stop predicting. Start understanding.*

[![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0-000?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![GSAP](https://img.shields.io/badge/GSAP-3.12-88CE02?logo=greensock&logoColor=black)](https://greensock.com/gsap/)
[![Leaflet](https://img.shields.io/badge/Leaflet-1.9-199900?logo=leaflet&logoColor=white)](https://leafletjs.com/)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](#-run-with-docker)

A transparent ML engine that explains the **why** behind the **how much** — built on 13,200 Bangalore datapoints and extended to **33+ Indian cities** through public housing benchmarks.

---

## 📖 About the Project

**Prophecy India** is an advanced real-estate intelligence platform designed to bring transparency to the Indian housing market. Unlike traditional "black-box" calculators, Prophecy uses a multi-tiered inference engine to provide not just a price, but a mathematical justification for that price.

### The Core Problem
The Indian real estate market is notoriously fragmented. Price discovery often relies on word-of-mouth or opaque aggregator data. For a buyer, understanding whether a property is "fairly priced" requires analyzing locality medians, builder premiums, and market sentiment—all of which are rarely available in one place.

### The Prophecy Solution
This project solves the discovery problem through:
1.  **Localized Machine Learning**: A high-precision Linear Regression model trained on 13,000+ Bangalore records (R² 0.84) with per-feature contribution analysis.
2.  **Explainable AI (XAI)**: Every prediction is broken down into specific contributors (e.g., "Area added ₹X Lakhs," "Locality adjustment was -₹Y Lakhs").
3.  **Investment Scoring**: A 0–10 score that benchmarks the property against real-time locality or city-level pulses.
4.  **Pan-India Scalability**: Extended to 33 major Indian cities using a benchmark-driven "City Index" model.

---

## ⚡ Why This Stands Out

Most real-estate predictors are black boxes. Prophecy is **two engines in one**, each transparent about its method:

| Mode | When | How it works |
|---|---|---|
| 🧠 `ml-localized` | Bangalore + locality | Full LinearRegression model with 240 one-hot localities (R² 0.84). Per-feature contributions extracted directly from coefficients. |
| 🌏 `city-index`   | Any of 33 Indian cities | Public median ₹/sqft benchmarks × area + BHK/bath adjustment factors + sentiment multiplier. Honest, free, reproducible. |

Both paths return **the same shape of response**: price + ranked explanation + investment score + coords.

---

## 🏗️ System Architecture

```mermaid
flowchart LR
    subgraph Data["📦 Data Layer"]
        A[Bengaluru_House_Data.csv<br/>13,200 rows]
        B[bhp.csv<br/>cleaned + engineered]
        S[locality_stats.json<br/>BLR locality medians]
        G[locality_coords.json<br/>BLR geo lookup]
        I[city_index.json<br/>33 Indian cities · public ₹/sqft]
    end

    subgraph Train["🧪 Training (notebook)"]
        C[Outlier removal<br/>±σ on ₹/sqft per locality]
        D[Feature engineering]
        E[GridSearchCV<br/>Linear · Lasso · DecisionTree]
        F[banglore_home_prices_model.pickle<br/>R² ≈ 0.84]
    end

    subgraph API["⚙️ Flask API"]
        H["/predict_home_price"]
        J["/compare_locations"]
        K["/sales_pitch"]
        M["/cities · /coords · /get_location_names"]
    end

    subgraph UI["🌐 SPA"]
        L[GSAP + ScrollTrigger<br/>Leaflet Atlas<br/>Magnetic CTA]
    end

    A --> C --> D --> E --> F
    B --> S
    F & S --> H
    I --> H
    H & J & K & M --> L
```

---

## 📈 Model Performance

| Metric | Value |
|---|---|
| Bangalore algorithm | **LinearRegression** (selected via `GridSearchCV` over Linear / Lasso / DecisionTree) |
| Bangalore R² | **0.84** |
| Bangalore features | 243 (sqft, bath, BHK + 240 one-hot localities) |
| Bangalore training rows | ~10,500 (after outlier removal) |
| Pan-India coverage | 33 cities (Tier 1 + Tier 2 + select Tier 3) |
| City-index source | Public NHB Residex + aggregated listings (2024) |

---

## 🛠️ Technical Requirements & Setup

### Requirements
*   **Python**: 3.9+ (Tested on 3.11)
*   **OS**: Windows/macOS/Linux
*   **Memory**: ~500MB (Lightweight model)

### Installation Guide
1.  **Clone the repository**:
    ```bash
    git clone https://github.com/Vaishnavi-Dubey/prophecy-india.git
    cd prophecy-india
    ```
2.  **Install dependencies**:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the application**:
    ```bash
    python server.py
    ```
    *Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser.*

### 🐳 Run with Docker
If you prefer containerized deployment:
```bash
docker build -t prophecy-india .
docker run -p 5000:5000 prophecy-india
```

---

## 🔌 API Reference

| Method | Endpoint | Body | Returns |
|---|---|---|---|
| `GET`  | `/get_location_names` | — | Bangalore localities |
| `GET`  | `/cities`             | — | 33 Indian cities + benchmarks |
| `GET`  | `/areas/<city>`       | — | Areas within a specific city |
| `POST` | `/predict_home_price` | `city, location?, total_sqft, bhk, bath, sentiment` | price + explanation + investment |

---

## 📁 Project Layout

```
prophecy-india/
├── server.py                          # Flask API + static SPA host
├── util.py                            # 2-mode inference, XAI, scoring, pitch
├── app.html / app.css / app.js        # Redesigned SPA (GSAP + Leaflet)
├── banglore_home_prices_model.pickle  # trained sklearn model
├── city_index.json                    # 33 Indian cities · public benchmarks
├── city_areas.json                    # 346 curated areas across India
├── price-prediction.ipynb             # training + EDA notebook
├── requirements.txt
└── README.md
```

---

## 🙌 Credits

Built by **[Vaishnavi Dubey](https://github.com/Vaishnavi-Dubey)**.
Bangalore dataset: [Bengaluru House Price Data (Kaggle)](https://www.kaggle.com/datasets/amitabhajoy/bengaluru-house-price-data).
City benchmarks: aggregated public NHB Residex + listing medians (2024).

> [!NOTE]
> The previous `price_prediction` repository should be deleted manually from your GitHub settings as current API tokens do not have administrative `delete_repo` scopes.

---
© 2026 Prophecy India. Property Intelligence Reimagined.
