# Youtube-data-Analysis
This project aims to leverage Amazon Web Services to create a youtube trending video analytics service. The project contains different data engineering, data analysis, and data science sections. The whole project is implemented on AWS Cloud.

# Project Goals

1. Data Ingestion - Create a data ingestion pipeline to extract new incoming data into the AWS data lake.
2. Data Lake - Create a centralized repository to store data from multiple sources and of different formats.
3. Data ETL Jobs - Create Extract, Transform, and Load jobs to preprocess raw data into usable proper versions.
4. Data Analysis - Create SQL queries to analyze data to create key insights about the product.
5. Data Reporting/Analytics - Create dashboards to answer questions and communicate insights to the Stakeholders.

# Dataset Used 
The dataset contains daily statistics for trending YouTube videos with up to 200 trending videos per day. It includes several months of data on daily trending YouTube videos. Major nations covered in this dataset are US, GB, DE, CA, JP, IN, and FR regions (USA, Great Britain, Germany, Canada, Japan, India, and France, respectively). 
* There are two parts to this dataset:
  1. Each region’s trending video data is in a separate .csv file. Data includes both numerical and categorical columns and some of them are the video title, channel title, publish time, tags, views, likes and dislikes, description, and comment count.
  2. Each video is also associated with its video category which is stored in a separate .json file that varies between regions.

Dataset link: https://www.kaggle.com/datasets/datasnaek/youtube-new

# Amazon Web Services used in this project
1. **AWS S3**: Amazon Simple Storage Service (Amazon S3) is a storage service that offers high scalability, data availability, security, and performance. It stores the data as objects within buckets and is readily available for integration with thousands of applications. An object is a file and any metadata that describes the file. A bucket is a container for objects. <br />
2. **AWS IAM**: Amazon Identity and Access Management is a service to securely control access to AWS Services. It is used to manage permissions for different roles under various scenarios. IAM is used to control who is authenticated (signed in) and authorized (has permissions) to use resources.<br />
3. **AWS Glue**: It is a scalable, serverless data integration service that can be used to discover, prepare, and combine data for application development, analytics, and machine learning. AWS Glue consolidates major data integration capabilities into a single service. It also provides DataOps tools to effortlessly author, run jobs, and implement business workflows.<br />
4. **AWS Lambda**: It is a compute service that provides a runtime environment letting you run code without provisioning or managing servers. The code is written in Lambda functions and is set to trigger as per the use case and is scaled as per the demand.<br />
5. **AWS Athena**: It is a query service that allows easy analysis of data stored directly in Amazon S3 using standard SQL. Amazon Athena also makes it easy to interactively run data analytics using Apache Spark without having to plan for, configure, or manage resources.
6. **AWS QuickSight**: It is a cloud-based Business Intelligence service that can be used to deliver easy-to-use insights and answer questions to the stakeholders. There are a lot of data integration options wherein a single data dashboard, QuickSight can include AWS data, third-party data, big data, spreadsheet data, SaaS data, B2B data, and more.

# Implementation

* **Step 1** - Ingest data into Amazon S3 buckets from Kaggle.
  The first step includes exporting data from Kaggle to Amazon S3 buckets. 
  
  <p align="center">
  <img width="550" height="100" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260062588-0d472672-0bd2-48f8-b6b2-8287538edbdf.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T195758Z&X-Amz-Expires=300&X-Amz-Signature=fe4febd56c3980a99ae19de931214dedb75df1d806c9dc12511e5a73bc9d0f7f&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696">
  <h6 align = "center" > 
  </p>

  There are two formats for each region namely csv and json files. The files are stored as S3 objects inside buckets in regions of your choice. The objects can be accessed anywhere with the help of a unique S3 URI (Uniform Resource Identifier).

* **Step 2** - Create a central repository of metadata of all the data assets in your project.
  
  <p align="center">
  <img width="550" height="150" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260061253-c0da62a5-a9ca-46d2-b171-619814ab02c5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T195956Z&X-Amz-Expires=300&X-Amz-Signature=491a13ccf81089d4c043ef94005d08e04da9db766324ce6c30a3cebc10beb284&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696">
  <h6 align = "center" > 
  </p>

  It is important to understand the structure of each data asset in your project. AWS crawler is a service that rund iteratively through each data source and infers their schema, structure and formats. It stores all this information in AWS Glue Catalog which is composed of databases and tables that provide a logical structure for storing and managing all the metadata. AWS Glue tables also store essential metadata such as column names, data types, and partition keys.

* **Step 3** - Create a AWS Lambda function that processes any new incoming data and stores it in cleansed Amazon S3 buckets.

  <p align="center">
  <img width="550" height="350" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260073386-28aaf2c7-b85c-47e6-93b5-e9516f1889ce.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T200032Z&X-Amz-Expires=300&X-Amz-Signature=329239eafc8fe6cec9ca60d4f0ba445f9cec2afe3f6cab5489fca30639bf9c3e&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696">
  <h6 align = "center" > 
  </p>

  AWS Lambda allows serverless compute functionality to run code in python (in this case) to process data according to business requirements. In our case, we will convert our data format from .json to parquet and store it new bucket. Parquet is an efficient data format than csv and json and is consistent across the data pipeline. It also creates updated data catalog in AWS Glue Catalog with updated schemas and column datatypes.

* **Step 4** - Create ETL jobs to extract data from S3 buckets, apply join transformation and load for further data analysis.

  <p align="center">
  <img width="550" height="150" src="[https://github.com/chayansraj/Youtube-video-data-analytics-using-AWS/assets/22219089/bff402b9-c8bb-4d42-a1f2-2246c83b1b3d](https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260078661-bff402b9-c8bb-4d42-a1f2-2246c83b1b3d.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T200056Z&X-Amz-Expires=300&X-Amz-Signature=5d03b287a85b13cc3ae28a29dd9e7f1c8779b5c6d9cedffcf730990fd6dec44d&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696)">
  <h6 align = "center" > 
  </p>

  Since we will not be manually processing new data every time, it is efficient to create ETL jobs that automate the data processing and data delivery task to the stakeholder. In our case, we join the two dataframes on category_id and id column and create a final cleaned data ready for analysis. The script has been uploaded in the file section.

  <p align="center">
  <img width="650" height="500" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260081013-b6764948-478c-4d54-a435-ab924c8dfea4.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T200119Z&X-Amz-Expires=300&X-Amz-Signature=11efe28d5b94fbfc92a0b787eb73aa426774f5c274aa894872881e89993597fa&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696">
  <h6 align = "center" > 
  </p>
      
* **Step 5** - Create a dashboard to visualize and answer questions according to business requirements or perform data analytics using AWS Athena.
  
    <p align="center">
  <img width="800" height="700" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22219089/260088129-1218e8f8-2a53-46e0-adb4-d6aecb8cb219.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231129T200140Z&X-Amz-Expires=300&X-Amz-Signature=7bf182a8e2e24f9b8c07557bfb22c9c2cce8b6259422e250eb6291ba44e34c24&X-Amz-SignedHeaders=host&actor_id=152231834&key_id=0&repo_id=677128696">
  <h6 align = "center" > 
  </p>

  Quicksight can be used as a business intelligence tool to deliver easy-to-understand insights to the people who you work with, wherever they are. Many other BI tools can be integrated with the final cleaned dataset. 

  
