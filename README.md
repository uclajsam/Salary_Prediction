# Predicting Salaries Based on Recent Job Postings

## Introduction
Have you ever thought how salaries for certain jobs are determined?  These figures are not just randomly determined.  One would assume that type of degree, the type of skills, and the job industry are just some of the factors that determine salary.  In this project, I will analyze variables associated with current job postings and how they can (or cannot) affect the salary.

Python will be used for this analysis along with the following libraries:
- Numpy
- Pandas
- Seaboard
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

## Exploratory Data Analysis
To understand the relationship between the variables and salary, we created several plots.  We used boxplots for categorical variables and line charts for numerical variables.
