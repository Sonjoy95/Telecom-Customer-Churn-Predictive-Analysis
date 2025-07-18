# Customer Churn Predictive Analysis: From Machine Learning to Business Intelligence (PowerBI + Python)

## 1. Project Overview

This project is a comprehensive **Telecom Customer Churn Predictive Analysis solution** that integrates the power of **Python for machine learning and predictive modeling** with **PowerBI for interactive data visualization and business intelligence**. The core objective is to accurately predict customer churn, providing actionable insights that can be consumed and analyzed by business stakeholders. This document details the complete end-to-end process, from model development to dashboard creation.

## 2. Business Problem

Customer churn represents a critical challenge for businesses globally, directly impacting revenue and growth. The cost of acquiring a new customer is often substantially higher than the cost of retaining an existing one. Unpredicted churn leads to lost revenue, decreased customer base stability, and impacts long-term growth. This project's primary goal was to develop a robust predictive model that provides early warnings of potential churn, enabling timely and effective intervention. The output of this predictive model is then designed to feed into a Business Intelligence (BI) dashboard for comprehensive analytical capabilities.

## 3. Dataset

The analysis utilizes a comprehensive telecom customer dataset. Key characteristics of the dataset include:
* **Numerical Features:** Such as `tenure` (how long the customer has been with the company), `MonthlyCharges`, and `TotalCharges`.
* **Categorical Features:** A significant number (17 in total), including binary attributes like `SeniorCitizen`, `Gender`, `Partner`, and various service-related features.
* **Target Variable:** `Churn` (binary: 'Yes' or 'No'), indicating whether the customer churned within a specific period.
* **Class Imbalance:** The dataset exhibits a typical class imbalance, with a larger proportion of non-churning customers.

## 4. Methodology & Key Steps

The project followed a systematic machine learning and business intelligence workflow:

1.  **Data Loading & Initial Exploration:** Understanding dataset structure, data types, and initial statistics.
2.  **Data Preprocessing & Feature Engineering:**
    * **Missing Value Imputation:** Handled 11 missing `TotalCharges` values by imputing `0` for new customers (`tenure = 0`), maintaining logical consistency.
    * **Target Encoding:** Transformed the 'Churn' target variable from string labels ('No'/'Yes') to numerical (0/1).
    * **Numerical Feature Transformation:** Applied power transformations (Yeo-Johnson for `tenure`, `TotalCharges`, and `SeniorCitizen` was later treated as categorical; Box-Cox for `MonthlyCharges`) to address skewness and improve distributional properties for certain models (creating the 'Z' dataset).
    * **Categorical Feature Handling:** Prepared categorical features for model-specific processing.
3.  **Class Imbalance Handling:** Applied `scale_pos_weight` to all boosting models to explicitly address the imbalance in the `Churn` target variable, prioritizing the correct identification of the minority class.
4.  **Model Selection & Benchmarking:** Explored state-of-the-art gradient boosting algorithms: Random Forest, XGBoost, LightGBM, and CatBoost. Each model was first evaluated with baseline parameters to establish initial performance.
5.  **Hyperparameter Tuning:** Rigorous hyperparameter tuning was performed using `RandomizedSearchCV` with cross-validation. This iterative process involved:
    * Defining broad parameter distributions for initial searches.
    * Refining ranges based on optimal parameters found.
    * Optimizing for `ROC-AUC` and `F1-Score` (for the minority class) to ensure robust performance.
6.  **Model Evaluation & Comparison:** Models were comprehensively evaluated on unseen test data using key metrics: `Accuracy`, `F1-Score`, `ROC-AUC Score`, and `Train-Test Gap Analysis`.
7.  **Threshold Optimization:** Explored adjusting the classification probability threshold to fine-tune the balance between Precision and Recall for the 'Churn' class, aligning with specific business objectives.
8.  **Prediction Export for BI:** The best model's predictions (probabilities and classifications) were exported to a CSV file, integrated with original customer features (`customerID`, demographics, service details), to serve as the data source for the PowerBI dashboard.
9.  **PowerBI Dashboard Development:** Designed and built an interactive dashboard in PowerBI, leveraging various visualization types to present churn insights effectively.

## 5. Key Findings & Best Model

