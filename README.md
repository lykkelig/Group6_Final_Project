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


https://github.com/lykkelig/Group6_Final_Project/blob/MeLea_branch/Resources/HR_Attrition_DB.xlsx


#### Feature selection
Through the hyperparameter tuning, we will determine the most relevant features for our model. 


