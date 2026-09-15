<div align="center">

# 🌱 CropCycle

### From satellite vegetation data to crop-cycle insights.

**A full-stack system for analysing multi-temporal NDVI data, detecting crop growth stages, and turning raw vegetation signals into readable agricultural insights.**

<br/>

![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=111827)
![MongoDB](https://img.shields.io/badge/MongoDB-8-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Express](https://img.shields.io/badge/Express-4-000000?style=for-the-badge&logo=express&logoColor=white)

<br/>

[📖 Documentation](#-how-it-works) · [🚀 Setup](#-run-locally) · [🔌 API](#-api) · [🛠️ Tech Stack](#️-tech-stack)

</div>

---

## 🌾 What is CropCycle?

CropCycle takes a time series of **NDVI (Normalized Difference Vegetation Index)** observations and uses it to estimate the major stages of a crop's seasonal cycle.

Instead of looking at a table of vegetation values, the application turns the data into a simple story:

```text
Raw NDVI data
      ↓
Noise reduction
      ↓
Growth detection
      ↓
Peak detection
      ↓
Decline / harvest detection
      ↓
Crop health + season metrics
      ↓
Visual agricultural dashboard
```

The project combines a **data-analysis engine, REST API, database, authentication, CSV ingestion, and frontend visualisation** into one application.

---

## ✨ Highlights

| 🌱 | Capability | What it does |
|---|---|---|
| 📈 | **NDVI Analysis** | Processes multi-temporal vegetation observations |
| 🧭 | **Stage Detection** | Identifies growth start, peak growth and harvest |
| 📊 | **Crop Metrics** | Calculates health, season length, average/max/min NDVI and variance |
| 🗺️ | **Location Support** | Supports agricultural datasets with geographic context |
| 📁 | **CSV Upload** | Import your own NDVI time-series data |
| 🔐 | **Authentication** | JWT-based user authentication and protected data |
| 🧪 | **Demo Data** | Test the system with sample crop datasets |
| ⚡ | **Full Stack** | React/Vite frontend + Node/Express backend + MongoDB |

---

## 🔬 How It Works

### 01 · Smooth the signal

A **3-point moving average** is applied to reduce short-term noise in the NDVI signal.

### 02 · Find growth start

The engine looks for the first sustained upward movement above a baseline threshold.

### 03 · Find peak growth

The highest NDVI value in the smoothed series becomes the estimated **peak growth stage**.

### 04 · Detect decline / harvest

After the peak, the engine searches for a sustained decline below the calculated harvest threshold.

### 05 · Calculate crop health

A 0–100 health score combines:

- Peak NDVI
- Average NDVI
- Growing-season duration
- Detection confidence

### 06 · Segment the crop cycle

The final time series is divided into five readable phases:

```text
Dormant ─────── Growing ─────── Peak ─────── Declining ─────── Harvest
   🌑              🌿             🌳              🍂                🌾
```

---

## 📊 What the System Produces

From a simple dataset like:

```csv
date,ndvi
2023-11-01,0.12
2023-12-01,0.31
2024-01-15,0.55
2024-03-15,0.85
2024-05-01,0.35
```

CropCycle can derive metrics such as:

```text
┌──────────────────────────────────────────┐
│             CROP ANALYSIS                 │
├──────────────────────────────────────────┤
│  🌱 Growth Start       2023-12-01         │
│  📈 Peak Growth        2024-03-15         │
│  🌾 Harvest            2024-05-01         │
│                                          │
│  ❤️ Crop Health        0–100 score       │
│  📊 Average NDVI       calculated         │
│  ⛰️  Maximum NDVI       calculated         │
│  📉 NDVI Variance      calculated         │
│  📅 Season Length      calculated         │
│  🌿 Coverage           Poor/Fair/Good/    │
│                        Excellent           │
└──────────────────────────────────────────┘
```

---

## 🏗️ Architecture

```text
                    ┌───────────────────┐
                    │    React / Vite   │
                    │    Frontend       │
                    └─────────┬─────────┘
                              │
                         REST API
                              │
                    ┌─────────▼─────────┐
                    │  Node + Express   │
                    │     Backend       │
                    └─────┬───────┬─────┘
                          │       │
                  ┌───────▼─┐ ┌──▼──────────────┐
                  │ MongoDB │ │ Analysis Engine │
                  │         │ │      NDVI       │
                  └─────────┘ └─────────────────┘
```

---

## 📁 Project Structure

```text
Extraction-of-Crop-Cycle-Parameters-from-Multi-Temporal-Data/
│
├── backend/
│   ├── config/             # Database configuration
│   ├── controllers/        # Auth, data & analysis controllers
│   ├── middleware/         # JWT authentication
│   ├── models/             # User, crop data & analysis schemas
│   ├── routes/             # REST API routes
│   ├── services/           # Core crop-cycle analysis engine
│   ├── sample_data/        # Example NDVI datasets
│   ├── .env.example
│   └── server.js
│
├── frontend/
│   ├── index.html          # Frontend / standalone demo
│   ├── vite.config.js
│   └── package.json
│
├── standalone/             # Standalone project assets
├── package.json
└── README.md
```

---

## 🚀 Run Locally

### Requirements

- **Node.js 18+**
- **MongoDB** — local instance or MongoDB Atlas

### Backend

```bash
cd backend
npm install
cp .env.example .env
```

Configure your environment:

```env
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_secret
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

Start the server:

```bash
npm run dev
```

Backend → `http://localhost:5000`

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend → `http://localhost:5173`

> 💡 The standalone `frontend/index.html` can also be opened directly in a browser for a backend-free demo.

---

## 🔌 API

### Authentication

| Method | Endpoint | Purpose |
|:---:|---|---|
| `POST` | `/api/auth/register` | Create account |
| `POST` | `/api/auth/login` | Login and receive JWT |
| `GET` | `/api/auth/me` | Get authenticated user |

### Crop Data

| Method | Endpoint | Purpose |
|:---:|---|---|
| `POST` | `/api/data/upload` | Upload NDVI CSV |
| `POST` | `/api/data/demo` | Load demo dataset |
| `GET` | `/api/data` | List user datasets |
| `GET` | `/api/data/:id` | Get dataset + time series |
| `DELETE` | `/api/data/:id` | Delete dataset + analysis |

### Analysis

| Method | Endpoint | Purpose |
|:---:|---|---|
| `GET` | `/api/analysis` | List analyses |
| `GET` | `/api/analysis/:cropDataId` | Get analysis for dataset |

---

## 🌿 Included Demo Datasets

| Dataset | Crop | Region | Observations |
|---|---|---|---:|
| 🌾 `wheat_india` | Wheat | Punjab, India | 15 |
| 🍚 `rice_kerala` | Rice | Kerala, India | 13 |
| 🌽 `corn_maharashtra` | Corn | Maharashtra, India | 12 |

---

## 🛠️ Tech Stack

<div align="center">

| Layer | Technology |
|---|---|
| **Frontend** | React 18 · Vite · Recharts / Chart.js |
| **Backend** | Node.js · Express 4 |
| **Database** | MongoDB · Mongoose |
| **Authentication** | JWT · bcryptjs |
| **Data ingestion** | Multer · csv-parse |
| **Mapping** | Leaflet |
| **Styling** | Custom CSS · CSS Variables |

</div>

---

## ☁️ Deployment

### Frontend → Vercel

```text
1. Import the frontend project
2. Configure VITE_API_URL
3. Select Vite as the framework
4. Deploy
```

### Backend → Render

```text
1. Create a Web Service
2. Set MONGODB_URI
3. Set JWT_SECRET
4. Set FRONTEND_URL
5. Build → npm install
6. Start → npm start
```

---

## 🎯 Why This Project?

Crop-cycle information is often hidden inside raw remote-sensing time series. CropCycle explores a practical way to make that information easier to interpret by combining **remote-sensing data, time-series analysis, backend systems, and visualisation** in one workflow.

The project was built around a simple idea:

> **Turn a vegetation signal into something a person can understand and act on.**

---

## 🔭 Future Scope

Potential extensions include:

- 🛰️ Direct satellite-data ingestion
- 🤖 ML-based crop-stage classification
- 🌦️ Weather-aware crop analysis
- 🗺️ Field-level geospatial analytics
- 📡 Near-real-time monitoring
- 📱 Mobile-friendly agricultural dashboards
- 📈 Historical crop-cycle comparison

---

## 📄 License

MIT License.

Built as an academic and technical exploration of **crop-cycle analysis using multi-temporal vegetation data**.

<div align="center">

### 🌱 Built with data, code & curiosity.

**[Jyatin](https://github.com/Jyatin)**

</div>
