# Early Detection of Diabetes: A Machine Learning Approach

## 1. Introduction

This presentation outlines a data science project focused on building a classification model to predict diabetes based on various health indicators. The goal is to assist mobile health clinics in pre-screening patients to identify individuals at risk and reduce hospital crowding.

## 2. Problem Statement

**Background:** A mobile health clinic aims to improve patient pre-screening for diabetes using basic health indicators.

**Objective:**
- Conduct Exploratory Data Analysis (EDA) to uncover factors influencing diabetes risk
- Develop a classification model to predict diabetes based on attributes like BMI, glucose, insulin levels, and age

## 3. Data Overview & Initial Exploration

- **Dataset Size:** 768 entries (rows) and 9 features (columns)
- **Features:** Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age, Outcome (target variable: 0 for No Diabetes, 1 for Diabetes)

### Initial Data Quality Checks:
- No missing values (data.isna().sum() showed all zeros)
- No duplicate rows (data.duplicated().sum() showed zero)

### Statistical Summary Highlights:
- Glucose, BloodPressure, SkinThickness, Insulin, and BMI had a minimum value of 0, which are biologically implausible and indicate potential missing/imputed values needing preprocessing
- Insulin and DiabetesPedigreeFunction showed high positive skewness, suggesting non-normal distributions

## 4. Exploratory Data Analysis (EDA)

### 4.1. Univariate Analysis

- **Outcome Distribution:** The dataset showed an imbalance, with approximately twice as many "No Diabetes" cases (0) as "Diabetes" cases (1)
- **Pregnancies Distribution:** Most individuals had 1 pregnancy, followed by 0. Higher numbers of pregnancies were less frequent
- **Distribution of Numerical Features (KDE Plots):** Glucose, Insulin, BMI, DiabetesPedigreeFunction, and Age exhibited varying degrees of skewness, particularly Insulin and DiabetesPedigreeFunction were highly right-skewed

### 4.2. Multivariate Analysis

**Correlation Matrix Insights:**
- **Outcome & Glucose (0.47):** A moderate positive correlation, indicating higher glucose levels are associated with a higher likelihood of diabetes
- **Pregnancies & Age (0.54):** Moderately positive, suggesting older individuals tend to have more pregnancies
- **Outcome & BMI (0.29):** A weak positive correlation
- **SkinThickness & Insulin (0.44):** Moderate positive correlation

**Pairwise Plots (Colored by Outcome):**
- Visually confirmed Glucose as a strong differentiator between diabetic and non-diabetic individuals, with diabetics clustering at higher glucose values
- Age vs. BMI and Age vs. Glucose plots showed that older individuals with higher BMI or glucose levels were more likely to be diabetic
- Insulin and SkinThickness showed wide variability without clear separation by outcome

**Box Plots (Features by Outcome):**
- Glucose levels were significantly higher for individuals with diabetes
- BMI showed a slight upward trend for diabetics
- Age indicated that diabetics tend to be older
- BloodPressure showed overlap, suggesting less distinction by outcome

### 4.3. Feature Engineering

**BMI Category:** A new feature BMICategory was created (Underweight, Normal, Overweight, Obese).

**Insights from BMI Category:**
- Obese and Overweight individuals tended to have a higher average number of pregnancies
- Individuals with diabetes (Outcome = 1) consistently showed higher glucose levels across all BMI categories (Obese, Overweight, Normal, Underweight), reinforcing the importance of glucose

**General EDA Insight:** Glucose levels are the most influential factor impacting diabetes risk, followed by BMI and Age. Other features showed weaker direct relationships.

## 5. Data Preprocessing for Model Building

### 5.1. Handling Invalid Zero Values

- Rows where Glucose, BloodPressure, SkinThickness, Insulin, or BMI were zero were removed, as these values are biologically unrealistic
- This reduced the dataset from 768 to 392 entries

### 5.2. Feature Transformation & Scaling

- **Skewness Check (After Zero Removal):** Features like Pregnancies, Insulin, DiabetesPedigreeFunction, and Age still showed significant skewness
- **Power Transformation (Yeo-Johnson) & Standardization:** A Pipeline combining PowerTransformer (Yeo-Johnson method) and StandardScaler was applied to all features to normalize their distributions and scale them
- This successfully reduced the skewness for all features, making them more suitable for machine learning models

### 5.3. Handling Class Imbalance

- **Target Variable Distribution:** After preprocessing, the Outcome variable still showed imbalance (No Diabetes: 262 instances, Diabetes: 130 instances)
- **SMOTEENN Resampling:** To address this, SMOTEENN (Synthetic Minority Over-sampling Technique with Edited Nearest Neighbors) was applied to the training data
- This technique over-samples the minority class and cleans the resulting data, leading to a more balanced dataset for training (x_resampled and y_resampled had 332 instances each)

### 5.4. Train-Test Split

The resampled data was split into training (80%) and testing (20%) sets to evaluate model performance (x_train, y_train, x_test, y_test).

## 6. Model Building and Evaluation

Various classification models were trained and evaluated:
- Logistic Regression
- Random Forest Classifier
- Decision Tree Classifier
- Support Vector Machine (SVM)

### Best Performing Model: Random Forest Classifier

The Random Forest Classifier consistently demonstrated strong performance.

- **Overall Accuracy:** 95.5% (64 out of 67 samples correctly predicted)

**Confusion Matrix:**
- True Negatives (TN): 30
- False Positives (FP): 3
- False Negatives (FN): 0
- True Positives (TP): 34

**Key Performance Metrics (from Classification Report):**
- **Precision (Class 0 - Low Risk):** 1.00 (All predictions of 'No Diabetes' were correct)
- **Precision (Class 1 - High Risk):** 0.92 (A few false positives)
- **Recall (Class 0 - Low Risk):** 0.91 (Missed 3 'No Diabetes' cases)
- **Recall (Class 1 - High Risk):** 1.00 (No high-risk cases were missed - Zero False Negatives!)
- **F1-Score:** Both classes scored around 0.95-0.96, indicating strong and balanced performance

**Crucial Finding:** The model achieved zero false negatives, meaning it correctly identified all high-risk (diabetic) cases. This is extremely valuable for health and safety-related applications, where missing a high-risk individual can have severe consequences.

### 6.1. Feature Importance (Random Forest Model)

The Random Forest model provides insights into the most influential features:

- **Glucose (Importance ≈ 0.30):** Overwhelmingly the most important factor, aligning with clinical understanding
- **Age (Importance ≈ 0.22):** Second most important, indicating age plays a significant role
- **Insulin (Importance ≈ 0.16):** Substantial contribution to the model's decisions
- **BMI (Importance ≈ 0.10):** Moderate influence
- **SkinThickness (Importance ≈ 0.07):** Lesser contribution
- **Pregnancies (Importance ≈ 0.05):** Relatively low importance
- **DiabetesPedigreeFunction (Importance ≈ 0.04):** Low importance
- **BloodPressure (Importance ≈ 0.03):** Least important feature in this model

## 7. Conclusion

The developed Random Forest classification model demonstrates exceptional performance in detecting diabetes, particularly with its 95.5% accuracy and perfect recall for high-risk cases (zero false negatives). This model is highly effective in identifying individuals with diabetes, making it a valuable tool for pre-screening in mobile health clinics. The analysis confirms that Glucose, Age, Insulin, and BMI are the most critical indicators for diabetes prediction.

The model's ability to avoid missing high-risk individuals is crucial for health-related applications, ensuring timely intervention and patient care.