Through systematic exploration, rigorous tuning, and a keen eye on real-world applicability, the **Refined Tuned XGBoost Classifier on the Transformed Numerical Dataset** was chosen as the ultimate best predictive model for this project.

**Rationale for Best Model Selection:**

* **Peak F1-Score (with Optimization):** Achieved an outstanding F1-Score of **0.6371** for the 'Churn' class when the classification threshold was optimally set to 0.60. This demonstrates a superior and business-aligned balance between minimizing missed churners (high Recall) and reducing unnecessary interventions (improved Precision).
* **Strong Discriminative Power:** Maintained a high ROC-AUC score of **0.8463**, indicating its excellent ability to correctly rank customers by their likelihood of churning across various thresholds.
* **Superior Generalization:** Exhibited very small train-test gaps across all metrics, confirming its robustness and reliability on unseen data.
* **Exceptional Computational Efficiency:** A critical factor for practical deployment, XGBoost proved significantly faster during the iterative tuning process (2-3x faster than LightGBM, 9-10x faster than CatBoost). This efficiency enables quicker model iterations and faster insights delivery.

**Performance Summary (Best Model - Refined Tuned XGBoost on Test Set):**

| Metric                     | Score  |
| :------------------------- | :----- |
| **Accuracy** | 0.7445 (0.7793 at Threshold 0.60)|
| **F1-Score (Class 1 - Churn)** | 0.6281 (0.6371 at Threshold 0.60)|
| **ROC-AUC Score** | 0.8463 |

## 6. PowerBI Dashboard Structure & Visualizations

The PowerBI dashboard is designed across multiple pages to provide both high-level overviews and detailed analytical capabilities:

* **Initial Page: Introduction:** Presents the project title, goals, target audience.
* **Page 1: Churn Overview**
    * KPI cards: Overall churn rates (Actual & Predicted), Total Customers, Actual Churners, Predicted Churners.
    * Slicer: `Tenure Category` (e.g., 0-1 Year, 1-2 Years).
    * Bar Charts: Actual vs. Predicted Churn Counts; Churn Rate by Gender.
    * Histogram: Distribution of `Predicted Churn Probability`.
    * Donut Chart: Churn Rate by `Contract` Type.
    * Scatter Plots: `Monthly Charges` vs. `Predicted Churn Probability`; `Tenure` vs. `Predicted Churn Probability` (with 0.60 threshold line).
* **Page 2: Churn Segment Analysis:**
    * Slicers: `Gender`, `Internet Service`, `Phone Service`, `Payment Method`, `Contract`.
    * Matrix Visuals: Confusion Matrix; Classification Metrics (Precision, Recall, F1-Score, Accuracy).
    * Scatter Plot: `Total Charges` vs. `Predicted Churn Probability`.
    * Table: Top 100 High Risk Customers.
* **Page 3: Segmented Churn Patterns**
    * Mtrix Visual: Churn Risk Matrix
    * Small Multiples (Column Chart): `Predicted Churn Rate` vs. `Tenure Cohort`, broken down by `Gender` and `Senior Citizen` (Small Multiples) and `InternetService` (Legend).
    * 100% stacked column chart: Predicted Churners by `Contract` and `InternetService`
    * Area Chart: Churn Probability by `Tenure Cohort`

## 7. Conclusion & Business Impact

This project successfully developed a high-performing, computationally efficient, and user-friendly solution for predicting customer churn. By bridging the gap between advanced machine learning in Python and interactive business intelligence in PowerBI, this solution empowers businesses to:

* **Proactively Drive Retention:** Identify high-risk customers with precision and timeliness.
* **Optimize Resource Allocation:** Target retention efforts more effectively, reducing wasted spend.
* **Gain Actionable Insights:** Understand the characteristics of churning segments through interactive dashboards.
* **Monitor Performance:** Continuously track churn trends and model effectiveness.

This comprehensive approach transforms raw data into actionable intelligence, significantly contributing to customer retention strategies and sustained business growth.

## 8. Project Details & How to Run

* **Development Period:** June 30, 2025 - July 17, 2025
* **Languages:** Python, DAX
* **Libraries:**
    * **Python:** 3.10.2
    * **Pandas:** 2.2.3
    * **NumPy:** 2.2.6
    * **Scikit-learn:** 1.6.1
    * **XGBoost:** 3.0.2
    * **LightGBM:** 4.6.0
    * **CatBoost:** 1.2.8
    * **Matplotlib:** 3.10.3
    * **Seaborn:** 0.13.2
    * **SciPy:** 1.15.3
    * **Jupyter Notebook:** 7.4.3 (for interactive development and analysis)
