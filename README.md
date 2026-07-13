# Spotify End-to-End Data Engineering Pipeline

An end-to-end data engineering pipeline to extract, transform, and load (ETL) Spotify music data using AWS services. The pipeline automates data collection, structures raw JSON data into analytics-ready formats, and automates cataloging for querying.

## 📌 Architecture Diagram

![Project Architecture](architecture.png)

## 🛠️ Tech Stack & AWS Services
* **Language:** Python
* **Data Source:** Spotify API (Spotipy)
* **Orchestration:** Amazon CloudWatch (Daily Trigger)
* **Compute:** AWS Lambda (Serverless ETL)
* **Storage:** Amazon S3 (Raw & Transformed Data Buckets)
* **Message Trigger:** Amazon SQS / EventBridge
* **Data Cataloging:** AWS Glue Crawler & AWS Glue Data Catalog
* **Analytics Engine:** Amazon Athena (SQL Analytics)

---

## ⚙️ Data Pipeline Flow (ETL)

### 1. Extract (Data Collection)
* **Amazon CloudWatch** fires a daily trigger that runs the **AWS Lambda (Data Extraction)** function.
* The Lambda function fetches the latest music data from the **Spotify API** using a Python script.
* This fetched raw data (Raw JSON) is saved in the **Amazon S3 (raw data bucket)**.

### 2. Transform (Data Cleaning & Formatting)
* As soon as new data arrives in the raw S3 bucket, an **S3 Event Trigger (SQS)** is fired.
* This trigger activates the second **AWS Lambda (Data Transformation)** function, which converts the complex JSON data into clean, structured formats.
* The final transformed data is loaded into the **Amazon S3 (transformed data bucket)**.

### 3. Load & Analyze (Data Loading)
* **AWS Glue Crawler** scans the transformed data bucket and automatically infers its schema.
* It stores the data as tables inside the **AWS Glue Data Catalog**.
* Finally, **Amazon Athena** is used to run SQL queries directly on this data for analytics.

---

## 📂 Project Structure inside Repository

* `spotify_api_data_extract.py` -> AWS Lambda code for extracting data from Spotify API.
* `spotify_transformation_load_function.py` -> AWS Lambda code for converting JSON to structured formats.
* `Spotify Data Pipeline Project.ipynb` -> Jupyter Notebook used for local testing and prototyping.
* `spotipy_layer.zip` -> Deployment layer containing library dependencies for AWS Lambda.
* `architecture.png` -> High-level architecture diagram of the pipeline.
