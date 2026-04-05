# Customer Segmentation using RFM Analysis and K-Means Clustering

This project performs **customer segmentation** on retail transaction data using **RFM analysis** and **K-Means clustering**. The goal is to group customers based on their purchasing behavior so businesses can better understand customer value, identify loyal customers, detect inactive users, and design targeted marketing strategies.

---

## Overview

Customer segmentation is a powerful technique used in marketing and customer relationship management. In this project, customers are segmented using three important behavioral metrics:

- **Recency** – How recently a customer made a purchase
- **Frequency** – How often a customer makes purchases
- **Monetary** – How much money a customer spends

After computing these RFM features, **K-Means clustering** is applied to identify distinct customer groups.

---

## Objectives

- Clean and preprocess transaction-level retail data
- Generate **RFM features** for each customer
- Handle outliers and scale the data for clustering
- Use the **Elbow Method** to determine the optimal number of clusters
- Apply **K-Means clustering** to segment customers
- Visualize and interpret the resulting customer segments

---

## Workflow

The project follows a complete customer segmentation pipeline:

1. Load and inspect the retail dataset
2. Clean missing and invalid records
3. Create a **Monetary Value** feature from transaction data
4. Convert transaction dates into proper datetime format
5. Build **Recency, Frequency, and Monetary** metrics for each customer
6. Remove extreme outliers to improve clustering quality
7. Standardize the RFM features
8. Use the **Elbow Method** to choose the best cluster count
9. Fit the **K-Means** model
10. Assign cluster labels to customers
11. Analyze cluster sizes and average RFM values
12. Visualize clusters in **2D and 3D**

---

## Technologies Used

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Scikit-learn**

---

## Project Structure

```bash
customer-segmentation-rfm-kmeans/
│
├── data/
│   └── retail_data.csv
├── notebooks/
│   └── customer_segmentation.ipynb
├── images/
│   ├── elbow_method.png
│   ├── cluster_plot_2d.png
│   └── cluster_plot_3d.png
├── README.md
└── requirements.txt
```
