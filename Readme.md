# AzureML Pipeline for luxury and non-luxury vehicles Price Prediction

This repository contains an end-to-end Machine Learning Operations (MLOps) pipeline designed to automate the preparation, training, and deployment of a machine learning model for predicting luxury and non-luxury vehicles prices. The solution uses Microsoft Azure Machine Learning services, with full integration of CI/CD principles and model management through MLflow.

## Table of Contents

1. [Problem Statement](#problem-statement)
2. [Objective](#objective)
3. [Dataset Description](#dataset-description)
4. [Solution Architecture](#solution-architecture)
5. [Pipeline Components](#pipeline-components)
6. [Technologies Used](#technologies-used)
7. [Setup Instructions](#setup-instructions)
8. [Execution](#execution)
9. [Outputs](#outputs)
10. [Notes](#notes)

---

## Problem Statement

An automobile dealership in Los Vegas specializes in selling both luxury and non-luxury vehicles. However, due to manual and disjointed pricing methods, they face challenges such as:

* Inconsistent and error-prone evaluations
* Delayed updates across systems
* Difficulty in scaling operations using manual workflows

These inefficiencies negatively affect model performance, pricing accuracy, and customer trust. The dealership seeks an automated, reliable, and scalable system for model-based pricing.

## Objective

As an MLOps engineer, your task is to design and implement an automated MLOps pipeline using Azure Machine Learning that delivers:

* Data preprocessing and transformation
* Model training and hyperparameter tuning
* Model performance logging and registration
* Model version management and deployment readiness

The goal is to improve pricing accuracy, handle evolving conditions, and scale effortlessly with demand.

## Dataset Description

The dataset contains attributes of used cars such as:

| Feature             | Description                            |
| ------------------- | -------------------------------------- |
| `Segment`           | Luxury or non-luxury classification    |
| `Kilometers_Driven` | Total kilometers driven by the vehicle |
| `Mileage`           | Fuel efficiency (km/l)                 |
| `Engine`            | Engine capacity (cc)                   |
| `Power`             | Engine power (BHP)                     |
| `Seats`             | Number of seats                        |
| `Price`             | Selling price (in lakhs)               |

This dataset is crucial for training a model that predicts prices based on key specifications.

## Solution Architecture

The solution uses AzureML to orchestrate a pipeline with the following stages:

1. **Data Preparation**
2. **Model Training**
3. **Hyperparameter Tuning**
4. **Model Registration**

All jobs are run on a reusable AzureML compute cluster, and the environment is containerized using a Conda-based configuration.

## Pipeline Components

### 1. Data Preparation (`data_prep`)

* Encodes categorical features
* Splits data into train and test sets
* Logs dataset metrics through MLflow

### 2. Model Training (`model_train`)

* Trains a Random Forest Regressor
* Evaluates performance via Mean Squared Error (MSE)
* Logs metrics and saves model as an MLflow artifact

### 3. Hyperparameter Tuning (`sweep_job`)

* Performs randomized hyperparameter search
* Minimizes MSE using multiple trials

### 4. Model Registration (`model_register`)

* Registers best-performing model to the MLflow registry
* Version controls the model for production readiness

## Technologies Used

* **Azure Machine Learning**
* **Python**
* **MLflow**
* **Scikit-learn**
* **Pandas, NumPy**
* **AzureML SDK v2**

## Setup Instructions

1. Clone the repository and navigate to the project directory.
2. Install the required dependencies (see `train-conda.yml`).
3. Create or use an existing AzureML workspace and compute cluster.
4. Upload the dataset (`used_cars.csv`) as an Azure ML data asset.
5. Configure the environment using the provided Conda configuration file.

## Execution

1. Submit the full pipeline using the AzureML SDK:

```python
pipeline_job = ml_client.jobs.create_or_update(pipeline_instance)
ml_client.jobs.stream(pipeline_job.name)
```

2. Monitor the pipeline progress in AzureML Studio.

3. Retrieve output artifacts, trained models, and logs upon completion.

## Outputs

* Preprocessed train and test datasets
* Best-trained model registered in MLflow
* Hyperparameter search logs
* Performance metrics (MSE)

## Notes

* The entire flow is designed to be reproducible and scalable.
* Use `ml_client.environments.create_or_update()` for environment versioning.
* Registered models can later be deployed as REST endpoints or integrated into downstream apps.

---
