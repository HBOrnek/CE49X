# CE49X — Conflict Situation Monitoring for Maritime Shipping

**Boğaziçi University · Spring 2026 · Dr. Eyuphan Koç**

Correlating NASA FIRMS satellite thermal anomalies with armed-conflict news to build an early situation monitoring system for a maritime shipping company.

---

## 📁 Files

| File | Description |
|---|---|
| `Final_Project_HamzaBarisOrnek.ipynb` | Main Jupyter notebook (all 4 tasks) |
| `requirements.txt` | Python dependencies |
| `docker-compose.yml` | PostgreSQL database setup |
| `dashboard.png` | Sample output: 6-panel dashboard (300 DPI) |

---

## 🚀 Setup & Run (Step by Step)

### Step 1: Prerequisites
- Python 3.8+
- Docker Desktop ([download here](https://www.docker.com/products/docker-desktop/))

Check installations:
```bash
python3 --version
docker --version
```

### Step 2: Start the PostgreSQL database

From inside the `baris-odev/` folder:
```bash
docker-compose up -d
```

Verify it's running:
```bash
docker ps
```
You should see `ce49x-postgres` listed.

> **Note**: If you don't have docker-compose, run the equivalent command from the assignment:
> ```bash
> docker run --name ce49x-postgres \
>   -e POSTGRES_USER=ce49x \
>   -e POSTGRES_HOST_AUTH_METHOD=trust \
>   -e POSTGRES_DB=conflict_monitoring \
>   -p 5432:5432 \
>   -d postgres:16
> ```

### Step 3: Create a Python virtual environment

```bash
python3 -m venv venv
source venv/bin/activate          # macOS/Linux
# venv\Scripts\activate           # Windows
```

### Step 4: Install dependencies

```bash
pip install -r requirements.txt
```

### Step 5: (Optional) Get a NASA FIRMS API key

A NASA FIRMS API key is required to pull real satellite data (used in this project):

1. Register free at: https://firms.modaps.eosdis.nasa.gov/api/area/
2. Set the key in Cell 3 of the notebook (`FIRMS_MAP_KEY = "your_key_here"`),
   or as an environment variable before launching Jupyter:
   ```bash
   export FIRMS_MAP_KEY="your_key_here"   # macOS/Linux
   set FIRMS_MAP_KEY=your_key_here        # Windows CMD
   ```

### Step 6: Run the notebook

```bash
jupyter notebook Final_Project_HamzaBarisOrnek.ipynb
```

In the Jupyter UI:
- **Kernel → Restart & Run All**

Everything executes top-to-bottom (~30 seconds with sample data).

---

## 📊 What Gets Produced

After running, the following files are generated in this folder:

| File | Content |
|---|---|
| `dashboard.png` | Main 6-panel summary dashboard (300 DPI) |
| `temporal_analysis.png` | Monthly event counts + FRP trends |
| `spatial_analysis.png` | World maps of thermal events |
| `confusion_matrices.png` | ML model evaluation (4 classifiers) |
| `feature_importances.png` | Decision tree feature ranking |
| `coverage_comparison.png` | News coverage by region |
| `day_night_pattern.png` | Day vs night detection ratios |

The PostgreSQL database (`conflict_monitoring`) will contain 4 populated tables:
`firms_detections`, `news_articles`, `thermal_events`, `event_matches`.

Verify with:
```bash
docker exec -it ce49x-postgres psql -U ce49x -d conflict_monitoring -c "\dt"
docker exec -it ce49x-postgres psql -U ce49x -d conflict_monitoring -c "SELECT COUNT(*) FROM firms_detections;"
```

---

## 🌍 Regions Analyzed

Selected for direct relevance to global shipping & energy markets:

| Region | Why it matters |
|---|---|
| **Ukraine** | Black Sea grain shipping; European energy supply |
| **Gaza / Israel** | Eastern Mediterranean; Suez Canal feeder routes |
| **Yemen / Red Sea** | Bab-el-Mandeb strait; Houthi attacks 2024 rerouted 30–40% of container shipping |
| **Iraq / Syria** | Middle East oil fields; pipeline infrastructure |

Time period: **2024-01-01 → 2024-07-01** (6 months)

---

## 📋 Task Breakdown (matches grading rubric)

| Task | Points | What the notebook does |
|---|---|---|
| **1. Data Collection** | 30 | Pulls VIIRS thermal data from NASA FIRMS (or generates sample), scrapes conflict news from GDELT, stores in PostgreSQL |
| **2. Spatial & Temporal Analysis** | 25 | DBSCAN clustering with Haversine metric (eps=10km, time=3d), monthly/spatial visualizations |
| **3. Correlation & Classification** | 30 | Event-news matching, Mann-Whitney U hypothesis test, 4 ML classifiers (LR/DT/NB/SVM) with confusion matrices |
| **4. Dashboard & Discussion** | 15 | 6-panel `dashboard.png` (300 DPI), written discussion in markdown cells |
| **Total** | **100** | |

---

## 🛠️ Troubleshooting

**Database connection fails?**
```bash
docker-compose down && docker-compose up -d
```

**Module not found errors?**
Make sure your virtual environment is active:
```bash
source venv/bin/activate
pip install -r requirements.txt
```

**FIRMS API errors?**
The notebook automatically falls back to sample data — just continue running.

---

## 📤 Submission Checklist

- [x] `Final_Project_HamzaBarisOrnek.ipynb` runs top-to-bottom without errors
- [x] `dashboard.png` saved at 300 DPI
- [x] `requirements.txt` lists all dependencies
- [x] `README.md` explains setup and reproduction
- [x] `docker-compose.yml` recreates the database
- [ ] Push to your GitHub course fork
- [ ] Record presentation video (10–15 min)
- [ ] Email video link to `eyuphan.koc@bogazici.edu.tr` and `mustafa.ozcetin@std.bogazici.edu.tr`
- [ ] Subject: `CE49X Final Project Video — <Group Member Names>`
