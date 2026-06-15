# 🚇 Riyadh Metro Data Pipeline

An end-to-end data engineering pipeline that ingests Riyadh Metro data via API, orchestrates processing with Apache Airflow, stores and transforms it in Google Cloud, and visualizes insights through a Looker Studio dashboard.

---

## 📐 Architecture Overview

```
Riyadh Metro API
       │
       ▼
 Apache Airflow          ← Orchestration & scheduling
       │
       ├──► Google Cloud Storage   ← Raw data lake
       │
       └──► BigQuery (raw table)   ← Initial load
                  │
                  ▼
          BigQuery (transformed table)   ← SQL transformations
                  │
                  ▼
          Looker Studio Dashboard        ← Visualization
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Orchestration | Apache Airflow |
| Cloud Storage | Google Cloud Storage (GCS) |
| Data Warehouse | Google BigQuery |
| Transformation | BigQuery SQL |
| Visualization | Looker Studio (Google Data Studio) |
| Data Source | Riyadh Metro Public API |

---

## 📁 Project Structure

```
riyadh-metro-pipeline/
├── dags/
│   └── metro_pipeline_dag.py       # Main Airflow DAG definition
├── dashboard/
│   └── looker_studio_link.md       # Link to the published dashboard
└── README.md
```

---

## 🔄 Pipeline Walkthrough

### 1. Data Ingestion
The pipeline starts by calling the **Riyadh Metro API** to fetch the latest transit data. The Airflow DAG handles scheduling, retries, and error handling for this step.

### 2. Storage to GCS
Raw API responses are downloaded and uploaded directly to a **Google Cloud Storage** bucket, serving as the raw data lake. This preserves the original data before any transformation.

### 3. Load into BigQuery (Raw Table)
The raw files are loaded from GCS into a **raw BigQuery table**, making the data queryable and ready for transformation.

### 4. Transformation
SQL transformations are applied within BigQuery to clean, structure, and enrich the data — including type casting, filtering, and field renaming. The results are written to a separate **transformed BigQuery table**.

### 5. Dashboard Visualization
The transformed table is connected to **Looker Studio** to power an interactive dashboard with real-time metro insights.

---

## ⚙️ Setup & Prerequisites

### Requirements
- Google Cloud Platform account with the following APIs enabled:
  - Cloud Storage API
  - BigQuery API
- Apache Airflow (local or Cloud Composer)
- Python 3.8+
- GCP Service Account with appropriate IAM roles:
  - `roles/storage.admin`
  - `roles/bigquery.dataEditor`
  - `roles/bigquery.jobUser`

### GCP Configuration

1. Create a GCS bucket:
```bash
gsutil mb gs://your-riyadh-metro-bucket
```

2. Create a BigQuery dataset:
```bash
bq mk --dataset your_project_id:riyadh_metro
```

3. Set your GCP credentials:
```bash
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account-key.json"
```

### Airflow Setup

1. Install dependencies:
```bash
pip install apache-airflow apache-airflow-providers-google
```

2. Place the DAG file in your Airflow `dags/` folder.

3. Configure Airflow GCP connection:
   - Go to **Admin → Connections** in the Airflow UI
   - Add a new connection with `Conn Type: Google Cloud` and your service account credentials

4. Trigger the DAG:
```bash
airflow dags trigger metro_pipeline_dag
```

---

## 📊 Dashboard

The Looker Studio dashboard visualizes key Riyadh Metro metrics including ridership trends, station activity, and service patterns.

> 🔗 **[View Dashboard](#)** ← *(replace with your Looker Studio link)*

---

## 🚧 Future Improvements

- Add data quality checks and alerting within Airflow
- Implement incremental loading to avoid full refreshes
- Parameterize configs using Airflow Variables
- Add CI/CD pipeline for DAG deployment
- Containerize with Docker for portable local development

---

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request.

---

## 📄 License

This project is licensed under the MIT License.
