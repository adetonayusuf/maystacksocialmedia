# maystacksocialmedia


# 📊 Creator Economy Analytics with Azure, Databricks & Power BI

### 🔍 **Project Title:**  
**Leveraging Social Media Analytics for Strategic Competitor and Niche Analysis in the Creator Economy**

---

## 📁 Project Overview

This project focuses on building a **scalable end-to-end data pipeline** for ingesting, processing, transforming, and analyzing **social media insights** related to creators and their content performance. It is tailored for use cases like audience segmentation, content effectiveness, and competitor intelligence in the **telecommunications & media space**.

---

## 🧱 Architecture Overview

![Mainstack Architecture](https://github.com/adetonayusuf/maystacksocialmedia/blob/main/MainstackArchitecture.drawio.png)

### 🔧 Technologies Used
- **Azure Data Factory (ADF)** – Data ingestion (REST APIs → Data Lake)
- **Azure Data Lake Storage (Gen2)** – Bronze, Silver, Gold storage zones
- **Azure Databricks** – Data transformation using PySpark
- **Azure PostgreSQL Database** – Data serving layer
- **Power BI** – Reporting and dashboarding
- **GitHub** – CI/CD version control (planned)
- **Optional**: Azure Monitor, Great Expectations (for future extensions)

---

## ✅ Key Deliverables

| Layer   | Description                                                                 |
|---------|-----------------------------------------------------------------------------|
| **Bronze** | Raw ingestion of social media data via REST API into Azure Data Lake      |
| **Silver** | Cleaned, deduplicated, typecast data using Apache Spark                   |
| **Gold**   | Aggregated fact and dimension tables ready for analysis                   |
| **SQL DB** | PostgreSQL database hosted on Azure containing gold tables                |
| **Power BI** | 2-page executive dashboard with KPIs, charts, filters, and deep dives   |

---

📌 Step 1: API Ingestion via ADF
Created 4 Copy Data activities (Demographics, Engagement, Content, Competitors)

Targeted endpoint: https://mainstack-media-api.amdari.io

Stored results in: mainstack/bronze/[table_name] (as .parquet)

📌 Step 2: Mount Data Lake in Databricks
python
Copy
Edit
dbutils.fs.mount(
  source = "wasbs://mainstack@mainstackstorage.blob.core.windows.net/bronze",
  mount_point = "/mnt/bronze",
  extra_configs = {"fs.azure.sas.mainstack.mainstackstorage.blob.core.windows.net": "<sas_token>"}
)
📌 Step 3: Silver Layer Transformation
Cleaned and deduplicated data using PySpark

Applied cast, dropDuplicates, withColumnRenamed

Saved outputs to /mnt/silver/[table]

📌 Step 4: Gold Modeling
Created star schema: fact_engagement, fact_content, dim_creators, dim_time, dim_platforms

Computed fields like engagement_score, engagement_rate, monetization_category

Stored to: /mnt/gold

📌 Step 5: Export to PostgreSQL
python
Copy
Edit
df_fact_engagement.write.jdbc(
    url=jdbc_url,
    table="fact_engagement",
    mode="overwrite",
    properties=connection_properties
)
📌 Step 6: Power BI Visualization
Connected Power BI Desktop to Azure PostgreSQL

Created 2-page executive dashboard:

Page 1: Overview KPIs, monetization %, engagement trend

![Executive Summary](https://github.com/adetonayusuf/maystacksocialmedia/blob/main/Executive%20Summary.png)

Page 2: Performance bands, creator segments, content heatmap

![Performance](https://github.com/adetonayusuf/maystacksocialmedia/blob/main/Performance%20%26%20Audience.png)

DAX measures: Engagement Rate, Monetized Content %, Performance Band

---

## 📊 Sample Visuals (Power BI)
<details>
<summary>Click to expand</summary>

- Total Reach, Likes, Revenue KPIs  
- Engagement Rate over Time  
- Creators by Subscription Tier  
- Performance Band Treemap  
- Heatmap: Content Type × Platform

</details>

---

## 📦 Folder Structure

```bash
MainstackAnalytics/
│
├── notebooks/
│   └── SilverTransformations.ipynb
│   └── GoldModeling_Notebook.ipynb
│
├── pipelines/
│   └── ADF_ingestion_pipeline.json (deprecated)
│
├── data/
│   └── sample_exports/
│       └── dim_creators.csv
│
├── powerbi/
│   └── dashboard.pbix
│
└── README.md
```

---

## 🔐 Security Notes
- SAS tokens used for secure access to Data Lake
- PostgreSQL credentials managed via Databricks secret scopes
- Optional integration: Azure Key Vault for enterprise-grade security

---

## 🚀 Future Improvements
- Add **Delta Lake** format for ACID-compliant tables
- Integrate **Great Expectations** for column-level validation
- Use **GitHub Actions** for deploying notebooks and pipelines
- Enable real-time streaming using Event Hubs or Kafka

---

## 📬 Contact

> **Yusuf Adetona**  
[LinkedIn](https://www.linkedin.com/in/yusuf-adetona-hnd-bsc-aat-aca-acca-diploma-in-ifrs/) • [Portfolio](https://www.datascienceportfol.io/adetonayusuf)  
📧 yustone003@yahoo.com  
📞 +234-706-205-6766
