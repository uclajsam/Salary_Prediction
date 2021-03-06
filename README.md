# Predicting Salaries Based on Recent Job Postings

## Introduction
Have you ever thought how salaries for certain jobs are determined?  These figures are not just randomly determined.  One would assume that type of degree, the type of skills, and the job industry are just some of the factors that determine salary.  In this project, I will analyze variables associated with current job postings and how they can (or cannot) affect the salary.

Python will be used for this analysis along with the following libraries:
- Numpy
- Pandas
- Seaboarn
- Matplotlib
- Sklearn

## Data Dictionary
| Variable | Description |
| -------- | ----------- |
| job_id | Unique identifier for the job posting |
| company_id | Identifier for each company that posted at least one job in this dataset |
| job_type | Job category based on level in company (C-level exeuctives at the top and Junior roles on the bottom) |
| degree | Minimum degree level required for the job |
| major | Desired degree major for the job |
| industry | Industry the company is part of |
| years_experience | Years of experienced desired for the job posting |
| miles_from_metropolis | Distance (in miles) the job is located from a downtown metropolis location (i.e. Los Angeles, San Francisco, New York City, etc.) |
| salary | The posted salary of the job |


## Data Preparation
The dataset consists of 1,000,000 entries and is fairly clean.  To determine any outliers, we used the IQR rule.  Outliers did exist on both sides.  Some salaries had a value of 0 while some had values greater than the job_type classification.  For example, there were 'junior' level roles being paid a salary correlating to a CEO.  After further analysis, we deduced that these 'junior' level roles were classified as such because some industries like Oil and Finance have junior roles that pay this high. Therefore, we only removed the entries with salaries equal to 0 becaue the data seems inaccurate.

The plots below illustrate the distribution of the target variable (with desired outliers removed):  

![Salary Distribution](https://user-images.githubusercontent.com/60159655/85882437-1d218400-b794-11ea-84af-44afb11f5abe.png)


## Exploratory Data Analysis (EDA)
To understand the relationship between the variables and salary, we created several plots.  We used boxplots for categorical variables and line charts for numerical variables.

![Job Type Degree](https://user-images.githubusercontent.com/60159655/85888080-bdc87180-b79d-11ea-8851-73b7f6aace30.png)

![Major Industry](https://user-images.githubusercontent.com/60159655/85888197-f49e8780-b79d-11ea-9a9f-c190ae0c2943.png)

![Miles Experience](https://user-images.githubusercontent.com/60159655/85888643-d71ded80-b79e-11ea-883e-04d103ec0191.png)

From these plots, we can determine a few things:
- For the type of degree, there is a significant difference in mean salary between jobs with a high school diploma (and lower) and jobs requiring a minimum of a Bachelor's degree
- There is a significant difference in mean salary between jobs requiring a major and those that do not require one.
- Mean salary increases based on the job classification
- As distance from a major city increases, the salary for these jobs generally decrease
- Job salaries generally increase with more years of experience. 


### Checking Dataset Distribution
In addition, we also analyzed the counts for each variable to see if we have a balanced sampling set.

![Company ID Countplot](https://user-images.githubusercontent.com/60159655/85899727-5ddcc580-b7b3-11ea-9d98-6b6b93956acb.png)

![Job Type Degree Countplot](https://user-images.githubusercontent.com/60159655/85901493-d8f3ab00-b7b6-11ea-8f93-50e97f6f3813.png)

![Major Industry Countplot](https://user-images.githubusercontent.com/60159655/85901592-04769580-b7b7-11ea-88a3-646c7215939f.png)

![Miles From Metropolis Countplot](https://user-images.githubusercontent.com/60159655/85901753-55868980-b7b7-11ea-9d90-dca09dc036b3.png)

![Years Experience Countplot](https://user-images.githubusercontent.com/60159655/85901825-77800c00-b7b7-11ea-83d4-8cc5650b9c10.png)

From these countplots (except for the Major countplot), we see that the dataset is well-balanced.  From the Major countplot, the high number of 'None' majors can be attributed to these job postings not requiring a college degree.  Taking this column out from the plot, we see that the others majors are well-balanced.  As a result of these observations, no additional sampling techinques are needed.


### Correlation Matrix
To implement a correlation matrix that includes both categorical and numerical variables, we must alter the categorical labels to meaningful numerical information.  Since we have a large enough dataset, we decided to replace each categorical label with the avearge salary for that label.  The images below illustrate the changes:

![Original Labels for Categories](https://user-images.githubusercontent.com/60159655/85904764-6b4b7d00-b7be-11ea-8f09-4543d072e2a6.png)

![Numbered Lables for Categories](https://user-images.githubusercontent.com/60159655/85904619-17409880-b7be-11ea-8280-69a7fbeea1dc.png)

Using these changes, we developed the following correlation matrix:

![Correlation Matrix](https://user-images.githubusercontent.com/60159655/85905031-152b0980-b7bf-11ea-9823-56d0629a37ce.png)

The following observations can be made about this matrix:
- Aside from the company ID, all other variables have a significant relationship with salary
- There is a significant relationship between degree and major with respect to job type.  This is evident in the boxplots above since the job type below the 'Junior' level is correlated with not having a degree or not declaring a major.
- There is a very strong relationship between degree and major.  This makes sense because a major must be delcare before you can get a degree in that subject.


## Modeling 
Before implementing different models with our dataset, we performed one-hot encoding on the categorical features.  In addition, we created a baseline metric by using a simple linear regression model with miles from metropolis as the independent variable.  From this model we deduced the following machine learning models appropriate for this dataset:

1. Linear Regression (all features) - Since salary is affected by many features (as seen from the correlation matrix above), this choice seems appropriate.
2. Random Forest - This model helps determine which features affect the target variable the most.  This model also works well with regression-related scenarios
3. Gradient Boosting - A form of 'random forest' where learning is improved from previous iterations.  

We used 'mean squared error' as our metric to evaluate the validity of our models.  Below is a table of our results:

| Model | Mean Squred Error |
| ----- | ----------------- |
| Baseline | 1366.19 |
| Linear Regression | 384.44 |
| Random Forest | 444.66 |
| Gradient Boosting | 359.47 |

We used the Gradient Boosting model to train our 'test' data set in order to predict salaries.  We saved the predictions as well as plotting the model's 'feature importances'.

![Feature Importances](https://user-images.githubusercontent.com/60159655/85908293-63450a80-b7c9-11ea-9430-af66be3084aa.png)

**Observations**
- Miles from metropolis and years of experience were very important in our final model.
- The 'Janitor' job type is also important.  This label is heavily influenced by the lack of a college degree, which results in an overall lower salary.

## Conclusion
This project served to understand how the variables in our dataset affect a target variable (salary).  By doing EDA, we were able to undersatnd what relationsihps were present between the target variable and all other variables.  Our observations were further heightened by the relationships depicted by our correlation matrix.  After defining such relationships, we trained several models to determine which one best suits our data.  We discovered that Gradient Boosting worked the best and we used this model to fit a 'test' dataset and predict salaries from that information.  We saved the predictions on a separate file as well as displaying which features were the most important in our model.  To no surprise, the most important features in our model supported our observations made from EDA.  To further the analysis of our dataset, more variables can be introduced which could affect salary.  Examples of such variables include itemizing job duties, essential skills, certifications, and demand of industry.  
