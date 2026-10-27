# churn-model-deployment-pipeline
Churn Model Deployment Pipeline

A machine learning project that trains, tests, and deploys a telecom customer churn prediction model through a REST API using Scikit-learn, FastAPI, Pydantic, and Docker.

Project Overview

Telecom companies lose revenue when customers discontinue their subscriptions. This project builds a machine learning pipeline that predicts whether a customer is likely to churn and exposes the prediction through an API that other applications can call.

The pipeline is designed to handle missing values, numeric charges stored as text, categorical features, and previously unseen categories. It also supports configurable prediction thresholds and basic API authentication.

Objectives

- Train a customer churn classification model.
- Build a reproducible preprocessing and prediction pipeline.
- Evaluate the model using classification metrics.
- Deploy the model using FastAPI.
- Provide single-customer and batch prediction endpoints.
- Validate incoming requests with Pydantic.
- Protect prediction endpoints using API-key authentication.
- Containerise the application with Docker.
- Add automated tests and model documentation.

Technology Stack

Technology| Purpose
Python| Core programming language
Pandas and NumPy| Data processing
Scikit-learn| Preprocessing, training, and evaluation
FastAPI| REST API
Pydantic| Request validation
Joblib| Model persistence
Pytest| Automated testing
Docker| Containerisation

Project Structure

churn-model-deployment-pipeline/
├── app/
│   ├── __init__.py
│   └── main.py
├── src/
│   ├── __init__.py
│   └── train.py
├── tests/
│   └── test_api.py
├── data/
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── artifacts/
│   ├── churn_pipeline.joblib
│   └── metadata.json
├── requirements.txt
├── Dockerfile
├── .gitignore
└── README.md

Dataset

The dataset contains customer demographics, subscription information, service usage, billing details, and a "Churn" target column.

Installation and Setup

1. Clone the repository

git clone https://github.com/kunjansharma008/churn-model-deployment-pipeline.git
cd churn-model-deployment-pipeline


2. Create a virtual environment

python -m venv .venv

Activate it on Windows:

.venv\Scripts\Activate.ps1

3. Install dependencies

pip install -r requirements.txt

4. Train the model

After placing the dataset in "data/", run:

python -m src.train

The script saves the trained pipeline and evaluation metadata inside "artifacts/".

5. Start the API

Set a development API key in PowerShell:

$env:API_KEY="your-strong-secret-key"

Run the application:

uvicorn app.main:app --reload

Open the interactive API documentation at:

http://127.0.0.1:8000/docs

API Endpoints

Method| Endpoint| Purpose
GET| "/health"| Check application and model status
POST| "/predict"| Predict churn for one customer
POST| "/predict/batch"| Predict churn for multiple customers

Prediction endpoints require an "X-API-Key" request header.

Example prediction request

{
  "features": {
    "gender": "Female",
    "SeniorCitizen": 0,
    "Partner": "Yes",
    "Dependents": "No",
    "tenure": 12,
    "PhoneService": "Yes",
    "MonthlyCharges": 70.35,
    "TotalCharges": "844.20"
  }
}

Include the other feature columns expected by the trained model when testing with the actual dataset.

Example response

{
  "churn_probability": 0.72,
  "churn_prediction": "Yes",
  "threshold": 0.35
}

The response above is illustrative, not a measured prediction from a trained model.

Model Evaluation

The training script reports the following metrics:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

The classification threshold affects the trade-off between missing customers who will churn and contacting customers who will stay. The selected threshold should be justified using business costs and validation data rather than accuracy alone.

Testing

Run the automated tests using:

pytest -v

Tests should cover request validation, authentication, health checks, predictions, batch requests, and missing-model behaviour. Record the actual test results before reporting the final test count.

Docker

Build the image:

docker build -t churn-api .

Run the container:

docker run --rm -p 8000:8000 -e API_KEY="your-strong-secret-key" churn-api

Ensure that the trained model artifact is included in the Docker build context before starting the container.

Security Considerations

- Use a strong API key outside local development.
- Never commit secrets, credentials, or sensitive customer data.
- Validate request data before inference.
- Avoid logging personal customer information.
- Restrict access to model artifacts and deployment credentials.

Future Improvements

- Cross-validation and hyperparameter tuning.
- Cost-sensitive threshold optimisation.
- Model and dataset versioning.
- Prediction latency benchmarking.
- Population Stability Index (PSI) drift monitoring.
- Continuous integration and automated deployment.
- Production-grade authentication and secret management.

Project Status

This repository provides the implementation structure for a churn model deployment pipeline. Training metrics, test results, latency measurements, and Docker image size must be generated and verified by running the implementation.

Author

Kunjan Sharma

Machine Learning Engineer Internship Project

License

This project is intended for educational and internship purposes. Add an appropriate open-source license if you plan to distribute the code publicly.
