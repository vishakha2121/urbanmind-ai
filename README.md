# 🏙️ UrbanMind AI

<div align="center">

![UrbanMind AI Banner](https://img.shields.io/badge/🏙️_UrbanMind-AI-6366f1?style=for-the-badge&logoColor=white)

**Autonomous Urban Planning System powered by Multi-Agent Simulation & Google Gemini AI**

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev)
[![TailwindCSS](https://img.shields.io/badge/Tailwind-3-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)
[![Gemini](https://img.shields.io/badge/AI-Gemini-FF6F00?style=flat-square&logo=google&logoColor=white)](https://ai.google.dev)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57?style=flat-square&logo=sqlite&logoColor=white)](https://sqlite.org)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

[Features](#-features) • [Architecture](#-architecture) • [Tech Stack](#-tech-stack) • [Setup](#-setup-guide) • [API Docs](#-api-endpoints) • [Screenshots](#-screenshots)

</div>

---

## 🎯 What is UrbanMind AI?

**UrbanMind AI** is an AI-driven urban planning simulator that helps city planners, students, and researchers visualize the future impact of urban development decisions — **before spending a single rupee on construction**.

Instead of relying on guesswork, UrbanMind AI runs **6 independent simulation agents** in parallel. Each agent models one critical dimension of city life. Their results are then passed to **Google Gemini AI**, which synthesizes everything into **human-readable, actionable recommendations**.

Think of it as a **"digital twin sandbox"** for cities — you describe a scenario, and the AI tells you what will happen.

### 🌍 The Problem It Solves

Traditional urban planning is:
- ❌ **Slow** — takes months of surveys and reports
- ❌ **Expensive** — consultants charge crores
- ❌ **Reactive** — problems found only *after* construction
- ❌ **Siloed** — traffic, housing, pollution analyzed separately

**UrbanMind AI fixes this** by simulating all dimensions **in seconds**, in **one unified dashboard**, with **AI recommendations** that tell you exactly what to fix first.

---

## ✨ Features

### 🧠 Multi-Agent Simulation Engine
- **6 autonomous agents** running in parallel
- Each agent is **modular** — add a new one without touching others
- Deterministic math models (no black-box ML)
- Real-time scoring (0-100) for each dimension

### 🤖 AI Recommendation Engine
- Powered by **Google Gemini 1.5 Flash**
- Strict **JSON output** parsing
- Actionable recommendations in **plain English**
- Priority ranking (what to fix first)
- Risk flagging (what will break by 2030)

### 🎨 Modern Dashboard
- **6 score cards** with color-coded health indicators
- **Radar chart** showing overall city balance
- **Bar chart** comparing agents side-by-side
- **Line chart** projecting growth over 10 years
- **Interactive Leaflet map** with color-coded zones

### 📊 Simulation History
- Save every simulation to SQLite
- Compare past scenarios
- Re-run with different parameters
- Export results as JSON

### 🚀 Developer Friendly
- **Runs on CPU** — no GPU required
- **Zero setup DB** — SQLite file-based
- **Free AI** — Gemini free tier (60 req/min)
- **Fully typed** — Pydantic + TypeScript-style schemas
- **Tested** — pytest for agents & API

---

## 🧠 The 6 Autonomous Agents

| Agent | What It Simulates | Output Metric | Formula Preview |
|-------|-------------------|---------------|-----------------|
| 🚗 **Traffic Agent** | Vehicle density, road congestion, peak-hour load | Traffic Score (0-100) | `score = 100 - (vehicles / road_capacity) * 50` |
| 🏠 **Housing Agent** | Population density, housing availability, affordability | Housing Score (0-100) | `score = min(100, housing_units / population * 100)` |
| 💧 **Utilities Agent** | Water demand, electricity load, waste capacity | Utilities Score (0-100) | `score = 100 - (demand / supply) * 40` |
| 🌫️ **Pollution Agent** | AQI estimate from traffic + industry + green cover | AQI Value + Score | `aqi = (traffic * 0.4) + (industry * 0.5) - (green * 0.3)` |
| 🚌 **Transport Agent** | Public transport coverage, accessibility | Transport Score (0-100) | `score = (bus_stops / area) * 10` |
| 📈 **Growth Agent** | Population projection, land-use expansion over 10 years | Growth Curve (JSON) | `pop(t) = pop0 * (1 + rate)^t` |

### Agent Architecture

Each agent extends `BaseAgent`:

```python
class BaseAgent(ABC):
    name: str
    description: str
    
    @abstractmethod
    def simulate(self, params: dict) -> dict:
        """Run simulation and return structured result"""
        pass
        -- Simulations table
CREATE TABLE simulations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    city_name VARCHAR(100) NOT NULL,
    population INTEGER NOT NULL,
    area_sqkm FLOAT NOT NULL,
    budget_crore FLOAT NOT NULL,
    overall_score FLOAT,
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Agent results table
CREATE TABLE agent_results (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    simulation_id INTEGER REFERENCES simulations(id),
    agent_name VARCHAR(50) NOT NULL,
    score FLOAT NOT NULL,
    data JSON NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Recommendations table
CREATE TABLE recommendations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    simulation_id INTEGER REFERENCES simulations(id),
    overall_score FLOAT,
    health_status VARCHAR(20),
    zones JSON,
    risks JSON,
    actions JSON,
    priority_order JSON,
    raw_response TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

urbanmind-ai/
│
├── backend/
│   ├── app/
│   │   ├── agents/          # 6 simulation agents
│   │   ├── models/          # SQLAlchemy models
│   │   ├── schemas/         # Pydantic schemas
│   │   ├── services/        # Business logic
│   │   ├── routers/         # API endpoints
│   │   └── utils/           # Helpers
│   ├── sql/                 # Raw SQL files
│   ├── tests/               # pytest tests
│   ├── main.py              # Entry point
│   └── requirements.txt
│
├── frontend/
│   ├── src/
│   │   ├── components/      # Reusable UI
│   │   ├── pages/           # Route pages
│   │   ├── api/             # Axios calls
│   │   ├── store/           # Zustand store
│   │   └── utils/           # Helpers
│   ├── index.html
│   └── package.json
│
├── docs/                    # Documentation
├── README.md
└── .gitignore