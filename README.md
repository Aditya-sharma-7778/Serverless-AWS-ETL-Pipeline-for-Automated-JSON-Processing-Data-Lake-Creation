# **AWS Serverless ETL Pipeline – JSON to Parquet Data Lake**

This project demonstrates a fully automated **serverless ETL pipeline** built on AWS, capable of ingesting nested JSON data, transforming it using Python, and storing it in an optimized Parquet format for analytics using AWS Glue and Athena.

---

## 🚀 **Architecture Overview**

**Trigger:** Upload JSON file → S3 Event Notification → Lambda Execution → Parquet Output → Glue Crawler → Athena

**Flow:**

1. Raw JSON file uploaded to **Amazon S3** triggers Lambda.
2. **AWS Lambda** reads, parses, and flattens nested customer & product order data.
3. Data is converted to **Parquet** using Pandas + PyArrow and pushed to S3 Data Lake.
4. **AWS Glue Crawler** updates schema in Glue Data Catalog.
5. Data becomes query-ready in **Amazon Athena**.

---

## 🧰 **AWS Services Used**

* **Amazon S3** – Raw data storage & data lake.
* **AWS Lambda** – Serverless transformation logic.
* **AWS Glue Crawler** – Automatic schema detection & Data Catalog updates.
* **AWS Glue Data Catalog** – Central metadata store.
* **Amazon CloudWatch** – Logs & monitoring.
* **Amazon Athena** (Optional) – Query transformed Parquet data using SQL.

---

## 📂 **Project Structure**

```
AWS-ETL-Pipeline/
│
├── lambda_function.py        # Main ETL logic
├── orders_etl.json           # Sample input JSON file
└── README.md                 # Project documentation
```

---

## 🧠 **How the ETL Works**

### **1️⃣ Extract**

Lambda is triggered when `orders_etl.json` is uploaded to S3.
It reads and loads JSON content.

### **2️⃣ Transform**

The `flatten()` function:

* Iterates through each order
* Extracts customer + product details
* Creates a clean tabular structure
* Converts into a Pandas DataFrame

### **3️⃣ Load**

* DataFrame is saved as **Parquet** into S3 (Data Lake folder)
* Timestamp is added for versioning
* Glue Crawler runs to update schema automatically

---

## 🧪 **Sample Input: JSON File (`orders_etl.json`)**

Contains nested order, customer, and product details like:

```json
{
  "order_id": 1,
  "order_date": "2024-01-10",
  "total_amount": 200.50,
  "customer": {"customer_id": 101, "name": "John Doe", ...},
  "products": [
    {"product_id": "P01", "name": "Wireless Mouse", ...},
    {"product_id": "P02", "name": "Bluetooth Keyboard", ...}
  ]
}
```

---

## 🧩 **Lambda Code Summary**

* Parse JSON from S3
* Flatten using Python
* Convert to Parquet
* Upload to S3 staging
* Trigger Glue Crawler

---

## 📊 **Output Example (Parquet Schema)**

* order_id
* order_date
* total_amount
* customer_id
* customer_name
* email
* product_id
* product_name
* category
* price
* quantity

---

## 🔍 **How to Query the Data in Athena**

After Glue Crawler finishes:

```sql
SELECT customer_name, product_name, quantity, total_amount
FROM orders_parquet_datalake
WHERE category = 'Electronics';
```

---

## 📦 **Use Cases**

* Building Data Lakes
* E‑commerce analytics
* Serverless pipelines
* Automated schema discovery
* JSON → Parquet optimization

---

## 👨‍💻 **Author**

This project is built for showcasing AWS Data Engineering & ETL capabilities.


