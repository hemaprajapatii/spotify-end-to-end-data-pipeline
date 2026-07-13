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

### 1. Extract (ડેટા કલેક્શન)
* **Amazon CloudWatch** દરરોજ એક ટ્રિગર ફાયર કરે છે જે **AWS Lambda (Data Extraction)** ફંક્શનને રન કરે છે.
* લેમ્બડા ફંક્શન Python સ્ક્રિપ્ટ દ્વારા **Spotify API** માંથી લેટેસ્ટ મ્યુઝિક ડેટા ખેંચે છે.
* આ ખેંચેલો રો ડેટા (Raw JSON) **Amazon S3 (raw data bucket)** માં સેવ થાય છે.

### 2. Transform (ડેટા ક્લીનિંગ અને ફોર્મેટિંગ)
* જેવો નવો ડેટા raw S3 બકેટમાં આવે છે, એટલે એક **S3 Event Trigger (SQS)** ફાયર થાય છે.
* આ ટ્રિગર બીજા **AWS Lambda (Data Transformation)** ફંક્શનને ચાલુ કરે છે જે JSON ડેટાને મસ્ત અને ક્લીન સ્ટ્રક્ચર્ડ ફોર્મેટમાં કન્વર્ટ કરે છે.
* ફાઇનલ ટ્રાન્સફોર્મ થયેલો ડેટા **Amazon S3 (transformed data bucket)** માં લોડ થાય છે.

### 3. Load & Analyze (ડેટા લોડિંગ)
* **AWS Glue Crawler** ટ્રાન્સફોર્મ થયેલા ડેટાની બકેટને સ્કેન કરીને તેનો સ્કીમા (Schema) આપોઆપ સમજી લે છે.
* તે ડેટાને **AWS Glue Data Catalog** માં ટેબલ તરીકે સ્ટોર કરે છે.
* છેલ્લે, **Amazon Athena** નો ઉપયોગ કરીને આ ડેટા પર સીધી જ SQL ક્વેરીઝ રન કરીને એનાલિટિક્સ કરી શકાય છે.

---

## 📂 Project Structure inside Repository

* `spotify_api_data_extract.py` -> AWS Lambda code for extracting data from Spotify API.
* `spotify_transformation_load_function.py` -> AWS Lambda code for converting JSON to structured formats.
* `Spotify Data Pipeline Project.ipynb` -> Jupyter Notebook used for local testing and prototyping.
* `spotipy_layer.zip` -> Deployment layer containing library dependencies for AWS Lambda.
* `architecture.png` -> High-level architecture diagram of the pipeline.# spotify-end-to-end-data-pipeline
