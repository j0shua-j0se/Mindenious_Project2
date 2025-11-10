# 🧩 Customer Segmentation with K‑Means

Unsupervised clustering of mall customers to discover actionable segments for marketing and product strategy. Uses the classic `Mall_Customers.csv` dataset and K‑Means with elbow & silhouette analysis. Produces clear 2D/3D visuals and cluster profiles.

---

## 1) Quick Start

### A. Setup
```
git clone https://github.com/<your-username>/customer-segmentation-kmeans.git
cd customer-segmentation-kmeans
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
pip install -r requirements.txt
```

### B. Run (choose one)
```
# Notebook
jupyter notebook notebooks/customer_segmentation.ipynb

# Script (fit + save outputs)
python src/run_kmeans.py --data data/Mall_Customers.csv --k 5 --out outputs/
```

---

## 2) Data

- File: `data/Mall_Customers.csv`  
- Columns: `CustomerID, Gender, Age, Annual Income (k$), Spending Score (1-100)`  
- Target: None (unsupervised); we cluster on features.

---

## 3) Approach (one glance)

- Features used: `Age`, `Annual Income (k$)`, `Spending Score (1-100)`  
  - Optional: add `Gender` as `Gender_enc` (Male=1, Female=0)
- Scaling: `StandardScaler` (critical for distance-based clustering)
- Model: **K‑Means (k-means++)**, `n_init=10`, `max_iter=300`
- K selection: **Elbow (inertia)** + **Silhouette score** (best typically K=5)
- Outputs:
  - Cluster labels appended to data
  - 2D plot: Income vs Spending with centroids
  - 3D plot: Age–Income–Spending
  - Cluster profiles (means per cluster)

---

## 4) Minimal Project Layout

```
.
├── data/
│   └── Mall_Customers.csv
├── notebooks/
│   └── customer_segmentation.ipynb
├── src/
│   ├── preprocess.py          # scaling / encoding
│   ├── kmeans.py              # fit, elbow, silhouette
│   ├── visualize.py           # 2D/3D plots
│   └── run_kmeans.py          # CLI entrypoint
├── outputs/
│   ├── clusters_2d.png
│   ├── clusters_3d.png
│   ├── elbow_curve.png
│   ├── silhouette_scores.png
│   ├── cluster_profiles.csv
│   └── labeled_customers.csv
├── requirements.txt
└── README.md
```

---

## 5) Example CLI

```
# Find K via diagnostics
python src/run_kmeans.py --data data/Mall_Customers.csv --find-k

# Fit final model at K=5 and export everything
python src/run_kmeans.py --data data/Mall_Customers.csv --k 5 --out outputs/
```

Key flags:
- `--k INT` number of clusters (omit to only run diagnostics)
- `--include-gender` to add one‑hot/encoded gender
- `--out PATH` output directory (default: `outputs/`)

---

## 6) Requirements

```
pandas>=1.3
numpy>=1.21
scikit-learn>=1.0
matplotlib>=3.4
seaborn>=0.11
jupyter>=1.0
```

Install:
```
pip install -r requirements.txt
```

---

## 7) Interpreting Segments (typical with K=5)

- Cluster A: **Loyal High‑Spenders** — high income, high score  
- Cluster B: **High Earners, Low Spend** — upsell premium/value services  
- Cluster C: **Young Enthusiasts** — low income, high score, trend‑driven  
- Cluster D: **Budget‑Conscious** — average income, moderate score  
- Cluster E: **Low Income, Low Spend** — necessity buyers, price‑sensitive

Use `outputs/cluster_profiles.csv` to confirm on your run.

---

## 8) Notes

- Scaling is mandatory for K‑Means to prevent any feature from dominating distance.  
- If clusters look non‑spherical, consider GMM or DBSCAN in future work.  
- Update visualizations to save `.png` files for reports.
