# 🌱 CropCycle

### Multi-temporal NDVI analysis for understanding crop growth cycles

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![JWT](https://img.shields.io/badge/Auth-JWT-000000?logo=jsonwebtokens&logoColor=white)](https://jwt.io/)
[![Leaflet](https://img.shields.io/badge/Maps-Leaflet-199900?logo=leaflet&logoColor=white)](https://leafletjs.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> **CropCycle** is a full-stack agricultural analysis system that transforms multi-temporal NDVI time-series data into crop growth stages, health indicators, vegetation coverage, and season-level insights.

The project is designed around a simple idea: **instead of looking at NDVI values as isolated measurements, use the time series to understand how a crop is developing over an entire season.**

---

## ✨ What the project does

CropCycle takes dated NDVI observations and turns them into an interpretable crop-cycle model.

```text
NDVI Time Series
       ↓
   Data Upload
       ↓
Signal Smoothing
       ↓
Stage Detection
       ↓
┌─────────────────────────────┐
│ Growth Start                │
│ Peak Growth                 │
│ Harvest / Decline           │
└─────────────────────────────┘
       ↓
Crop Health + Season Metrics
       ↓
Visual Analysis Dashboard
```

### Core capabilities

- 📈 Analyze multi-temporal NDVI observations
- 🌱 Detect the beginning of crop growth
- ☀️ Identify peak vegetation / peak growth
- 🌾 Estimate the harvest or sustained-decline stage
- 🧮 Calculate a crop health score from 0–100
- 🌿 Classify vegetation coverage as **Poor, Fair, Good, or Excellent**
- 🗓️ Estimate growing-season duration
- 📊 Calculate average, minimum, maximum, and variance of NDVI
- 🧩 Split the season into five interpretable phases
- 📂 Upload CSV datasets or load demo data
- 🔐 Support user authentication with JWT
- 🗺️ Provide map-related support through Leaflet

---

## 🧠 How the analysis works

The analysis engine lives in [`backend/services/analysisService.js`](backend/services/analysisService.js).

### 1. Smooth the signal

A **3-point moving average** is applied to the NDVI series to reduce short-term noise before stage detection.

### 2. Detect growth start

The engine looks for the first point that rises above a threshold set at **25% of the NDVI range above the minimum**, while also showing a consistent upward trend.

### 3. Find peak growth

Peak growth is identified as the point with the **maximum NDVI value in the smoothed series**.

### 4. Estimate harvest / decline

After the peak, the engine searches for a sustained decline that falls below a threshold set at **35% of the observed NDVI range above the minimum**.

### 5. Calculate crop health

The health score combines:

| Component | Maximum contribution |
|---|---:|
| Peak NDVI | 40 points |
| Average NDVI | 30 points |
| Season length | 20 points |
| Detection confidence | 10 points |

### 6. Segment the crop cycle

The resulting time series is divided into five phases:

**Dormant → Growing → Peak → Declining → Harvest**

> These rules are intentionally deterministic and explainable, making the system useful for experimentation, prototyping, and academic evaluation.

---

## 📊 Example input

CropCycle accepts a simple two-column CSV format:

```csv
date,ndvi
2023-11-01,0.12
2023-12-01,0.31
2024-01-15,0.55
2024-03-15,0.85
2024-05-01,0.35
```

The analysis engine can then derive values such as:

```text
Growth Start      → detected from rising NDVI
Peak Growth       → highest NDVI
Harvest / Decline → sustained post-peak decrease
Average NDVI      → seasonal mean
Health Score      → 0–100 composite score
Season Length     → growth-start to harvest duration
```

---

## 🌿 Included demo datasets

The repository includes example agricultural datasets for different regions and crops.

| Dataset | Crop | Location | Points |
|---|---|---|---:|
| `wheat_india` | Wheat | Punjab, India | 15 |
| `rice_kerala` | Rice | Kerala, India | 13 |
| `corn_maharashtra` | Corn | Maharashtra, India | 12 |

The backend also contains a sample wheat CSV at:

```text
backend/sample_data/wheat_punjab_sample.csv
```

---

## 🏗️ Architecture

```text
┌───────────────────────────────┐
│           Frontend            │
│       Vite / React UI         │
│   Charts • Data • Dashboard   │
└───────────────┬───────────────┘
                │ REST API
                ▼
┌───────────────────────────────┐
│            Backend            │
│        Node.js + Express      │
│                               │
│ Auth • Data • Analysis APIs   │
│            │                  │
│            ▼                  │
│     Analysis Service          │
│   NDVI → stages + metrics     │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│            MongoDB            │
│ Users • Crop Data • Analysis  │
└───────────────────────────────┘
```

---

## 📁 Project structure

```text
Extraction-of-Crop-Cycle-Parameters-from-Multi-Temporal-Data/
│
├── backend/
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── dataController.js
│   │   └── analysisController.js
│   ├── middleware/
│   │   └── auth.js
│   ├── models/
│   │   ├── User.js
│   │   ├── CropData.js
│   │   └── Analysis.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── data.js
│   │   └── analysis.js
│   ├── services/
│   │   └── analysisService.js
│   ├── sample_data/
│   │   └── wheat_punjab_sample.csv
│   ├── .env.example
│   ├── package.json
│   └── server.js
│
├── frontend/
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
│
├── standalone/
│   └── ... standalone demo assets
│
├── package.json
├── README_VSCODE.md
└── README.md
```

---

## 🛠️ Tech stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | React / Vite | Interactive analysis interface |
| Visualization | Recharts / Chart.js | NDVI and crop-cycle charts |
| Maps | Leaflet | Geographic visualization support |
| Backend | Node.js / Express | REST API and application logic |
| Database | MongoDB / Mongoose | Users, datasets, analysis results |
| Authentication | JWT / bcryptjs | Secure user authentication |
| File processing | Multer / csv-parse | CSV upload and parsing |
| Configuration | dotenv | Environment-based configuration |

---

## 🔌 API

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register a user |
| `POST` | `/api/auth/login` | Login and receive JWT |
| `GET` | `/api/auth/me` | Get the authenticated user |

### Crop data

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/data/upload` | Upload a CSV dataset |
| `POST` | `/api/data/demo` | Load demo data |
| `GET` | `/api/data` | List user datasets |
| `GET` | `/api/data/:id` | Get one dataset and its time series |
| `DELETE` | `/api/data/:id` | Delete a dataset and its analysis |

### Analysis

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/analysis` | List analyses for the user |
| `GET` | `/api/analysis/:cropDataId` | Get analysis for a dataset |

---

## 🚀 Run locally

### Prerequisites

- **Node.js 18+**
- **MongoDB** locally or through MongoDB Atlas

### Backend

```bash
cd backend
npm install
cp .env.example .env
```

Configure the environment file:

```env
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

Start the API:

```bash
npm run dev
```

Backend runs on:

```text
http://localhost:5000
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend runs on:

```text
http://localhost:5173
```

### Standalone demo

The repository also includes a standalone frontend demo. The existing `frontend/index.html` can be opened directly in a browser for a backend-independent demonstration.

---

## ☁️ Deployment

The current project structure is suitable for a split deployment model:

### Backend → Render

1. Deploy the `backend` directory as a Web Service.
2. Install dependencies with `npm install`.
3. Start with `npm start`.
4. Configure:
   - `MONGODB_URI`
   - `JWT_SECRET`
   - `NODE_ENV=production`
   - `FRONTEND_URL`

### Frontend → Vercel

1. Deploy the `frontend` directory.
2. Set the API URL environment variable used by the frontend.
3. Use the Vite framework configuration.

---

## 🎯 Why this project matters

Remote-sensing datasets can contain a large amount of information, but raw NDVI observations are not immediately useful to every user.

CropCycle focuses on the layer in between **raw satellite-derived measurements** and **interpretable agricultural information**:

```text
Raw NDVI observations
          ↓
     Signal cleanup
          ↓
   Biological stages
          ↓
     Crop metrics
          ↓
   Human-readable insight
```

The project can serve as a foundation for future work involving richer remote-sensing inputs, crop-specific models, anomaly detection, forecasting, and decision-support systems.

---

## 🔭 Future directions

Potential next steps include:

- 🤖 ML-based crop-stage detection instead of fixed thresholds
- 🛰️ Direct ingestion of Sentinel-2 / Landsat time-series data
- 🗺️ Field-level geospatial analysis
- 🌧️ Weather and rainfall integration
- 🚨 Crop-stress and anomaly detection
- 📅 Yield and harvest-date forecasting
- 🧠 Crop-specific calibration of thresholds and health metrics
- 📡 Automated time-series updates instead of manual CSV uploads

---

## 👨‍💻 Project

Built by **Jyatin** as a research-oriented full-stack project exploring how time-series remote-sensing data can be converted into practical crop-cycle information.

**Repository:** [Extraction-of-Crop-Cycle-Parameters-from-Multi-Temporal-Data](https://github.com/Jyatin/Extraction-of-Crop-Cycle-Parameters-from-Multi-Temporal-Data)

---

## 📄 License

This project is licensed under the **MIT License**.

---

<p align="center">
  <b>🌱 Turning NDVI time series into a story of the crop season.</b>
</p>
