# Healthcare Insurance Fraud Detection

## 📋 Project Overview

This project develops machine learning models to detect fraudulent healthcare insurance providers using beneficiary, inpatient, and outpatient claims data. The system identifies suspicious patterns in provider behavior to minimize financial losses from fraudulent claims while maintaining high detection rates.

**Key Features:**
- Multi-model ensemble approach (Gradient Boosting, XGBoost, Logistic Regression, Random Forest)
- Cost-optimized fraud detection (balancing false positives and false negatives)
- Comprehensive feature engineering from claims and beneficiary data
- Model calibration for reliable probability estimates
- Detailed error analysis and business cost evaluation

---

## 👥 Team Members

- **Omar Hossameldin**
- **Karim Mostafa**
- **Omar Hany**
- **Ramez Mokbil**

---

## 📊 Summary of Results

### Model Performance (Validation Set)

| Model | Threshold | Precision | Recall | F1 Score | ROC-AUC | Avg Precision |
|-------|-----------|-----------|--------|----------|---------|---------------|
| **Random Forest** | 0.2014 | 0.575 | 0.902 | 0.702 | 0.957 | 0.839 |
| Gradient Boosting | 0.6208 | 0.534 | 0.922 | 0.677 | 0.958 | 0.842 |
| XGBoost | 0.5653 | 0.540 | 0.922 | 0.679 | 0.954 | 0.835 |
| Logistic Regression | 0.3092 | 0.301 | 0.922 | 0.453 | 0.904 | 0.671 |

### Best Model: Random Forest
- **Test Set Performance:**
  - Precision: 0.5744
  - Recall: 0.9020
  - F1 Score: 0.7016
  - ROC-AUC: 0.9569
  - Average Precision: 0.8395

### Business Cost Analysis
- **False Negative Cost:** $10,000 per missed fraud case
- **False Positive Cost:** $500 per incorrect fraud flag
- **Random Forest achieved the lowest total business cost** among all models
- Cost savings vs. worst performing model: Significant reduction in investigation costs

### Key Insights
- Random Forest provides the best balance between fraud detection and false alarm costs
- High recall (90%+) ensures most fraudulent providers are flagged
- Model calibration improves probability estimates for better decision-making
- Error analysis reveals opportunities for additional feature engineering

---

## 📁 Project Structure

```
HealthCare-Insurance-Fraud-Detection/
│
├── data/                                    # Data directory
│   ├── Train_Beneficiarydata-*.csv         # Training beneficiary data
│   ├── Train_Inpatientdata-*.csv           # Training inpatient claims
│   ├── Train_Outpatientdata-*.csv          # Training outpatient claims
│   ├── Train-*.csv                         # Training labels
│   ├── Test_Beneficiarydata-*.csv          # Test beneficiary data
│   ├── Test_Inpatientdata-*.csv            # Test inpatient claims
│   ├── Test_Outpatientdata-*.csv           # Test outpatient claims
│   ├── Test-*.csv                          # Test labels
│   └── processed/                          # Processed datasets
│       └── Train_final.csv                 # Engineered features for training
│
├── notebooks/                               # Jupyter notebooks
│   ├── data_exploration_and_feature_engineering.ipynb
│   ├── 02_modeling.ipynb                   # Model training and calibration
│   └── 03_evaluation.ipynb                 # Model evaluation and analysis
│
├── models/                                  # Saved models
│   ├── gb_calibrated.joblib                # Calibrated Gradient Boosting
│   ├── xgb_calibrated.joblib               # Calibrated XGBoost
│   ├── log_calibrated.joblib               # Calibrated Logistic Regression
│   └── rf_calibrated.joblib                # Calibrated Random Forest
│
├── reports/                                 # Generated reports and visualizations
│   ├── confusion_matrices.png              # Confusion matrices for all models
│   └── cost_analysis.png                   # Business cost comparison charts
│
└── README.md                                # Project documentation
```

---

## 🚀 Reproduction Instructions

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab
- Required libraries (see Installation)

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/KarimMostafa123/HealthCare-Insurance-Fraud-Detection.git
cd HealthCare-Insurance-Fraud-Detection
```

2. **Create a virtual environment (recommended):**
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate
```

3. **Install required packages:**
```bash
pip install numpy pandas matplotlib seaborn scikit-learn xgboost joblib jupyter
```

### Running the Analysis

#### Step 1: Data Exploration and Feature Engineering
```bash
jupyter notebook notebooks/01_data_exploration_and_feature_engineering.ipynb
```
- Execute all cells to explore the dataset
- Generates engineered features and saves to `data/processed/Train_final.csv`
- Creates visualizations of data distributions and correlations

#### Step 2: Model Training and Calibration
```bash
jupyter notebook notebooks/02_modeling.ipynb
```
- Execute all cells to train four machine learning models
- Applies isotonic calibration for probability estimates
- Optimizes decision thresholds for target recall levels
- Saves calibrated models to `models/` directory

#### Step 3: Model Evaluation and Analysis
```bash
jupyter notebook notebooks/03_evaluation.ipynb
```
- Execute all cells to load and evaluate saved models
- Generates performance metrics on validation and test sets
- Creates confusion matrices and cost analysis visualizations
- Performs error analysis on false positives and false negatives
- Saves reports to `reports/` directory

### Expected Outputs

After running all notebooks, you should have:
- ✅ Processed training data: `data/processed/Train_final.csv`
- ✅ Four calibrated models in `models/` directory
- ✅ Performance metrics matching the results table above
- ✅ Confusion matrix visualizations: `reports/confusion_matrices.png`
- ✅ Cost analysis charts: `reports/cost_analysis.png`
- ✅ Error analysis samples in `data/processed/`

### Troubleshooting

**Issue: Models not found**
- Ensure you've run `02_modeling.ipynb` completely to save models
- Check that `models/` directory exists

**Issue: Data files not found**
- Verify all CSV files are in the `data/` directory
- Check file names match those in the notebooks

**Issue: Memory errors**
- Reduce dataset size for testing
- Close other applications to free up RAM
- Consider using a machine with more memory

---

## 📈 Methodology

### 1. Feature Engineering
- Aggregated provider-level statistics from beneficiary and claims data
- Created claim frequency, cost, and demographic features
- Engineered ratio features (e.g., claims per beneficiary)
- Handled missing values and outliers

### 2. Model Training
- Trained four classification models: Gradient Boosting, XGBoost, Logistic Regression, Random Forest
- Applied stratified train-validation-test splits (80%-10%-10%)
- Used isotonic calibration for probability estimates
- Optimized thresholds for high recall (90-92%) to minimize missed fraud

### 3. Evaluation Framework
- **Metrics:** Precision, Recall, F1, ROC-AUC, Average Precision
- **Business Cost Analysis:** Weighted false negatives ($10,000) vs false positives ($500)
- **Error Analysis:** Case studies of misclassified providers
- **Visualization:** Confusion matrices and cost comparison charts

---

## 🎯 Future Improvements

1. **Additional Features:**
   - Temporal patterns (claim timing, frequency over time)
   - Network analysis (provider-beneficiary relationships)
   - Geographic anomaly detection

2. **Model Enhancements:**
   - Ensemble methods combining multiple models
   - Deep learning approaches for complex pattern detection
   - Cost-sensitive learning directly optimizing business costs

3. **Operational Strategies:**
   - Real-time fraud detection pipeline
   - Human-in-the-loop review for borderline cases
   - Continuous monitoring and model retraining

---

## 📄 License

This project is for educational purposes.

---

**Last Updated:** December 2025