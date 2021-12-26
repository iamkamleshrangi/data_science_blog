# data_science_blog
stack overflow 2017, Developers survey 2017
# What does the developer's survey think about Job Satisfaction?

To understand the data, we require a industry standard model for doing the analysis, for now, I'm selecting CIRSP-DM method, it is efficient with clear goals on each steps. CRISP-DM stands for Cross-industry standard process for data mining, is an open standard process model that describes common approaches used by data mining experts. It is the most widely-used analytics model to understand and answer data related questions. Base line of the CIRSP-DM as follows.
- Business Understanding
- Data Understanding 
- Prepare Data 
- Data Modeling (Optional)
- Evaluate the Results
- Deploy

## Business Understanding

Job satisfaction survy is the most importance topic for business, as companies pay on avarage $1K per employee on training program, It would be better if they can understand the trand of the employees job satisfaction, That can leads to understand the current trands and satisfaction levels within the organization, so that business can take best possible step towards it. By doing so, they can save alot of companies cost and time to encorporate businesses. I want to demonstrate few points which can help of answer few of the question towards job satisfaction. 

### Introduction

I’ll use the StackOverflow developer survey 2017, and we will take a closer look at these questions and the data which contains 64000 reviews from 213 countries, the aim of the survey was to understand the job-related aspect in the software development field.

There are two DataFrame in the dataset, first is schema_df and second is survey_df. The schema_df contains the schema of the question which being asked during the servey process, that has one-to-many answer related to the questions. The survey_df contains the result the feedback for the question.

- Took over view of the both dataFrame in the notebook.
- Visulized null value in the notebook to idenfity the columns and there presence in the columns itself.
- Visulize the correlation with the help of heatmap, to identify relations.

## Data Understanding
The given dataset contains two file, one contains the survey data and second contains the schema of the dataset.

**Survey dataset:** The aim of the survey was to understand the job-related aspect in the development field so based on feedback collected in the the survey data set contains 51392 rows with 154 columns outof which only these columns has no missing values, namaly Respondent, Professional, ProgramHobby, Country, University, EmploymentStatus, FormalEducation in total 7, however 147 columns contains missing value in it so we have to perform some data wraggling methods to fix that issue.

**Schema dataset:** In the schema dataset contains questions which were asked during the survey, It has all the columns which match with survey dataset columns and question related to the column name, it contains, 154 questions with two cloumns, namaly column and question. Some of the question contains multiple selection in the survey dataset that we need to take care in frequecy graph or analysis. 

### Prepare Data
Need to prepaer the data to answer the above questions, however there are alot of the columns which needs to takle based on logic, So I'm selecting the columns based on the question which need to be explore. 

-  **Does employment status related to job satisfaction? **

The above question is related to **EmploymentStatus and JobSatisfaction**, so we are exploring the columns and found that no missing values for EmploymentStatus however found 21.4% missing values in JobSatisfaction columns, one way to drop those row but that will leads to less data to explore relatively, second is to take the means of the JobSatisfaction and fill the missing value with that, that will help us to be use most of the data for analysis so that we can pull out the most of it. 

```console
# code findout missing values, 0 means no missing, otherthen that represent proportion of missing values
> survey_df['EmploymentStatus'].isnull().mean()
> 0

> survey_df['JobSatisfaction'].isnull().mean()
> 0.214
# 0.214, Contains 21.4% missing values

# filling the values with mean
> survey_df['JobSatisfaction'] = survey_df['JobSatisfaction'].fillna(survey_df['JobSatisfaction'].mean())
> survey_df['JobSatisfaction'].isnull().mean()
> 0
# 0, means all the value got filled with mean of the JobSatisfaction column.
```
- **Are you a hobby programmer or/and contributor to open source projects, how it is related to job satisfaction?**

The question maily takes about the programming as hobby, based on that we want to find the connection between the job satisfaction and what they are doing as hobby, if there is match between both of them, let's prepare for that.

