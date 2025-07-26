# 📦 Data Ingestion from S3 to RDS with Fallback to AWS Glue using Dockerized Python Application

This project implements a resilient, cloud-native, and containerized data ingestion pipeline using **Amazon S3**, **Amazon RDS**, **AWS Glue**, and **Docker**. It is designed to ingest CSV data from S3 into an RDS MySQL database, and automatically falls back to AWS Glue for cataloging if the RDS operation fails.

---

## 🚀 Features

- ✅ Automated ingestion of CSV data from S3  
- ✅ Primary data upload to Amazon RDS  
- ✅ Fallback mechanism to AWS Glue on failure  
- ✅ Dockerized for consistent deployment  
- ✅ AWS-native service integration (boto3, Glue, RDS, S3)  
- ✅ IAM-based secure service access  

---

## 🧠 Objectives

- Automate CSV data ingestion from Amazon S3 to RDS.  
- Implement a fallback data cataloging system using AWS Glue.  
- Package the application in a Docker container for portability and scalability.  

---

## 🛠️ Technology Stack

| Component        | Technology/Tool          |
|------------------|--------------------------|
| Programming      | Python 3.9+              |
| Cloud Services   | Amazon S3, RDS, AWS Glue |
| Containerization | Docker                   |
| Libraries        | `boto3`, `pandas`, `sqlalchemy`, `pymysql` |

---

## 🏗️ Architecture Overview

CSV File (S3)
|
v
Dockerized Python Script
|
v
Try to Push to Amazon RDS
|___________
| |
Success Failure
| v
v Fallback to AWS Glue
Data in RDS Data Catalog in Glue (with S3 Location)

---

## ⚙️ Configuration Parameters

Update the Python script or use environment variables for:

- **S3 Bucket Name** and **CSV File Key**  
- **RDS**: Endpoint, Username, Password, DB Name, Table Name  
- **Glue**: Database Name, Table Name, S3 Location  

---

## 📦 Installation & Deployment

### 1. Clone the Repository

git clone https://github.com/rohinipandhare12/S3-RDS-Glue-Fallback.git
cd S3-RDS-Glue-Fallback
2. Prepare AWS Services
✅ Create an S3 bucket and upload your CSV.

✅ Launch and configure an RDS MySQL-compatible instance.

✅ Create an AWS Glue database (can be empty initially).

✅ Create an IAM Role with:

AmazonS3FullAccess

AWSGlueConsoleFullAccess

Add the role to the EC2 instance or pass AWS credentials in the container.

3. Python Environment Setup (for local testing)

pip install -r requirements.txt
python main.py
4. Dockerize the Application
Dockerfile Example:
dockerfile

FROM python:3.9
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
Build & Run Docker Image:

docker build -t data-ingestor .
docker run -e AWS_ACCESS_KEY_ID=your_key \
           -e AWS_SECRET_ACCESS_KEY=your_secret \
           -e AWS_DEFAULT_REGION=your_region \
           data-ingestor
✅ Execution Steps Summary
Read CSV from S3 using boto3

Parse data with pandas

Attempt RDS insertion using sqlalchemy

If RDS fails, create a Glue table using boto3

Register S3 location in Glue Data Catalog

🔍 Data Validation
Use MySQL Workbench or CLI to verify data in RDS.

Check AWS Glue Data Catalog for fallback table and schema.

📊 Results
✅ Data successfully uploaded to RDS (or)

✅ Fallback to AWS Glue succeeded

✅ Docker logs confirm full flow execution

🧾 Conclusion
This project demonstrates a fault-tolerant, automated, and Dockerized data pipeline leveraging AWS services. 
By providing fallback using AWS Glue, it ensures reliability even in cases where RDS operations fail.
