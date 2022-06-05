# Group 6 Final Project: Employee Attrition
## Overview
The topic we have selected is Employee Attrition, looking particularly at the factors that contribute to employee turnover.
### Question
Can we determine the factors that lead to employee attrition? For those employees identified as being at risk, potential interventions can be considered.
### Rationale
The reason for selecting this topic was to enable companies to evalute their current employee status, and determine whether an employee is at risk for leaving the company. Given the cost associated with hiring a new employee, we'd also like to determine the factors contributing towards employees leaving to help eliminate/minimize these costs. 
### Type of Problem
This is a classification problem

### Resources
#### Dataset
Link to Dataset:

https://github.com/IBM/employee-attrition-aif360/blob/master/data/emp_attrition.csv

Dataset - This dataset was gathered from Kaggle and contains HR data of employees that were retained by or exited the company. The dataset includes multiple variables such as Education, Gender, Salary, and Job Satisfaction. 

#### Code: Python
The code for the data preprocessing, exploratory analysis, and machine learning models is in the following notebook:

https://github.com/lykkelig/Group6_Final_Project/blob/main/Python_Code/Employee_Attrition_Model.ipynb

#### Dashboard
Link to dashboard:

https://github.com/lykkelig/Group6_Final_Project/tree/Priya_branch/Web


#### ERD
The ERD can be found at this link: 

https://github.com/lykkelig/Group6_Final_Project/blob/main/DataBase/Group6-ERD-Employee_Status.pdf

#### Tableau Visualizations
https://public.tableau.com/app/profile/erik.svoboda/viz/EmployeeAttrition_16528352408970/FinalProject?publish=yes

https://public.tableau.com/app/profile/reilly.thompson/viz/Group6_Final_Tab/Story1



## Outline
### Database
Using our python code (shown below), we connect with postgreSQL to join our primary table with six additional tables. The ERD can be found at this link:

[https://github.com/lykkelig/Group6_Final_Project/blob/main/DataBase/Group6-ERD-Employee_Status.pdf](https://github.com/lykkelig/Group6_Final_Project/blob/main/Database/Group6-ERD-Employee_Status.pdf)
```
#Connect to database
conn = psycopg2.connect("host='{}' port={} dbname='{}' user={} password={}".format('127.0.0.1', 5432, 'Group6Final', 'postgres', postpw)

#Perform joins
SQL = """SELECT "Age",
"Attrition",
"BUSINESSTRAVEL",
"DailyRate",
"Department",
"DistanceFromHome",
"Education",
"EDUCATIONFIELD",
"EmployeeCount",
"EmployeeNumber",
"EnvironmentSatisfaction",
"GENDER",
"HourlyRate",
"JobInvolvement",
"JobLevel",
"JOBROLE",
"JobSatisfaction",
"MARITALSTATUS",
"MonthlyIncome",
"MonthlyRate",
"NumCompaniesWorked",
"Over18",
"OverTime",
"PercentSalaryHike",
"PerformanceRating",
"RelationshipSatisfaction",
"StandardHours",
"StockOptionLevel",
"TotalWorkingYears",
"TrainingTimesLastYear",
"WorkLifeBalance",
"YearsAtCompany",
"YearsInCurrentRole",
"YearsSinceLastPromotion",
"YearsWithCurrManager",
"NumberProjects",
"WorkplaceAccident"
FROM "Employee_Status" AS ES
JOIN "CD_TRAVEL" CDT ON ES."BusinessTravel_FK" = CDT."BUSINESSTRAVEL_FK"
JOIN "CD_DEPARTMENT" CDD ON ES."Department_FK" = CDD."DEPARTMENT_FK"
JOIN "CD_EDUCATIONFIELD" CDE ON ES."EducationField_FK" = CDE."EDUCATIONFIELD_FK"
JOIN "CD_GENDER" CDG ON ES."Gender_FK" = CDG."GENDER_FK"
JOIN "CD_ROLE" CDR ON ES."JobRole_FK" = CDR."JOBROLE_FK"
JOIN "CD_M_STATUS" CDS ON ES."MaritalStatus_FK" = CDS."MARITALSTATUS_FK"""

# Add double quotes to end
SQL = SQL + '"'


# Read the file into a dataframe
hr_df = pd.read_sql_query(SQL, conn)
hr_df_copy = hr_df 
```

### Machine Learning Model
#### Preliminary data preprocessing, preliminary feature engineering and preliminary feature selection, including the decision-making process (rationale)
Preliminary data preprocessing:
- Check shape 
    - 1470 rows, 37 columns
- Count nulls
    - 0 nulls

Preliminary feature selection:
- Count unique values in each column & drop columns with just 1 unique value. *Rationale: all employees have the same value, will not be beneficial to analysis*
    - 3 columns dropped: 'EmployeeCount','Over18','StandardHours'
- Drop columns that are not applicable to analysis. *Rationale: this feature is just an identifier and does not add value to the analysis.*
    - 1 column dropped: 'Employee ID'


Preliminary Feature Engineering:
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
**Data was split into training and testing sets using train_test_split**
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
**Shape of resultant datasets:**

X_train (1176, 53)

X_test (294, 53)

y_train (1176,)

y_test (294,)



**Data Scaling**
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
We looked at model accuracy to evaluate and compare the models. Initial accuracy scores for each model are shown below.

- Logistic Regression
    - Accuracy Score: 87.228%

- Random Forest Classifier
    - Accuracy Score: 87.772%

- Support Vector Machine
    - Accuracy Score: 87.228%

- XG Boost
    - Accuracy Score: 88.315%

- Naïve Bayes
    - Accuracy Score: 71.467%

The accuracy scores after hyperparameter tuning are shown below.
- Logistic Regression
    - Accuracy Score: 89.796%

- Random Forest Classifier
    - Accuracy Score: 86.735%

- Support Vector Machine
    - Accuracy Score: 89.116%

- XG Boost
    - Accuracy Score: 87.755%

- Naïve Bayes
    - Accuracy Score: 62.585%

There are many options that can be tried with hyperparameter tuning. We experimented with a number of options but none improved the model accuracy. 

The Confusion Matrix of the Logistic Regression model is shown below:
```
 [[247   5]
 [ 25  17]]
 ```
**Feature Selection**

Feature importances were also explored. The top five features for the logistic regression model are shown below
    
1. YearsInCurrentRole
2. YearsAtCompany
3. YearsSinceLastPromotion
4. EnvironmentSatisfaction
5. YearsWithCurrManager

**Summary of Selected Model**

The best performing model is the logistic regression model. The logistic regression model has an accuracy of 89.8%, indicating that this model correctly predicted attrition almost 90% of the time. The confusion matrix shows this model categorized 247 correctly as high risk, and 17 correctly as low risk. This model incorrectly categorized 25 incorrectly as high risk, and 5 incorrectly as low risk. 


### Visualization
Tableau was used to visualize the data with different charts. 

https://public.tableau.com/app/profile/erik.svoboda/viz/EmployeeAttrition_16528352408970/FinalProject?publish=yes

https://public.tableau.com/app/profile/reilly.thompson/viz/Group6_Final_Tab/Story1

### Dashboard
- Tools that will be used to create the final dashboard:
    - HTML file with tableau visualizations deployed on GitHub
- Interactive elements:
    - The HTML utilizes interactive buttons to navigate different pages and open links to new pages


