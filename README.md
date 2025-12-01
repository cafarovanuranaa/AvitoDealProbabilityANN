# Avito Deal Probability Prediction using ANN – README Explanation

This project implements an Artificial Neural Network (ANN) for predicting the deal probability of Avito listings. The pipeline follows a structured end-to-end approach, including data cleaning, preprocessing, feature engineering, encoding, scaling, model building, hyperparameter tuning, and evaluation.

---

## 1. Data Loading and Sampling
The dataset `regression_avito_deals.xlsx` is loaded, and a 10% random sample is taken to accelerate initial experiments.

**Dataset Link:**  
[Google Drive Dataset](https://docs.google.com/spreadsheets/d/1Qv5rrzmkFZCzbKnovFikmisG-KiIz8DM/edit?usp=drive_link&ouid=117339924051817037398&rtpof=true&sd=true)


---

## 2. Initial Data Exploration
- Columns with very low variance or a single unique value are removed.
- Missing values are handled appropriately:
  - `param_3` → filled with `'Unknown'`.
  - Other categorical columns → filled with mode.
  - Numerical columns → filled with mean.

---

## 3. Outlier Treatment
- Boxplots are drawn for numeric features.
- Extreme values are clipped using the Interquartile Range (IQR) method to prevent distortion in distance-sensitive or scaled features.

---

## 4. Feature Grouping and Engineering
- High-cardinality categorical features (`param_1`, `param_2`, `param_3`) are grouped into logical clusters.
- Rare categories in `category_name` are replaced with `"Other"`.
- New features created:
  - `category_encoded`: target-encoded using K-Fold smoothing.
  - `category_popularity`: count of items per category.
  - `city_grouped`: top 15 cities preserved, others labeled as `"Other"`.
  - `region_activity`: count of items per region.
  - `desc_length`: length of the description.
  - Date-based features: year, month, weekday from `activation_date`.

---

## 5. Feature Selection
- Features strongly correlated with the target (`deal_probability`) are retained.
- Multicollinearity is checked using correlation thresholds and VIF, removing redundant variables to improve stability.

---

## 6. Encoding and Scaling
- Remaining categorical features are one-hot encoded.
- Features are standardized using `StandardScaler` to ensure consistent scale for the ANN.
- Dataset is split into input features (`X`) and target (`y`).

---

## 7. Train/Test Split
- The data is divided into training (80%) and testing (20%) sets to allow unbiased evaluation of the model.

---

## 8. ANN Model Definition
- A Sequential ANN is created with:
  - Two hidden layers with ReLU activation.
  - One output neuron with ReLU activation for regression.
- The optimizer, learning rate, batch size, and epochs are parameterized for hyperparameter tuning.
- Loss function: mean absolute error (MAE).

---

## 9. Hyperparameter Optimization with Optuna
- Optuna searches for the best hyperparameters:
  - Number of neurons in each hidden layer.
  - Optimizer choice (`Adam`, `SGD`, `RMSprop`, `Adagrad`).
  - Learning rate.
  - Batch size and epochs.
- Each trial trains the ANN and evaluates R² on the test set.
- The best trial is selected for the final model.

---

## 10. Best Model Construction
- The ANN is rebuilt using the best hyperparameters from Optuna.
- Compiled with the corresponding optimizer and learning rate.
- Trained on the full training set.

---

## 11. Model Evaluation
- Metrics computed for both training and test sets:
  - R² (coefficient of determination)
  - MAE (mean absolute error)
- Provides insight into predictive power and potential overfitting.

---

## 12. Summary
This ANN pipeline:

- Cleans and preprocesses Avito deal data.
- Performs feature engineering and encoding suitable for neural networks.
- Tunes hyperparameters to optimize performance.
- Produces a well-optimized ANN for regression tasks.
- Provides clear evaluation metrics for interpretation.

The result is a fully functional ANN capable of predicting `deal_probability` with optimized architecture and parameters.
