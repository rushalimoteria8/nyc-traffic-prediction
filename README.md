# Traffic Flow Intelligence – Big Data Project (Spring 2025)

## Project Overview  
This project presents a scalable data processing and prediction framework to analyze traffic patterns in New York City using the “Automated Traffic Volume Counts” dataset from NYC Open Data. It leverages PySpark for distributed data handling, performs time-series forecasting, and applies machine learning models (Linear Regression, Random Forest, GBT) for traffic volume prediction.

---

## INSTALLATION & SETUP

### ✅ A. If using Google Colab (Recommended)

> Note: On some Google Colab sessions, basic Spark operations may work without full setup. However, to ensure compatibility with PySpark MLlib and cross-validation features, the full Spark environment setup is included below and recommended for all users.

Google Colab supports PySpark with minor setup. Use the following commands at the beginning of your notebook to configure Spark:

```python
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz
!tar xf spark-3.3.2-bin-hadoop3.tgz
!pip install -q findspark

import os
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = "/content/spark-3.3.2-bin-hadoop3"

import findspark
findspark.init()
```

Install additional required packages:
```python
!pip install prophet statsmodels scikit-learn
```

Add this right after the `findspark.init()` line:
```python
# Optional: Allocate more memory to the Spark driver (recommended for large datasets)
import os
if not os.environ.get("PYSPARK_SUBMIT_ARGS"):
    os.environ["PYSPARK_SUBMIT_ARGS"] = "--driver-memory 6g pyspark-shell"
```
> 💡 **Tip:** To avoid memory issues when loading or processing large datasets, we set the Spark driver memory to **6 GB** using the `PYSPARK_SUBMIT_ARGS` environment variable.



No other configuration is needed in Colab. The environment supports parallel Spark jobs and large dataset handling.

### ✅ B. If running locally (Jupyter Notebook or VSCode)

Ensure the following are installed:

**Prerequisites:**
- Python 3.x
- Java 8 or Java 11 (set JAVA_HOME)
- Apache Spark ([Download here]([https://example.com](https://spark.apache.org/downloads.html)))

**Required Python packages:**

Install all required packages via pip:
```python
pip install pyspark pandas matplotlib seaborn numpy statsmodels prophet scikit-learn
```

**Environment Variables**

Set up the following environment variables:
```python
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export SPARK_HOME=/path/to/spark
```
Add `$SPARK_HOME/bin` to your system path if using terminal or VSCode.

Include the following after setting up `JAVA_HOME` and `SPARK_HOME`:
```python
# (Optional) Increase Spark driver memory for local runs
export PYSPARK_SUBMIT_ARGS="--driver-memory 6g pyspark-shell"
```

> For local execution, it's recommended to allocate more memory to the Spark driver (e.g., 6 GB) using the `PYSPARK_SUBMIT_ARGS` environment variable to avoid crashing on large operations.

---

## EXECUTION INSTRUCTIONS

**Notebook:** `Traffic_Flow_Analysis_Spark.ipynb`

This notebook contains the entire pipeline, divided into the following steps:

**A. Data Ingestion**
- Connects to NYC Open Data API.
- Loads data in chunks using pagination ($limit, $offset).
- Converts raw JSON to Spark DataFrames.
- Saves combined data as a Parquet file.

**B. Data Cleaning & Preparation**
- Drops null values and duplicates.
- Filters out records with volume = 0.
- Parses spatial coordinates from wktgeom (longitude, latitude).

**C. Exploratory Data Analysis (EDA)**
- Generates:
  - Volume distribution plots
  - Borough-wise and direction-based breakdowns
  - Heatmaps (hour vs. day)
  - Top traffic segments
  - Volume trends across hours, days, and months

**D. Time-Series Forecasting**
- Uses ARIMA and Facebook Prophet on hourly traffic volume.
- Forecasts next 24 hours and visualizes results using line plots.

**E. Predictive Modeling using Spark ML**
- Models: Linear Regression, Random Forest, Gradient Boosted Trees (GBT)
- Pipeline steps:
  - Feature engineering: One-hot encoding, weekday/time features
  - Feature vectorization and scaling
  - Cross-validation and hyperparameter tuning
  - RMSE and MAE used as evaluation metrics

**F. Prediction Analysis**
- Creates a scatter plot for predicted vs. actual traffic volumes (500 sample points).
- Analyzes model performance visually and statistically.
- Performs live inference using a sample input (e.g., weekday + hour for a borough and direction).

### How to Run the Code

✅ In Google Colab:
- Open `Traffic_Flow_Analysis_Spark.ipynb` in Colab.
- Run the Spark environment setup cells first.
- Run all cells sequentially from top to bottom.
- No file upload required — data is fetched directly via the API.
- Visualizations and results will display inline.

✅ Locally (Jupyter/VSCode):
- Open the notebook in Jupyter or VSCode with a Python kernel.
- Ensure Java and Spark are properly configured.
- Run cells sequentially (model training may take longer).
- You can comment out the API fetch and use a saved .parquet file if internet access is restricted.

**Outputs Generated**
- Cleaned dataset in .parquet format
- EDA visualizations (line charts, heatmaps, bar graphs)
- Time-series forecast plots
- Actual vs. predicted scatter plot
- Live traffic volume prediction for custom input

---
## Conclusion

This project demonstrates how big data technologies and machine learning can be combined to understand and predict urban traffic patterns at scale. By using PySpark, time-series forecasting, and predictive modeling, this framework can serve as a foundation for smarter traffic monitoring systems and future congestion management solutions.





