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

<img width="470" height="733" alt="ERmodel" src="https://github.com/user-attachments/assets/90c08c53-994e-4ed6-989b-55076573822a"/>

## Steps & Implementation

### Step 1: Setup & Configuration

#### 1.1 Install Dependencies

```
pip install boto3 pandas redshift_connector configparser
```

#### 1.2 Configure AWS Credentials
Create a ```cluster.config``` file to store AWS credentials:

```
[AWS]
KEY = your_aws_access_key
SECRET = your_aws_secret_key
```

#### 1.3 Load AWS Credentials in Python

```
import boto3
import configparser

config = configparser.ConfigParser()
config.read('cluster.config')

AWS_ACCESS_KEY_ID = config['AWS']['KEY']
AWS_SECRET_KEY = config['AWS']['SECRET']
AWS_REGION = "us-east-1"
```

### Step 2: Query Data from AWS Athena

#### 2.1 Initialize Athena Client
```
athena_client = boto3.client('athena', region_name=AWS_REGION,
    aws_access_key_id=AWS_ACCESS_KEY_ID, aws_secret_access_key=AWS_SECRET_KEY)
```

#### 2.2 Execute Athena Query
```
query = "SELECT * FROM countrycode;"

response = athena_client.start_query_execution(
    QueryString=query,
    QueryExecutionContext={"Database": "covid_19"},
    ResultConfiguration={"OutputLocation": "s3://sai-athena-output-bucket/output"},
)

query_execution_id = response["QueryExecutionId"]
print(f"✅ Query Execution Started: {query_execution_id}")
```

### Step 3: AWS Glue Crawler for Schema Detection
AWS Glue Crawler automates schema detection and creates metadata tables in the AWS Glue Data Catalog.

#### 3.1 Steps to Create a Glue Crawler
Open AWS Glue Console → Crawlers → Create Crawler.
Data Source: Select S3 bucket where the raw data is stored.
IAM Role: Assign a role with S3 read permissions.
Target Database: Create or select an existing Glue database.
Run the Crawler: It will scan the files in S3, infer schema, and create tables.
#### 3.2 Verify Schema in AWS Glue Data Catalog
After the crawler runs successfully, check the tables in AWS Glue Console under the Data Catalog.

### Step 4: Automating Transformation with AWS Glue Job
AWS Glue Job automates data transformation and loading into Redshift.

<img width="926" height="682" alt="dimensional_model" src="https://github.com/user-attachments/assets/05ddd0c2-e581-40fc-b2c9-26323fe1d440" />

#### 4.1 Create AWS Glue Job
Navigate to AWS Glue Console → Jobs → Create Job.
Select Python Shell as the execution environment.
Upload external libraries (Redshift Connector) to S3.
Configure Glue Job Parameters:
Script location: S3 bucket.
IAM Role: Ensure it has S3 read/write and Redshift access.
Python version: Use Python 3.
## 4.2 AWS Glue Python Script
```
import redshift_connector

conn = redshift_connector.connect(
    host=Give End point,
    port=5439,
    database="dev",
    user="awsuser",
    password="Passw0rd123"
)

cursor = conn.cursor()
cursor.execute("COPY dev.public.dimDate FROM 's3://sai-covid19-project/output/dimDate.csv' IAM_ROLE 'arn:aws:iam::503561422673:role/redshift-s3-access' FORMAT AS CSV DELIMITER ',' IGNOREHEADER 1 REGION 'us-east-1';")
print("✅ Data Loaded into Redshift")
```

### Step 5: Power BI Integration with Amazon Redshift

#### 5.1 Connecting Power BI to Redshift
Open Power BI → Get Data.
Select Amazon Redshift as the data source.
Enter the Redshift cluster endpoint, database name, username, and password.
Load the data and create Power BI reports.

### 5.2 Create Interactive Reports
KPIs: Total cases, deaths, recoveries.
Time-Series Analysis: COVID-19 trends over time.
Geospatial Visualizations: Impact by region.

## Debugging & Troubleshooting
Error: Redshift Connection Failed

- **Verify IAM Role permissions.**
    Check Redshift Security Group settings.
    Ensure correct database credentials.
    Error: COPY Command Fails
- **Check if S3 bucket path is correct.**
    Ensure the IAM Role has Amazon S3 read access.
    Verify CSV file formatting.
    Error: Power BI Connection Issues
- **Ensure Redshift cluster is publicly accessible.**
    Check Power BI firewall settings.
    Validate Redshift connection string.
  
## Future Enhancements
🔹 **Implement Machine Learning Models** on Redshift data for forecasting.<br>
🔹 **Optimize ETL Performance** with AWS Glue crawlers and workflows.<br>
🔹 **Deploy Real-Time Analytics** using AWS Kinesis.<br>
🔹 **Automate Power BI Dashboards** using scheduled refresh.

## Next Steps
- Explore **AWS Data Lake Formation**.<br>
- Implement **CI/CD for ETL pipelines**.<br>
- Enhance **Power BI reporting** with predictive analytics.
