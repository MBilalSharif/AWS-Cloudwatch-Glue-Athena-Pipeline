AWS Glue ETL Pipeline for CloudWatch Logs Processing & Athena Querying
## 📌 Project Overview

This project demonstrates how to build a serverless log processing pipeline using AWS services. The pipeline retrieves logs from Amazon CloudWatch Logs, processes them using an AWS Glue ETL job, stores structured output in Amazon S3, and enables querying through Amazon Athena for analytical insights.

The system filters logs containing INFO and ERROR tags, extracts key metadata, and transforms unstructured log data into a structured format for SQL-based analysis.

## 🏗️ Architecture

The architecture consists of the following components:

Amazon CloudWatch Logs → Source of application logs (from Lambda function cleanupLambda)
AWS Glue ETL Job → Processes and transforms log data
Amazon S3 → Stores processed structured JSON output
AWS Glue Data Catalog → Maintains metadata for Athena queries
Amazon Athena → Enables SQL querying over processed logs
## ⚙️ Implementation Steps
1. Create S3 Bucket
Created an Amazon S3 bucket to store processed log outputs in JSON format.
This bucket serves as the data lake storage layer.
2. Create AWS Glue ETL Job
Developed an AWS Glue job using Python Shell / Spark.
The job retrieves logs from Amazon CloudWatch Logs.
3. Configure IAM Role
Configured an AWS Identity and Access Management (IAM) execution role.
Granted permissions for:
CloudWatch Logs access
S3 read/write access
AWS Glue execution permissions
4. Retrieve CloudWatch Logs
Used boto3 CloudWatch Logs client inside the Glue job.
Source log group: /aws/lambda/cleanupLambda
5. Log Parsing & Filtering
Parsed log messages and identified:
INFO logs
ERROR logs
Extracted structured fields such as:
timestamp
name_tag
message
6. Data Transformation
Converted raw logs into structured JSON format.
Example output:
{
  "timestamp": "2026-04-30 14:16:05",
  "name_tag": "ERROR",
  "message": "[ERROR] NoSuchBucket: An error occurred (NoSuchBucket) when calling the ListObjectsV2 operation..."
}
7. Store Processed Data in S3
Uploaded transformed JSON records into Amazon S3 for downstream analytics.
8. Create Glue Data Catalog Table
Registered the dataset in AWS Glue Data Catalog.
Enabled schema discovery for Athena querying.
9. Query Data Using Amazon Athena
Used Amazon Athena to query structured log data.
Example queries:
Filter logs by ERROR
Filter logs by date range
Analyze frequency of issues
## 📊 Sample Athena Query
SELECT *
FROM cloudwatch_processed_logs
WHERE name_tag = 'ERROR'
AND timestamp >= '2026-04-01'
ORDER BY timestamp DESC;
🧾 Sample Output Record
{
  "timestamp": "2026-04-30 14:16:05",
  "name_tag": "ERROR",
  "message": "[ERROR] NoSuchBucket: An error occurred (NoSuchBucket) when calling the ListObjectsV2 operation..."
}
## 📈 Result
Successfully processed CloudWatch logs using AWS Glue.
Structured logs stored in Amazon S3.
Queryable dataset created in Amazon Athena.
## 🎯 Conclusion

This project demonstrates how AWS Glue, Amazon CloudWatch, Amazon S3, and Amazon Athena can be integrated to build a fully serverless log analytics pipeline.

It enables:

Automated log ingestion
Data transformation at scale
Centralized storage
SQL-based analytics on log data
