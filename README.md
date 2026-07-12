# Banking-Loan-Data-Lake-Pipeline
This project demonstrates classic batch of ETL (Extract, Transform &amp; Load)

This project builds an end-to-end ETL pipeline using Amazon S3, AWS Glue, Amazon Redshift, AWS Athena and Power BI for reporting. 

The objective is to automate the extraction, transformation, and loading (ETL) of COVID-19 data into Amazon Redshift, enabling seamless analytics and visualization.

<img width="1022" height="610" alt="Architecture" src="https://github.com/user-attachments/assets/2bab9b73-b115-4001-a8d7-9333873050b8" />

## Workflow Steps:

1. **Data Extraction**: Query raw data stored in Amazon S3 using AWS Athena.
2. **Data Crawling & Schema Detection**: Use AWS Glue Crawler to detect schema.
3. **Data Transformation**: Process and clean data using Pandas in Jupyter Notebook.
4. **AWS Glue Job Execution**: Automate transformation and data movement to Redshift.
5. **Loading into Amazon Redshift**: Use the COPY command to efficiently transfer the transformed data from S3 to Redshift.
6. **Data Visualization**: Connect Power BI to Redshift to generate interactive reports.

## Tech Stack & AWS Services

- **AWS Athena** – Serverless SQL queries on data in Amazon S3.
- **Amazon S3** – Storage for raw and processed datasets.
- **AWS Glue Crawler** – Automatically detects schema and creates metadata tables.
- **AWS Glue Job** – ETL automation and job scheduling.
- **Amazon Redshift** – Data warehouse for analytics and reporting.
- **Power BI** – Visualization tool to create dynamic dashboards from Redshift data.
- **Python & Pandas** – Data processing and schema extraction.
- **Boto3** – AWS SDK for programmatic access.
