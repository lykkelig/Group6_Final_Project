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

Random Forest Classifier
- Accuracy Score: 87.772%

Support Vector Machine
- Accuracy Score: 87.228%

XG Boost
- Accuracy Score: 88.315%

Naïve Bayes
- Accuracy Score: 71.467%

From the first round, prior to hyperparameter tuning, Naive bayes had the lowest accuracy score, while the other four had scores within about 1% of one another. With hyperparameter tuning, we will select the model with the highest accuracy for our final model.

**Feature Selection**
Feature importances were also explored. Feature importances are shown below.
MonthlyIncome: (0.07150306388968197)
YearsAtCompany: (0.05807616966840398)
Age: (0.05330981509566146)
TotalWorkingYears: (0.050245021973411)
DailyRate: (0.04537980848227202)
MonthlyRate: (0.04498047205481945)
HourlyRate: (0.043802301414367494)
DistanceFromHome: (0.03529855546318947)
YearsWithCurrManager: (0.035122841444005494)
OverTime_No: (0.034847748816645346)
JobLevel: (0.03284102402886094)
PercentSalaryHike: (0.03107222389656012)
NumCompaniesWorked: (0.030696997069795815)
YearsInCurrentRole: (0.028732339684965504)
StockOptionLevel: (0.026851662368389616)
OverTime_Yes: (0.02557564131660932)
EnvironmentSatisfaction: (0.025286571725883385)
TrainingTimesLastYear: (0.023146406567618683)
JobSatisfaction: (0.021115409662644642)
YearsSinceLastPromotion: (0.020978506026656418)
WorkLifeBalance: (0.020934200731404465)
RelationshipSatisfaction: (0.02054707427633941)
NumberProjects: (0.019546501039550194)
JobInvolvement: (0.01937764514616901)
Education: (0.017195164662508127)
MARITALSTATUS_Single: (0.01654875832160354)
DEPARTMENT_Research & Development: (0.00971448913608631)
BUSINESSTRAVEL_Travel_Frequently: (0.009100127897459646)
DEPARTMENT_Sales: (0.008971077343631652)
EDUCATIONFIELD_Life Sciences: (0.008967076606018521)
JOBROLE_Laboratory Technician: (0.008816836204110605)
MARITALSTATUS_Married: (0.008306320305722724)
GENDER_Female: (0.008139064043715442)
EDUCATIONFIELD_Technical Degree: (0.007936484718039188)
GENDER_Male: (0.007003669397755229)
JOBROLE_Manufacturing Director: (0.00620196645433555)
EDUCATIONFIELD_Medical: (0.005799131373343575)
BUSINESSTRAVEL_Travel_Rarely: (0.0057806201018827975)
JOBROLE_Research Scientist: (0.0056002809456128025)
MARITALSTATUS_Divorced: (0.005456843388724418)
JOBROLE_Sales Representative: (0.0052514958958241355)
EDUCATIONFIELD_Marketing: (0.005220790793298497)
JOBROLE_Sales Executive: (0.0051582408509034)
BUSINESSTRAVEL_Non-Travel: (0.004910530118812076)
PerformanceRating: (0.004511609290055028)
WorkplaceAccident: (0.0038089513850478283)
JOBROLE_Healthcare Representative: (0.0033392508246691648)
EDUCATIONFIELD_Other: (0.0027170220949647517)
JOBROLE_Manager: (0.0022441859353904176)
JOBROLE_Research Director: (0.0014301035256888506)
JOBROLE_Human Resources: (0.001387590012002482)
DEPARTMENT_Human Resources: (0.0008450699093607982)
EDUCATIONFIELD_Human Resources: (0.0003692466195273013)



### Database
Using our python code (shown below), we connect with postgreSQL to join our primary table with six additional tables. The ERD can be found at this link: https://github.com/lykkelig/Group6_Final_Project/blob/main/DataBase/Group6-ERD-Employee_Status.pdf
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

### Dashboard
- Storyboard
- Description of the tool(s) that will be used to create the final dashboard:
    - HTML file with tableau visualizations deployed on GitHub
- Description of interactive element(s)
    - Tableau will contain the interactive elements

### Visualization
Using HTML we will utilize interactive buttons to navigate different pages, open links to new pages. We will use Tableau to visualize the data with different charts. 

Link to updated HTML https://github.com/lykkelig/Group6_Final_Project/tree/Erik_Branch/Web