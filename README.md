# Group 6 Final Project: Employee Attrition

## Segment 1
### Overview
The topic we have selected is Employee Attrition, looking particularly at the factors that contribute to employee turnover.
#### Question
Can we determine the factors that lead to employee attrition? For those employees identified as being at risk, potential interventions can be considered.
#### Rationale
The reason for selecting this topic was to enable companies to evalute their current employee status, and determine whether an employee is at risk for leaving the company. Given the cost asscociated with hiring a new employee, we'd also like to determine the factors contributing towards employees leaving to help eliminate/minimize these costs. 
#### Type of Problem
This is a classification problem
### Link to Dataset
https://github.com/IBM/employee-attrition-aif360/blob/master/data/emp_attrition.csv

Dataset - This dataset was gathered from Kaggle and contains HR data of employees that retained and exited the company. The dataset includes muliple variables such as Education, Gender, Salary, and Job Satisfaction. 

### Machine Learning Outline 

#### Import libraries
#### Read Data
#### Initial Analysis
- Any missing elements that need to be fixed
- Fix data issues
#### Exploratory Phase
- Get data statistics 
- Visualize the data
#### Transform Phase
- One hot encoding
- Reduction/Scaling
#### Build Models
- Logistical Regression
- Random Forest
- Support Vector Machine
- XGBoost
- Naive Bayes
#### Assess classification model accuracy
- Confusion Matrix
#### Hyper parameter tuning
- Grid search
- Random search
#### Apply tuned parameters to models
#### Test Models
- Assess classification model accuracy
- Choose best performing model
#### Finalize Model

### Communication protocols 
Our primary means of communication will be to utilize the group Slack channel, as well as meeting as needed over Zoom.

### Database
We will be using PostgreSQL

#### Data Layout
The database will contain lookup tables for categorical data. We will define foreign keys for those that we can use to join the lookup tables to the main dataset.  




#### Feature selection
Through the hyperparameter tuning, we will determine the most relevant features for our model. 

## Segment 2
### Resources
#### Code
The code for the data preprocessing, exploratory analysis, and machine learning models is in the following notebook:

https://github.com/lykkelig/Group6_Final_Project/blob/main/Python_Code/Employee_Attrition_Model.ipynb
### Presentation

### Storyboard


### Machine Learning Model
#### Description of preliminary data preprocessing
- Check shape 
    - 1470 rows, 37 columns
- Count nulls
    - 0 nulls
- Count unique values in each column & drop columns with just 1 unique value
    - 3 columns dropped: 'EmployeeCount','Over18','StandardHours'
- Drop columns that are not applicable to analysis
    - 1 column dropped: 'Employee ID'
- Explore data types
    - int64: 25 columns
    - object: 8 columns
- Use OneHotEncoder to transform object columns
```
# One Hot Encoding
from sklearn.preprocessing import OneHotEncoder
#generate categorical variable list
hr_cat = hr_df.dtypes[hr_df.dtypes == "object"].index.tolist()

# check unique values
hr_df[hr_cat].nunique()

enc = OneHotEncoder(sparse=False)

#fit/transform
hr_encode = pd.DataFrame(enc.fit_transform(hr_df[hr_cat]))

# add names to df
hr_encode.columns = enc.get_feature_names(hr_cat)

# merge/drop originals

hr_df = hr_df.merge(hr_encode, left_index=True, right_index=True)
hr_df = hr_df.drop(hr_cat, 1)

```

#### Description of preliminary feature engineering and preliminary feature selection, including the decision-making process
#### Description of how data was split into training and testing sets
**Split data using train test split**
```
# split data into feature/target
y = hr_df['Attrition_Yes'].values
X = hr_df.drop(['Attrition_Yes', 'Attrition_No'], 1)

# Split
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state=78)

print("X_train {}".format(X_train.shape))
print("X_test {}".format(X_test.shape))
print("y_train {}".format(y_train.shape))
print("y_test {}".format(y_test.shape))

```
**Shape of resultant groups:**
X_train (1102, 53)
X_test (368, 53)
y_train (1102,)
y_test (368,)


**Scale data**
```
# Reduction/Scaling
# scaling
from sklearn.preprocessing import StandardScaler
# Create instance
scaler = StandardScaler()

# Fit 
X_scaler = scaler.fit(X_train)

# Scale 
X_train_scaled = X_scaler.transform(X_train)
X_test_scaled = X_scaler.transform(X_test)
```

#### Explanation of model choice, including limitations and benefits
**Model accuracy**
Logistic Regression
- Accuracy Score: 87.228%
- Classification Report:
```    
                  pre       rec       spe        f1       geo       iba       sup

        0.0       0.87      1.00      0.00      0.93      0.00      0.00       321
        1.0       0.00      0.00      1.00      0.00      0.00      0.00        47

avg / total       0.76      0.87      0.13      0.81      0.00      0.00       368

```
Random Forest Classifier
- Accuracy Score: 87.772%
- Classification Report:
```

```


Support Vector Machine
- Accuracy Score: 87.228%
- Classification Report:
```


```




XG Boost
- Accuracy Score: 88.315%
- Classification Report:
```


```




Naïve Bayes
- Accuracy Score: 71.467%
- Classification Report:
```


```


### Dashboard
- Description of the tool(s) that will be used to create the final dashboard:
    - HTML file with tableau visualizations deployed on GitHub
- Description of interactive element(s)
    - Tableau will contain the interactive elements