```console
# Code findout missing values, 0 means no missing
> survey_df['ProgramHobby'].isnull().mean()
> 0

# So the above code we already filled the na
> survey_df['JobSatisfaction'].isnull().mean()
> 0
```

- **Does Salary gives you job satisfaction?**

Find out the salary the relationship between Salary and JobSatisfaction, The salary columns contains **~75%** missing values, and **~25%** in the data conatains values in it, at that position we have two options, first is to deleted or fill the mean of the salary. If we fill mean value that can mislead the outcome as we have few values in the salary columns, so clearly better option is to deleted the missing values and compair the with job satisfaction.

```console
# Code findout missing proportion in column
> survey_df['Salary'] = survey_df['Salary'].isnull().mean()
> 0.7491

# drop missing values in salary column 
> drop_sal = survey_df.dropna(subset=['Salary'])
> survey_df['Salary'].isnull().mean() 
```

- **Is empoyment status has relation to job statisfaction?**
Reviewed the JobSatisfaction and EmploymentStatus in the survey_df. That turn out be no missing value in EmploymentStatus, however I found 0.2143 missing values in 
JobSatisfaction column, so filled all the NA value of the JobSatisfaction by filling in the mean of the column, by doing so I got the appropate dataframe to find the relation in between. 

Output
```console
EmploymentStatus

Independent contractor, freelancer, or self-employed    7.202299
Not employed, and not looking for work                  6.957094
I prefer not to say                                     6.957078
Retired                                                 6.957078
Not employed, but looking for work                      6.957078
Employed full-time                                      6.928428
Employed part-time                                      6.879209
Name: JobSatisfaction, dtype: float64
```
**JobSatisfaction and EmploymentStatus**
![plot](jobsatisfaction_and_employmentStatus.png)

In the above results, It's clearly idendicate that the Independent contractor, freelancer, or self-employed are statisfied in the group.

### Are you a hobby programmer or/and contributor to open source projects, how it is related to job satisfaction?

It's interesting the have look on the hobbies with the professional outcome as the job satisfaction. I followed below step to find out the outcome.
1. finding out the null values in the ProgramHobby and JobSatisfaction.
2. Found that there were no entries where missing.
3. Group by based on the hobby and their satisfaction ratiing. 
4. Once everything, got the result of the analysis. 

Output
```console
ProgramHobby

Yes, both                                    7.108041
Yes, I contribute to open source projects    7.041535
Yes, I program as a hobby                    6.913072
No                                           6.833825
```
** hobby programmer, contributor and EmploymentStatus**
![plot](hobby_and_employmentStatus.png)

Result shown that the most of the satisfied people have hooby as programming or they contribute to the open source project.


### Does Salary gives you job satisfaction?
It's always seems like the salary give satisfaction, let's find with the given dataset, alot of salary information as missing, there could be a answer to fill the dataset with the mean of the value however i think, it's not good idea to fill the mojor chunk with the help of the given sample value, in the other hand, i could be possible but may lead to different conclusions.

1. Checked for the missing values.
2. Major chunk is missing, decide to drop missing values.
3. Plot the relationship between JobSatisfaction and Salary

```console
JobSatisfaction

0.0     47111.799610
1.0     48100.740801
2.0     48289.403289
3.0     49952.709955
4.0     47201.877289
5.0     49478.845404
6.0     53221.769136
7.0     54093.964298
8.0     59485.961163
9.0     64089.922631
10.0    61440.444486
```
**hobby programmer, contributor and EmploymentStatus**

![plot](salary_and_employmentStatus.png)

There are avidence that salary gives motivation towards job satisfaction, both are proportional to each other.

### Conclusion

We talk about the parameter and job satisfaction based on the data provided by StackOverflow, It clearly shows the following points.
- **If your passions and work are mutual, then developer as a professional career is a great choice.**
- **Non-conventional methods like freelancing and contracting will give more satisfaction compared to other employment types.**
- **Salary gives motivation towards job satisfaction, both are equally proportional to each other.**
