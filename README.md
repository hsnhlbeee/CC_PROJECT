# FraudShield: Cloud-Based Fraud Detection System

A sophisticated cloud-based system for detecting fraudulent credit card transactions using Google Cloud Platform services and machine learning.

## Project Overview

FraudShield is an end-to-end fraud detection pipeline that processes credit card transactions, applies machine learning to identify potential fraud, and presents results through an interactive web dashboard. The system handles highly imbalanced data (0.172% fraud rate) and delivers real-time fraud alerts via a containerized web application.

## Features

- **Scalable Data Processing**: Process large transaction datasets using Google Cloud Dataproc and PySpark
- **Advanced ML Detection**: Implement Random Forest classification with 98% precision and 90% recall for fraud detection
- **Real-time Monitoring**: Track and visualize potential fraudulent activities as they occur
- **Interactive Dashboard**: Explore fraud patterns and insights through a Streamlit-powered interface
- **Cloud-Native Architecture**: Leverage GCP services for a fully scalable, serverless implementation

## Architecture

FraudShield implements a complete cloud-native pipeline:

1. **Data Storage & Ingestion**
   - Google Cloud Storage for raw transaction data
   - Original dataset: 284,807 transactions with 492 fraud cases (0.172%)

2. **Data Warehousing**
   - BigQuery datasets and tables for structured storage

3. **Data Processing**
   - Dataproc cluster running PySpark jobs
   - Data cleaning, normalization, and feature engineering

4. **Machine Learning**
   - Vertex AI Workbench for model development
   - Random Forest classifier optimized for precision and recall
   - Model deployment as an endpoint for real-time prediction

5. **Integration & Prediction**
   - Python integration script for new transaction processing
   - Storage of prediction results in BigQuery "Fraud Alerts" table

6. **Web Application**
   - Interactive Streamlit dashboard for visualization
   - Docker containerization
   - Deployment on Google Cloud Run

## Performance Metrics

Our fraud detection model achieved:
- **Precision**: 0.98
- **Recall**: 0.90
- **F1 Score**: 0.94
- **Accuracy**: 1.0

## Project Structure

```
.
├── Inference.ipynb       # Notebook for making predictions with the trained model
├── PySpark.ipynb         # Data processing with PySpark
├── Train_Model.ipynb     # Model training and evaluation
├── cc_project.zip        # Project archive
├── data_link.txt         # Link to the dataset
├── model.joblib          # Serialized trained model
└── README.md             # This file
```

## Getting Started

### Prerequisites

- Google Cloud Platform account with enabled services:
  - Cloud Storage
  - BigQuery
  - Dataproc
  - Vertex AI
  - Cloud Run
  - Artifact Registry
- Python 3.7+
- Docker (for local development)

### Setup Instructions

1. **Clone the repository**
   ```
   git clone [repository-url]
   cd fraudshield
   ```

2. **Set up Google Cloud environment**
   ```
   # Set your project ID
   export PROJECT_ID="your-project-id"
   gcloud config set project $PROJECT_ID
   
   # Enable required services
   gcloud services enable storage-api.googleapis.com bigquery.googleapis.com \
                         dataproc.googleapis.com aiplatform.googleapis.com \
                         run.googleapis.com artifactregistry.googleapis.com
   ```

3. **Data preparation**
   - Download the dataset from [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
   - Upload to your GCS bucket:
     ```
     gsutil cp [downloaded-file] gs://[your-bucket]/raw/creditcard.csv
     ```

4. **Run data processing**
   - Create a Dataproc cluster
   - Upload and run `PySpark.ipynb` on the cluster

5. **Train the model**
   - Create a Vertex AI Workbench instance
   - Upload and run `Train_Model.ipynb`

6. **Deploy the web application**
   - Build the Docker container:
     ```
     docker build -t gcr.io/$PROJECT_ID/fraudshield-app .
     docker push gcr.io/$PROJECT_ID/fraudshield-app
     ```
   - Deploy to Cloud Run:
     ```
     gcloud run deploy fraudshield-app \
       --image gcr.io/$PROJECT_ID/fraudshield-app \
       --platform managed \
       --allow-unauthenticated
     ```

## Usage

1. **Access the web dashboard**
   - Open the URL provided by Cloud Run after deployment
   - Explore fraud detection results and analytics

2. **Run inference on new data**
   - Use the `Inference.ipynb` notebook or the Python integration script

## Contributors

- Hasan Al-Halabi
- Ahmad Al-Hayek
- Qusay Abusalem

## Acknowledgments

- Course: Cloud Computing, Dr. Mu'nis Qasaymeh
- [Kaggle Credit Card Fraud Detection Dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

## References

1. Google Cloud Documentation. "Vertex AI." https://cloud.google.com/vertex-ai/docs
2. Google Cloud Documentation. "Dataproc." https://cloud.google.com/dataproc/docs
3. Google Cloud Documentation. "BigQuery." https://cloud.google.com/bigquery/docs
4. Google Cloud Documentation. "Cloud Run." https://cloud.google.com/run/docs
5. Apache Spark Documentation. "PySpark." https://spark.apache.org/docs/latest/api/python/
6. Streamlit Documentation. https://docs.streamlit.io/
