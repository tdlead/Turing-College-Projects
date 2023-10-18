# Logistic Regression
Link: https://cosmic-station-abb.notion.site/Logistic-Regression-6acaea68f09148b6a83b2b63d7ffe460 <br>
Link to Colab: https://colab.research.google.com/drive/1wqGLd0xybdCw9S-F-PvfeweT3FnBWaQv?usp=sharing
# Define the problem

Problem : To predict whether the patient has **10-year risk of future coronary heart disease (CHD)**

# Data Preparation

Data Source: **Heart_desease.csv**

Data Origin: cardiovascular study on residents of the town of **Framingham, Massachusetts**. 

Description: The dataset provides the patients’ information. It includes over 4,000 records and 15 attributes. The data attributes are described in Heart_desease_desc sheet.

Variables: 

- Sex: male or female(Nominal)
- Age: Age of the patient;

(Continuous - Although the recorded ages have been truncated to whole numbers, the concept of age is continuous)

- Current Smoker: whether or not the patient is a current smoker (Nominal)
- Cigs Per Day: the number of cigarettes that the person smoked on average in one day.(can be considered continuous as one can have any number of cigarettes, even half a cigarette.)

Medical( history)

- BP Meds: whether or not the patient was on blood pressure medication (Nominal)
- Prevalent Stroke: whether or not the patient had previously had a stroke (Nominal)
- Prevalent Hyp: whether or not the patient was hypertensive (Nominal)
- Diabetes: whether or not the patient had diabetes (Nominal)
- Tot Chol: total cholesterol level (Continuous)
- Sys BP: systolic blood pressure (Continuous)
- Dia BP: diastolic blood pressure (Continuous)
- BMI: Body Mass Index (Continuous)
- Heart Rate: heart rate (Continuous - In medical research, variables such as heart rate though in fact discrete, yet are considered continuous because of large number of possible values.)
- Glucose: glucose level (Continuous)

**Predict variable** (desired target)

- 10 year risk of coronary heart disease CHD (binary: “1”, means “Yes”, “0” means “No”)

# Data Exploration

### Data Structure

Entries: **4238**
Data columns:  **16 columns**

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled.png)

Categorical binary columns: `male`, `currentSmoker`, `BPMeds`, `prevalentStroke`, `prevalentHyp`, `diabetes`, `TenYearCHD`

---

Ordinal categorical columns: `education`

Continuous variables: `age`, `cigsPerDay`, `totChol` , `sysBP`, `diaBP`, `BMI`, `heartRate`, `glucose`

### Unique values

- Total Duplicates rows observed: 0

[Unique Values in 7 Categorical Variables]

- male : 2 Unique Values -> [1 0]
- currentSmoker : 2 Unique Values -> [0 1]
- BPMeds : 2 Unique Values -> [ 0. 1. nan]
- prevalentStroke : 2 Unique Values -> [0 1]
- prevalentHyp : 2 Unique Values -> [0 1]
- diabetes : 2 Unique Values -> [0 1]
- TenYearCHD : 2 Unique Values -> [0 1]

### Missing data

Number of observations (rows) with missing data values = **582 (13.73%)** 

|  | Total | Percent(%) |
| --- | --- | --- |
| ⚠️ glucose | 388 | 9.155262 |
| education | 105 | 2.477584 |
| BPMeds | 53 | 1.250590 |
| totChol | 50 | 1.179802 |
| cigsPerDay | 29 | 0.684285 |
| BMI | 19 | 0.448325 |
| heartRate | 1 | 0.023596 |

`glucose` has the highest missing data values 9.16%. Other columns `education`, `BPMedstotCholcigsPerDayBMI` , `heartRate` has not semnificative number of missing values.

**Solution:** 

- Drop the missing values that are less than 3 % of column data
- Handle glucose missing values by imputing a median per diabetes 
It was mapped with diabetes because there is a significant difference in glucose value in no diabetes/diabetes

After cleaning missing I have **3987** records.

### Summary statistics

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%201.png)

Observation : 

- I've checked all min and max values, and it seems that data is reliable, as it possible to have encountered such metrics, even though some of them are extremely low or extremely high. 
I need to check for outliers.
- When I compare mean and median, I observe that some column data is quite skewed.

### Class Distribution

**Class Distribution for categorical binary data**

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%202.png)

The dataset has severe class imbalance problem with: `prevalentStroke` , `diabetes`, `BPMeds` variables, as well as the target variable `TenYearCHD` is also imbalanced.

**Class Distribution for ordinal**

Education 

1.0    41.5%
2.0    30.3%
3.0    16.7%
4.0    11.5%

The education is also imbalanced. 

**Class Distribution for numerical data**

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%203.png)

Most of the data is skewed , but `glucose` is very significantly skewed to the right. 

**Box plots** 

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%204.png)

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%205.png)

There are serious outlier, people with no `diabetes` has a big number of outliers `glucose` , the value can reach values from the one from`with diabetes.

| Count | Outliers |
| --- | --- |
| ⚠️ glucose | 246 |
| ⚠️ sysBP | 120 |
| BMI | 94 |
| diaBP | 81 |
| heartRate | 76 |
| totChol | 50 |
| cigsPerDay | 11 |
| age | 0 |

Feature `totChol`, `sysBP`, `diaBP`, `BMI`, `heartRate`, `glucose` have a lot of outliers.

## **Correlation and Multicollinearity Analysis**

### Correlation with our target variable THD

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%206.png)

`age`, `sysBP`, `prevalentHyp`,`diaBP`, `glucose` has some level of correlation with our target variable.

### **Correlation matrix**

Correlation analysis between variables spotted some of the high-correlated factors: 
![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%207.png)

`age`, `sysBP`, `prevalentHyp`,`diaBP`, `glucose` has some level of correlation with our target variable.

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%208.png)

`diaBP` and `sysBP` has quite high correlation of 0.79. `cigsPerDay` and `currentSmoker` ha also quite high correlation of 0.77. I will exclude them from analysis to avoid overfitting.

### **Variance inflation factor (VIF)**

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%209.png)

`diaBP` and `sysBP` has very high VIF factor of **138**. I will also consider this, when I will remove predictors from training to avoid **multicollinearity**, and it will be diaBP, because it has lower correlation with the target.

# Model Training

### Splitting the data

The dataset was randomly split in 70% training data and 30% testing data.

predictors training table: (2790, 15)
predictors test table: (1197, 15)
target training table: (2790,)
target test table: (1197,)

### Remove outliers

Remove outliers based on IQR*1.5 technique

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%2010.png)

### First model on training data

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%2011.png)

`male` `age` `cigsPerDay` `sysBP` have a p > 0.05.

### Remove insignificant variables and fit the model

   

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%2012.png)

<aside>
💡 Variables were removed using step-wise approach and comparing the AIC metric (penalizing metric for complexity)

</aside>

# Analyze results on test data

### Confusion Matrix

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%2013.png)

**What percentage of patients with heart disease were correctly identified**

Sensitivity(Recall) = 61.11%

**What percentage of patients without heart disease were correctly identified**

Specificity = 99.30%

### ROC

![Untitled](Logistic%20Regression%206acaea68f09148b6a83b2b63d7ffe460/Untitled%2014.png)

With an AUC of 0.71, the model is reasonably good at distinguishing between the two classes (positive and negative). However, there is still room for improvement in terms of its overall predictive ability.

# Suggestions

- Need more data about persons with heart disease
- Look for other possible variables
- Try different classification approach to optimize ROC curve
