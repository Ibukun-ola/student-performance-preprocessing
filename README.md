# Student Academic Performance Data Preprocessing Pipeline
## A Reproducible Workflow for Machine Learning Readiness
This project implements an end-to-end data preprocessing pipeline that transforms raw student academic records into a clean, structured, and machine-learning-ready dataset. It systematically identifies and resolves common real-world data quality issues; including missing values, duplicate records, inconsistent categorical formatting, and incorrect data types, that would otherwise compromise the reliability of any downstream predictive model.
The pipeline is designed as a reproducible, well-documented workflow suitable for data science students, data professionals, and recruiters seeking clean portfolio-grade code that reflects industry preprocessing standards.

## Dataset Description
The dataset was provided as part of a university coursework assignment (NUTDTS 815 — Data Exploration and Preprocessing, NUTM). Each row represents a unique student record, with the dataset containing 525 entries across 9 features spanning demographic, behavioral, and academic attributes.

## Column	Description
- Student_ID — Unique identifier assigned to each student (dropped during preprocessing)
- Age	— Chronological age of the student
- Gender	— Student gender (contained mixed casing and abbreviations: M, F, Male, Female)
- Study_Hours	— Weekly hours dedicated to studying
- Attendance	— Overall attendance rate (%)
- Previous_GPA —	Grade point average from prior terms
- Income	— Household financial background of the student
- Course	— Academic programme enrolled in (Data Science, Economics, Statistics)
- Final_Result	(Target variable) — whether the student passed or failed

## Prediction Goal 
Binary classification; predicting whether a student will Pass or Fail based on their demographic, behavioral, and academic profile.

## Raw Data Quality Issues Identified:

- Missing values in Study_Hours and Attendance
- 25 fully duplicated rows
- Age column stored as object dtype due to text entries ("Nineteen", "Twenty")
- Inconsistent category formatting in Gender and Course
- Unencoded categorical variables incompatible with ML algorithms
- Unscaled numerical features with varying magnitudes

## Pipeline Workflow
The preprocessing pipeline follows a deliberate, sequential structure where each step builds on the last:
- Load Data — Import the raw dataset and conduct an initial structural inspection
- Data Quality Assessment — Audit for missing values, duplicates, incorrect data types, inconsistent categories, outliers, and class imbalance
- Drop Identifier Column — Remove Student_ID as it carries no predictive value
- Remove Duplicates — Eliminate 25 fully duplicated rows to ensure independent observations
- Impute Missing Values — Fill missing numerical entries using median imputation
- Fix Data Types — Correct the Age column from object to integer by mapping text entries to numeric equivalents
- Standardize Categories — Resolve inconsistent formatting in Gender and Course columns
- Encode Categorical Variables — Apply explicit mapping for the target variable and one-hot encoding for nominal features
- Train/Test Split — Partition into 80% training and 20% testing sets with stratification
- Feature Scaling — Standardize continuous numerical features using StandardScaler fitted on training data only
- Validate & Export — Confirm pipeline integrity and save processed datasets to CSV and Excel formats

## Technologies Used
Tool	Purpose
Python 3	Core programming language for pipeline execution
pandas	Data manipulation, missing value handling, and tabular transformations
NumPy	Vectorized numerical operations and array transformations
scikit-learn	Feature scaling (StandardScaler), encoding, and train/test partitioning
matplotlib / seaborn	Exploratory visualizations including distribution plots and class balance charts
VS Code	Local development environment

## Repository Structure
```student-performance-preprocessing/
│
├── data/
│   ├── raw/                        # Original unmodified dataset
│   │   └── student_data_raw.csv
│   └── processed/                  # Cleaned and scaled output datasets
│       ├── X_train_scaled.csv
│       ├── X_test_scaled.csv
│       ├── y_train.csv
│       ├── y_test.csv
│       └── processed_student_dataset.xlsx
│
├── notebooks/
│   └── 01_data_preprocessing.ipynb  # Main preprocessing pipeline notebook
│
├── src/
│   └── __init__.py                 # Reserved for modular script expansion
│
├── .gitignore                      # Excludes sensitive and unnecessary files
├── requirements.txt                # Python dependency list
└── README.md                       # Project overview and usage guide


## How to Run
### 1. Clone the repository
git clone <repository-url>
cd student-performance-preprocessing

### 2. Install dependencies
pip install -r requirements.txt

### 3. Open the notebook
In VS Code, open the notebooks/ folder and run 01_data_preprocessing.ipynb
Execute cells sequentially from top to bottom
Note: Ensure the raw dataset file is placed in data/raw/ before running the notebook.

## Key Findings
- Data Quality Issues Identified: The raw dataset contained 2 missing values across Study_Hours and Attendance, 25 fully duplicated rows, 67 age entries stored as text strings ("Nineteen", "Twenty"), and inconsistent formatting across Gender and Course columns.
- Final Dataset Shape: After cleaning, the dataset was reduced from 525 to 500 rows and expanded from 9 to 11 columns following one-hot encoding. The final split produced 400 training samples and 100 testing samples (80/20).
- Class Distribution: The target variable (Final_Result) was reasonably balanced — 256 Fail and 244 Pass — eliminating the need for resampling techniques like SMOTE. Stratification was applied during splitting to preserve this balance across both subsets.
- Key Pipeline Insight: The most critical design decision was enforcing a strict train/test boundary before feature scaling. Fitting the StandardScaler exclusively on training data prevents statistical information from the test set leaking into the training process — a subtle but important safeguard against artificially inflated model performance.
- Notable Removal: Student_ID was dropped early in the pipeline as a non-predictive identifier, preventing it from being accidentally included in any transformation or modeling step.

## Transparency Note
During the development of this project, AI tooling was used as a collaborative learning aid to assist with code structuring, documentation drafting, and notebook markdown refinement. As an MSc Data Science student, I engaged with AI assistance intentionally, to explore industry-standard preprocessing practices beyond coursework scope and accelerate my exposure to professional workflows. All core analytical decisions, pipeline logic, and preprocessing steps were directed, reviewed, and verified by me to ensure accuracy and genuine understanding.

## Future Improvements
- Modular Python Script: Refactor the preprocessing pipeline into a standalone src/preprocessing.py script with reusable functions, enabling the workflow to be executed from the command line without requiring a notebook environment.
- Automated Pipeline: Explore scikit-learn Pipeline and ColumnTransformer objects to chain preprocessing steps into a single, exportable pipeline object that can be saved and reused across modeling workflows.
- Extended Exploratory Analysis: Add a dedicated EDA section with correlation heatmaps, feature distribution plots, and pairplots to deepen insight into feature relationships before modeling.
- Model Baseline: Append a lightweight modeling section that trains a simple classifier (e.g., Logistic Regression) on the processed data to validate that the preprocessing pipeline produces meaningful predictive results.
- Automated Data Validation: Integrate a schema validation step using libraries like great_expectations or pandera to programmatically enforce data quality rules at pipeline entry.
