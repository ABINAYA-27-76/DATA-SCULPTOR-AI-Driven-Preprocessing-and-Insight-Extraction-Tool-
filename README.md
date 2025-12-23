# DATA-SCULPTOR-AI-Driven-Preprocessing-and-Insight-Extraction-Tool
## 1. Introduction

DATA SCULPTOR is an intelligent, end-to-end data preprocessing and analytics web application built using Flask.
It automates data cleaning, outlier detection, statistical insight extraction, and data visualization, enabling users to transform raw datasets into actionable intelligence with minimal manual effort.

The system follows a modular, AI-ready architecture, allowing seamless integration of advanced machine learning and predictive analytics in future extensions.

## 2. Statement of the Problem

### Raw datasets often contain:

Missing values

Duplicate records

Outliers

Inconsistent formats

These issues reduce analysis accuracy and increase preprocessing time.
Manual data preparation is time-consuming, error-prone, and requires technical expertise.

## 3. Purpose of the Project

### The goal of DATA SCULPTOR is to:

Automate essential data preprocessing steps

Extract meaningful statistical insights

Identify anomalies in datasets

Provide clear visual representations

Reduce dependency on manual coding for analytics

## 4. Features Overview
### Home Page

Dataset upload interface

Trigger analysis pipeline

Navigate insights and visual outputs

Clean, minimal UI for non-technical users

## Automated Data Preprocessing

Handles essential data preparation tasks automatically.

Key Capabilities

Handling missing values

Removing duplicate records

Standardizing column formats

Detecting invalid date values

## AI-Driven Insight Extraction

Generates descriptive and statistical insights from cleaned data.

Insights Generated

Dataset shape (rows & columns)

Summary statistics (mean, median, min, max)

Column-wise numeric distributions

Correlation indicators

## Outlier Detection System

Identifies anomalous values that may affect data quality.

Techniques Used

Statistical thresholding

Interquartile Range (IQR) method

Numeric feature scanning

## Data Visualization Engine

Automatically generates visual analytics.

Visual Outputs

Correlation heatmaps

Time-based trend plots

Numeric feature trends

## 5. AI-Ready Architecture

DATA SCULPTOR is designed for scalability.

Modular utility files (utils/)

Independent preprocessing and analysis functions

Ready for ML model integration:

Classification

Regression

Clustering
# Installation & Setup:
1️⃣ Clone the Repository
```
git clone https://github.com/ABINAYA-27-76/DATA-SCULPTOR.git
cd DATA-SCULPTOR

```
2️⃣ Create a Virtual Environment
```
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows
```
3️⃣ Install Dependencies

Ensure you have file containing:

