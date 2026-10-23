# AI Project Fraud Detection

This project is a credit card fraud detection system built with Python, machine learning, Cassandra, and a Flask API. It reads transaction data, trains multiple ML pipelines, evaluates model performance, and exposes a prediction endpoint for new transactions.

## Overview

The project uses a dataset similar to a credit card fraud dataset (`credit.csv`) with transaction features such as `Time`, `Amount`, and PCA-transformed variables (`V1` to `V28`). A model is trained to classify transactions as fraudulent or legitimate based on the label `C`.

The workflow is implemented across several files:

- `main.py`: loads data into Cassandra, creates the ML pipeline, trains the model, and evaluates it.
- `ml_model.py`: builds and tunes the machine learning pipeline using `GridSearchCV`.
- `TxInfo.py`: defines the Cassandra table schema for training data.
- `PredictTxInfo.py`: defines the Cassandra schema for prediction results.
- `cassandra_rw.py`: handles Cassandra synchronization, CSV imports, DataFrame reads, and JSON writes.
- `flask_ml_api.py`: exposes a Flask API endpoint to predict a transaction and store prediction results.

## Features

- Cassandra-based storage for transaction records
- CSV-to-database import pipeline
- DataFrame conversion from Cassandra to pandas
- Machine learning pipeline including:
  - `StandardScaler` or `MinMaxScaler`
  - `SMOTE` or `SMOTEENN`
  - `SGDClassifier`, `RandomForestClassifier`, and `SVC`
- Hyperparameter tuning using `GridSearchCV`
- Fraud prediction endpoint via Flask

## Model Training Pipeline

The project builds several pipelines by combining:

- A scaler: `standardscaler` or `minmaxscaler`
- A sampler: `smote` or `smoteenn`
- An estimator: `sgdClassifier`, `randomForestClassifier`, or `svc`

The training process in `main.py`:

1. Connects to Cassandra
2. Syncs the transaction table schema
3. Loads the credit dataset into the database
4. Retrieves the data as a pandas DataFrame
5. Splits the data into train/test sets
6. Fits the model pipeline
7. Evaluates the classifier with a confusion matrix

## API

The Flask API in `flask_ml_api.py` provides a POST endpoint:

- `/predict/tx`

It expects a JSON transaction payload, predicts the label, writes the result to Cassandra, and returns the same data with the prediction included.

## Example Workflow

```bash
python main.py
python flask_ml_api.py
```

Then send a transaction payload to the API, for example:

```json
{
  "Time": 0,
  "V1": -1.359807,
  "V2": -0.072781,
  "V3": 2.536347,
  "V4": 1.378155,
  "V5": -0.338321,
  "V6": 0.462388,
  "V7": 0.239599,
  "V8": 0.098698,
  "V9": 0.363787,
  "V10": 0.090794,
  "V11": -0.551599,
  "V12": -0.617801,
  "V13": -0.991390,
  "V14": -0.311169,
  "V15": 1.468177,
  "V16": -0.470401,
  "V17": 0.207971,
  "V18": 0.025791,
  "V19": 0.403993,
  "V20": 0.251412,
  "V21": -0.018307,
  "V22": 0.277838,
  "V23": -0.110474,
  "V24": 0.066928,
  "V25": 0.128539,
  "V26": -0.189115,
  "V27": 0.133558,
  "V28": -0.021053,
  "Amount": 149.62
}
```

## Dependencies

This project relies on Python packages such as:

- pandas
- scikit-learn
- imbalanced-learn
- cassandra-driver
- Flask
- joblib

## Author

Created by: Sebastian