* **Tools:** Jupyter Notebook, PowerBI Desktop
* **Setup:**

    To ensure the project runs correctly and to avoid dependency conflicts with other Python projects on your system, it's highly recommended to use a virtual environment with **Python 3.10.2**.

    **Important Pre-requisite:**
    Before proceeding, verify that you have **Python 3.10.2** installed on your system. You can check your default Python version by running `python --version` (or `python3 --version`). If Python 3.10.2 is not installed, please download and install it from the [official Python website](https://www.python.org/downloads/release/python-3102/).

    Follow these steps to set up the environment:

    1.  **Open your Command Prompt (Windows) or Terminal (macOS/Linux) to clone the repository.**
        ```bash
        git clone https://github.com/Sonjoy95/Telecom-Customer-Churn-Predictive-Analysis.git
        ```
    3.  **Navigate to the project directory.**
        Use the `cd` command to change to the directory where you've cloned or downloaded this project.
        ```bash
        cd path/to/your/telecom_churn_project
        ```
        (Replace `path/to/your/telecom_churn_project` with the actual path on your system.)

    4.  **Create a Virtual Environment.**
        This command creates a new directory (e.g., `telecom_churn_env`) inside your project folder, which will house the isolated Python environment. The `venv` module will use the Python 3.10.2 executable that is found first in your system's PATH when you execute the command.

        ```bash
        # If 'python' command points to Python 3.10.2:
        python -m venv telecom_churn_env

        # If you have multiple Python versions and 'py' launcher (Windows):
        # py -3.10 -m venv telecom_churn_env

        # If you have multiple Python versions and a specific executable (macOS/Linux):
        # python3.10 -m venv telecom_churn_env
        ```
        *You can choose a different name for the environment if you prefer, but `telecom_churn_env` is used here for consistency.*

    5.  **Activate the Virtual Environment.**
        You **must** activate the environment in each new command prompt/terminal session before working on the project.

        * **On Windows (Command Prompt):**
            ```bash
            .\telecom_churn_env\Scripts\activate
            ```
        * **On Windows (PowerShell):**
            ```powershell
            .\telecom_churn_env\Scripts\Activate.ps1
            ```
        * **On macOS / Linux:**
            ```bash
            source telecom_churn_env/bin/activate
            ```
        Once activated, your command prompt/terminal prompt will typically show the environment's name in parentheses, like `(telecom_churn_env) C:\path\to\your\telecom_churn_project>`.

    6.  **Install Required Libraries.**
        With the virtual environment activated, install all the project's dependencies using the `requirements.txt` file provided.

        ```bash
        pip install -r requirements.txt
        ```
        This command will install all necessary libraries with their precise versions.

    7.  **Deactivate the environment (when done).**
        When you're finished working on the project, you can deactivate the environment.

        ```bash
        deactivate
        ```
        (Alternatively, simply closing the command prompt/terminal window will also deactivate it.)

    8.  **Place Dataset:** Ensure your raw dataset (`your_telecom_churn_data.csv`) is in the project root or specified path.
    9.  **Run Jupyter Notebook:** Execute the project notebook (e.g., `Churn_Prediction_Project.ipynb`) step-by-step to perform data processing, model training, and prediction export.
    10.  **PowerBI Integration:** Import the exported CSV (`churn_predictions_full_for_powerbi.csv`) into PowerBI Desktop. Build relationships with original data and configure visuals as described.

## 9. Future Enhancements

* **Advanced Feature Engineering:** Explore creating more complex features (e.g., interaction terms, time-series features if data permits).
* **Cost-Sensitive Learning:** Incorporate specific business costs of false positives/negatives directly into model optimization within PowerBI reports.
* **Model Monitoring Dashboard:** Develop PowerBI visuals to monitor model performance drift and data quality in a production environment.
* **Automated Data Pipelines:** Implement automated processes for data refresh and prediction updates for the PowerBI dashboard.
* **A/B Testing Integration:** Design and analyze results of A/B tests on retention strategies directly within PowerBI.

---
