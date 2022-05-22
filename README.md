# Group6_Final_Project

### Segment 1
1) Project Topic (what are you trying to solve)
- Employee Attrition 
- See whether a not a person is at risk for leaving the company 
- What is the problem, why do they want to leave? 


2) Dataset - What are you picking (does not need to be final)

https://www.kaggle.com/datasets/colearninglounge/employee-attrition?select=employee_attrition_test.csv

Updated Dataset -

https://github.com/IBM/employee-attrition-aif360/blob/master/data/emp_attrition.csv

Dataset - This dataset was gathered from Kaggle and contains HR data of employees that retained and exited the company. The dataset includes muliple variables such as Education, Gender, Salary, and Job Satisfaction.


3) Describe EDA (exploratory data analysis, clean, org, plot)

 - Python, Jupyter, Pandas

4) Describe DB (Mongo vs SQL, tables, etc...)

- We will be using PostgreSQL

5) Machine Learning Model (build a flow chart to think out loud of what you are planning to do. Classification vs regression, how you think it will work)

Communication protocols: Constant slack communication and keep an up to date readme. 

Import libraries

Read Data

Initial Analysis

- Any missing elements that need to be fixed
- Fix data issues
Exploratory Phase
- Get data statistics
- Visualize the data
Transform Phase
- One hot encoding
- Reduction/Scaling
Build Models
- Logistical Regression
- Random Forest
- Support Vector Machine
- XGBoost
- Naive Bayes
Assess classification model accuracy
- Confusion Matrix
Hyper parameter tuning
- Grid search
- Random search
Apply tuned parameters to models
Test Models
- Assess classification model accuracy
- Choose best performing model
Finalize Model
