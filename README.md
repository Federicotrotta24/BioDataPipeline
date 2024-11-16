# BioDataPipeline


## Overview
This project aims to process health and demographic data from the BRFSS dataset, leveraging a robust pipeline for data cleaning, transformation, dimensionality reduction using PCA (Principal Component Analysis), and storing the processed data in Amazon Redshift. The pipeline is designed for scalability and reproducibility, utilizing Docker for containerization.


## Description of Each Step in the Pipeline
1. Data Ingestion
  - Connects to an Amazon S3 bucket using PySpark to read raw data in formats like CSV, Parquet, or JSON. The data is then saved locally in the data/raw/ directory for further processing.

2. Data Cleaning
  - Handles missing values through imputation or removal.
  - Normalizes formats (e.g., unit conversions and standardizing categorical data).
  - Identifies and processes outliers based on predefined rules.



3. Data Transformation
 - Variable Selection: Filters the dataset to include only variables present consistently across all years or datasets, ensuring comparability.
 - Column-Specific Fixes:
   - Weight (weight): Converts coded weight values into a standard format (e.g., kilograms or pounds).
   - Height (height): Processes height data by interpreting coded values and converting them into a standard metric (e.g., meters or centimeters).
   - BMI (bmi): Standardizes BMI values by handling codes for missing, refused, or invalid entries (e.g., -9999 or BLANK).
- Scales and normalizes the data using PySpark's StandardScaler to prepare it for PCA.
- Encodes categorical variables into numerical values using PySpark's built-in encoding methods.


4. Dimensionality Reduction with PCA
  Purpose: Reduce the dataset's complexity while preserving critical information.
  - Uses PySpark's PCA library to compute principal components.
  - Determines the optimal number of components based on the explained variance ratio (e.g., 95% of variance).
  - Transforms the data into its reduced-dimensional representation.

5. Data Storage in Amazon Redshift
  - Establishes a connection to Amazon Redshift using configuration file credentials.
  - Connect with PowerBI for easy plotting 
  - Uploads the transformed dataset to a specified schema and table.

## Features
Data Cleaning: Handles missing values, outliers, and inconsistent data formats.
Dimensionality Reduction: Applies PCA to reduce dataset complexity while retaining essential variance.
Integration with Amazon Redshift: Stores processed data for further analysis and visualization.


## Steps to Run the Pipeline
1. Clone the Repository
Begin by cloning the project repository:
```
git clone https://github.com/Federicotrotta24/rBioDataPipeline.git
cd BioDataPipeline
```



2. Build the Docker Container
Ensure Docker is installed, then build the container:
```
docker build -t data_pipeline .
```
3. Run the Pipeline
```
Run the pipeline using the following command:
docker run -v $(pwd)/data:/app/data -e CONFIG_PATH=/app/config.yaml data_pipeline
```

This command:
- Connects to the S3 bucket to load raw data.
- Processes data with PySpark for cleaning, transformation, and PCA.
- Loads the processed data into Amazon Redshift.


## Requirements
Software
- Python 3.8 or higher
- PySpark
- Docker
- Amazon S3 and Redshift credentials



## Acknowledgements
This project uses data from the Behavioral Risk Factor Surveillance System (BRFSS). Special thanks to open-source tools such as PySpark, Docker, Pandas, Scikit-learn, and Amazon Redshift for enabling this project.



