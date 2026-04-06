# Customer Segmentation using RFM Analysis and KMeans Clustering

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange?style=flat-square&logo=scikit-learn)
![pandas](https://img.shields.io/badge/pandas-Data%20Analysis-150458?style=flat-square&logo=pandas)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## Project Overview

This project applies **RFM Analysis** combined with **KMeans Clustering** to segment customers of an online retail business into meaningful groups based on their purchasing behavior. The goal is to help businesses understand their customer base better — enabling targeted marketing, improved retention strategies, and smarter resource allocation.

### What is RFM?

**RFM** stands for:

- **Recency (R)** — How recently did a customer make a purchase? A customer who bought last week is more engaged than one who bought last year.
- **Frequency (F)** — How often does a customer buy? Frequent buyers are more loyal and engaged.
- **Monetary (M)** — How much money has a customer spent in total? High spenders represent greater business value.

Together, these three metrics paint a clear picture of each customer's value to the business.

### What is KMeans Clustering?

**KMeans** is an unsupervised machine learning algorithm that groups data points into **K distinct clusters** based on similarity. In this project, it groups customers with similar RFM profiles together — automatically identifying segments like loyal customers, at-risk customers, and high-value shoppers — without any pre-labeling.

---

## Features

- Cleans and preprocesses raw retail transaction data for reliable analysis
- Engineers RFM metrics from transactional history on a per-customer basis
- Handles outliers to ensure clustering is not skewed by extreme values
- Applies feature scaling for fair distance-based clustering
- Uses the **Elbow Method** to determine the optimal number of clusters
- Segments customers into distinct, actionable groups using KMeans
- Labels clusters with human-readable business categories (e.g., "Loyal", "At-Risk")
- Generates rich visualizations including 2D scatter plots, 3D cluster views, and summary bar/line charts

---

## Tech Stack

| Tool / Library    | Purpose                                  |
| ----------------- | ---------------------------------------- |
| **Python 3.13.5** | Core programming language                |
| **pandas**        | Data loading, cleaning, and manipulation |
| **matplotlib**    | Base plotting and custom visualizations  |
| **seaborn**       | Statistical and aesthetic visualizations |
| **scikit-learn**  | Feature scaling and KMeans clustering    |

---

## Dataset Description

The project uses an **Online Retail dataset** containing transactional records from a UK-based e-commerce business. Each row represents a single product line within an invoice.

| Column        | Description                                  |
| ------------- | -------------------------------------------- |
| `CustomerID`  | Unique identifier for each customer          |
| `InvoiceNo`   | Unique invoice/transaction number            |
| `InvoiceDate` | Date and time of the transaction             |
| `StockCode`   | Unique product/item code                     |
| `Description` | Name of the product                          |
| `Quantity`    | Number of units purchased in the transaction |
| `UnitPrice`   | Price per unit of the product (in GBP £)     |
| `Country`     | Country where the customer resides           |

> 📎 **Dataset Source:** [UCI Machine Learning Repository – Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/Online+Retail)

---

## Project Workflow

The project follows a structured, end-to-end data science pipeline:

### 1. Data Loading & Inspection

The retail dataset is loaded into a pandas DataFrame. An initial inspection is performed to understand the shape of the data, column types, and a statistical summary — helping identify potential issues before any processing begins.

### 2. Data Cleaning

Rows with missing `CustomerID` values are removed since customer identity is fundamental to RFM analysis. Cancelled transactions (identified by invoice codes starting with `'C'`) are filtered out. Records with negative quantities or zero/negative unit prices are also excluded to ensure data integrity.

### 3. Feature Engineering — Monetary Value

A new column, `MonetaryValue`, is derived by multiplying `Quantity` by `UnitPrice` for each transaction line. This forms the foundation of the **Monetary** dimension of RFM.

### 4. Date Conversion & Recency Calculation

The `InvoiceDate` column is converted to a proper datetime format. A **reference date** (typically the day after the last recorded transaction) is established. **Recency** is then calculated as the number of days since each customer's most recent purchase.

### 5. RFM Aggregation per Customer

All transactions are grouped by `CustomerID` to compute the three core metrics:

- **Recency** — Days since last purchase
- **Frequency** — Total number of unique invoices
- **Monetary** — Total amount spent across all transactions

This produces a concise, one-row-per-customer RFM table.

### 6. Outlier Handling

Extreme outliers in the RFM features — such as unusually high spenders or excessively frequent buyers — are capped or removed using statistical thresholds (e.g., IQR-based filtering). This prevents outliers from distorting cluster boundaries.

### 7. Feature Scaling

Since RFM values operate on different scales (days vs. count vs. currency), **StandardScaler** from scikit-learn is applied to standardize all three features. This ensures that no single dimension dominates the distance calculations in KMeans.

### 8. Elbow Method — Choosing Optimal K

KMeans is run iteratively for a range of K values (e.g., K = 2 to 10). The **Within-Cluster Sum of Squares (WCSS/Inertia)** is plotted against K. The "elbow" point — where the rate of improvement sharply decreases — indicates the optimal number of clusters.

### 9. KMeans Clustering

With the optimal K selected, the final KMeans model is trained on the scaled RFM data. Each customer is assigned a cluster label representing their behavioral segment.

### 10. Cluster Labeling

The clusters are analyzed by computing average RFM values per cluster. Based on these profiles, each cluster is assigned a meaningful business label such as:

- **Cluster 0 (Red) - Retain** — High-value customers who purchase frequently, though not very recently
- **Cluster 1 (Green) - Re-Engage** — Low-value, infrequent customers who have not purchased recently
- **Cluster 2 (Blue) - Nurture** — Least active and lowest-value customers who have purchased recently
- **Cluster 3 (Yellow) - Reward** — High-value, very frequent, and still actively purchasing loyal customers

### 11. Visualization

Multiple plots are generated to communicate findings clearly:

- **3D Scatter Plot** — All three RFM dimensions visualized simultaneously
- **Bar Charts** — Average RFM values per cluster for easy comparison
- **Elbow Curve** — WCSS vs. K to justify the cluster count selection

### 12. Cluster Interpretation

The final step translates the statistical clusters into business insights — explaining who each customer segment is, what drives their behavior, and what strategic actions (e.g., loyalty rewards, win-back campaigns) are most appropriate for each group.

---

## Results & Insights

The KMeans model segments customers into **4 distinct groups** (based on optimal K), each with a unique behavioral profile:

#### Cluster 0 (Red): **Retain**

##### Characteristics

This cluster represents high-value customers who purchase frequently, though not very recently. The focus should be on retention efforts to maintain their loyalty and spending levels.

##### Strategy

Implement loyalty programs, personalized offers, and regular engagement to ensure they remain active.

#### Cluster 1 (Green): **Re-Engage**

##### Characteristics

This cluster includes low-value customers and infrequent buyers who have not purchased recently. The focus should be on re-engagement to bring them back to active purchasing behavior.

##### Strategy

Use targeted marketing campaigns, special discounts, or reminders to encourage them to return and purchase again.

#### Cluster 2 (Blue): **Nurture**

##### Characteristics

This cluster represents the least active and lowest-value customers, but they have made recent purchases. These customers may be new or may need nurturing to increase their engagement and spending.

##### Strategy

Focus on building relationships, providing excellent customer service, and offering incentives to encourage more frequent purchases.

#### Cluster 3 (Yellow): **Reward**

##### Characteristics

This cluster includes high-value customers and very frequent buyers, many of whom are still actively purchasing. They are the most loyal customers, and rewarding their loyalty is key to maintaining their engagement.

##### Strategy

Implement a robust loyalty program, provide exclusive offers, and recognize their loyalty to keep them engaged and satisfied.

---

## Visualizations

The following visualizations are produced as part of the analysis:

- **Elbow Curve** — Identifies the optimal number of clusters (K)
- **2D Scatter Plot** — Recency vs. Monetary, colored by cluster
- **3D Scatter Plot** — All three RFM features in a single 3D view
- **Bar Chart** — Mean Recency, Frequency, and Monetary per cluster
- **Customer Count per Cluster** — Bar chart showing segment size distribution

---

## How to Run the Project

### Prerequisites

Ensure you have **Python 3.13.5** installed along with `pip`.

### Step 1 — Clone the Repository

```bash
git clone https://github.com/your-username/customer-segmentation-rfm.git
cd customer-segmentation-rfm
```

### Step 2 — Create a Virtual Environment (Recommended)

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### Step 3 — Install Required Libraries

```bash
pip install -r requirements.txt
```

Or install manually:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Step 4 — Add the Dataset

Download the **Online Retail dataset** from the [UCI Repository](https://archive.ics.uci.edu/ml/datasets/Online+Retail) and place the file in the `data/` folder:

```
customer-segmentation-rfm/
│
├── data/
│   └── online_retail.xlsx
├── notebooks/
│   └── rfm_clustering.ipynb
├── outputs/
├── requirements.txt
└── README.md
```

### Step 5 — Run the Notebook

Launch Jupyter Notebook and open the analysis file:

```bash
jupyter notebook notebooks/rfm_clustering.ipynb
```

Run all cells sequentially from top to bottom. Outputs and visualizations will be saved in the `outputs/` directory.

---

## Future Improvements

- [ ] **RFM Scoring System** — Add a traditional 1–5 scoring system per RFM dimension for segment comparison across time periods
- [ ] **Interactive Dashboard** — Build a Plotly Dash or Streamlit dashboard for dynamic, filterable cluster exploration
- [ ] **DBSCAN / Hierarchical Clustering** — Compare KMeans results against alternative clustering algorithms
- [ ] **Time-Series Segmentation** — Track how customer segments evolve month-over-month
- [ ] **Predictive Modeling** — Use cluster labels as features in a churn prediction or CLV (Customer Lifetime Value) model
- [ ] **Deployment** — Package the pipeline into a REST API using FastAPI for real-time customer scoring

---

## License

```
MIT License

Copyright (c) 2025 [Subham Chauhan]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

<p align="center">
  Made by <a href="https://github.com/Subham1751">Subham Chauhan/a> &nbsp;|&nbsp;
  ⭐ Star this repo if you found it helpful!
</p>