flask
pandas
numpy
matplotlib
seaborn
#### Install with:
```
pip install -r requirements.txt
```
4️⃣ Run the Application
```
python app.py
```
# program:
```
app py

from flask import Flask, render_template, request, redirect, url_for
import pandas as pd
from werkzeug.utils import secure_filename
import os
import sys

# Extend sys path to access custom utils
sys.path.append(os.path.join(os.path.dirname(__file__), 'utils'))

# Import modular AI-driven preprocessing and analysis functions
from utils.data_preprocessing import clean_data
from utils.data_insights import generate_insights
from utils.data_visualization import save_heatmap, save_trend_plot
from utils.data_outliers import detect_outliers


# Initialize Flask app
app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = 'uploads'
app.config['IMAGE_FOLDER'] = 'static/images'

# Ensure required folders exist
os.makedirs(app.config['UPLOAD_FOLDER'], exist_ok=True)
os.makedirs(app.config['IMAGE_FOLDER'], exist_ok=True)


@app.route('/')
def index():
    """Render homepage for Datasculptor"""
    return render_template('index.html', title="DATA SCULPTOR – AI Driven Tool")


# @app.route('/analyze', methods=['POST'])
# def analyze():
#     """Process uploaded file and analyze using AI-driven modules"""
#     # Handle file upload
#     if 'file' not in request.files or request.files['file'].filename == '':
#         return "⚠️ Please upload a valid dataset file.", 400

#     file = request.files['file']
#     filename = secure_filename(file.filename)
#     filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
#     file.save(filepath)

#     # Load dataset
#     try:
#         df = pd.read_csv(filepath)
#     except Exception:
#         return "Error reading CSV. Ensure your file contains valid data.", 500

#     # Step 1: Preprocessing & Data Cleaning
#     df, missing, missing_percent, duplicates, invalid_dates = clean_data(df)

#     # Step 2: AI-Driven Insight Extraction
#     insights = generate_insights(df)

#     # Step 3: Outlier Detection
#     total_outliers, outlier_counts = detect_outliers(df)

#     # Step 4: Visualization
#     heatmap_file = save_heatmap(df)
#     trend_plots = {
#         col: save_trend_plot(df, col) for col in df.select_dtypes(include=['number']).columns
#     }

    

#     return render_template('analyze.html',
#                            title="Analysis Results – DATA SCULPTOR",
#                            shape=df.shape,
#                            columns=df.columns.tolist(),
#                            missing=missing,
#                            missing_percent=missing_percent,
#                            duplicates=duplicates,
#                            invalid_dates=invalid_dates,
#                            insights=insights,
#                            total_outliers=total_outliers,
#                            outlier_counts=outlier_counts,
#                            heatmap_file=heatmap_file,
#                            trend_plots=trend_plots,
#                            )


# if __name__ == '__main__':
#     app.run(debug=True)


@app.route('/analyze', methods=['POST'])
def analyze():
    """Process uploaded file and analyze using AI-driven modules"""
    if 'file' not in request.files or request.files['file'].filename == '':
        return "⚠️ Please upload a valid dataset file.", 400

    file = request.files['file']
    filename = secure_filename(file.filename)
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)

    try:
        df = pd.read_csv(filepath)
    except Exception:
        return "Error reading CSV. Ensure your file contains valid data.", 500

    # Preprocessing & Cleaning
    df, missing, missing_percent, duplicates, invalid_dates = clean_data(df)

    # AI Insights
    insights = generate_insights(df)

    # Outliers
    total_outliers, outlier_counts = detect_outliers(df)

    # Visualization
    heatmap_file = save_heatmap(df)
    trend_plots = {}
    for col in df.select_dtypes(include=['number']).columns:
        trend_plots[col] = save_trend_plot(df, col)

    return render_template(
        'analyze.html',
        title="Analysis Results – DATA SCULPTOR",
        shape=df.shape,
        columns=df.columns.tolist(),
        missing=missing,
        missing_percent=missing_percent,
        duplicates=duplicates,
        invalid_dates=invalid_dates,
        insights=insights,
        total_outliers=total_outliers,
        outlier_counts=outlier_counts,
        heatmap_file=heatmap_file,
        trend_plots=trend_plots,
    )

if __name__ == '__main__':
    app.run(debug=True)
```
## UI Highlights

Clean, minimal web-based interface

Simple dataset upload workflow

Automated analytics without coding

Visual outputs rendered instantly

Modular backend for easy upgrades
## Interface Preview:

### dataset information:
![WhatsApp Image 2025-12-23 at 9 41 41 PM](https://github.com/user-attachments/assets/d8799cd1-865e-4311-8a3a-aa08a1375181)
### AI-driven insigths:
![WhatsApp Image 2025-12-23 at 9 43 05 PM](https://github.com/user-attachments/assets/8de3d62e-1203-4985-a730-f7d4de1fc649)
### Correlation heatmap:
![WhatsApp Image 2025-12-23 at 9 43 12 PM](https://github.com/user-attachments/assets/f3b41be6-90fb-4aa5-a618-91d3c255d2a9)
### Trend plots:
![WhatsApp Image 2025-12-23 at 9 43 15 PM](https://github.com/user-attachments/assets/370f00a4-ee55-4884-a73a-82d3747e62d2)


## Future Enhancements:

 Machine Learning model integration

 Predictive analytics dashboards

 Auto feature engineering

 Cloud deployment (AWS / IBM Cloud)

 Downloadable insight reports (PDF)

