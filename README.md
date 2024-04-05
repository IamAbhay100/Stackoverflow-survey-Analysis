<img src="https://stackoverflow.design/assets/img/logos/so/logo-stackoverflow.png" align="left" height="100" width="450" >
<br>
<br>
<br>
<br>
<br>
<br>

# <a name="1 Project Description">Project description:</a>


Stack overflow is a professional community for developers. They conduct developer surveys every year since 2011, and the collected data is available open-source on the web. The latest dataset 2020 was released on March 5th, 2021. With proper analysis, the Dataset would help us to answer real-world questions. For instance, we can find the most popular language that the developers use.We also can find the developer role which pays the highest salary. Our project is to analyze the last year developer survey and gather meaningful insights from it.

As a first step, we will clean the data by removing null values and outliers in each column. Then, refactor the columns in such a way that help us in analysis. Then we performed data analysis on the cleaned dataset.

# <a name="2 Data Source">Data source:</a>

The dataset is very diverse and came from a Stack overflow developer survey from 180 countries. Stack overflow has data collected through surveys from 2011 to 2024 We choose 2021 survey to analyze for the projects. The participants mostly from the US, India, and EMEA regions. The majority of the survey respondents had a background of developer/ coding experience. We performed various analysis and our key results are given in the `Data Analysis` section.

Dataset can be downloaded from the mentioned below link:

**Download Link** ->   "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/LargeData/m1_survey_data.csv"

here data are available in the CSV format ranging from 40 to 150 MB with data of 1.5 Lakh survey participants.
The reason why we chose this dataset is because of its diverse nature and it was completely uncleaned.  We, as a developer, use Stack overflow to find answers for most of the questions we get. That encouraged us to explore and derive key insights from the survey results. Also, the Insights can be used for a better understanding of the information technology and hiring employees and job seekers for preparing the career resume building.

# <a name="2 Data Source">Exploring Dataset:</a>

The Data set has 11552 rows and 85 columns. we can use `df.shape` method to find out the rows and columns number. 

# <a name="2 Data Source">Selected Columns:</a>

As with any large data sources we have to select subset from the dataset, We have selected a subset of 20 columns from our dataset for analysis. These columns include: `'Country', 'Age', 'Gender', 'EdLevel', 'UndergradMajor', 'OrgSize', 'YearsCode', 'YearsCodePro', 'JobSat', 'JobSeek', 'CurrencySymbol', 'LanguageWorkedWith', 'LanguageDesireNextYear', 'OpenSource', 'Hobbyist', 'Student', 'Age1stCode', 'WorkWeekHrs', and 'WorkLoc'`.

# <a name="4 Data Cleaning">Data Cleaning</a>

<img src="https://recodehive.com/wp-content/uploads/2021/05/Data-Cleaning-1024x361.png">

First, we will identify null values within our dataset across the selected columns. Then, we'll apply various approaches to handle these null values, such as replacing them with the` mean, median, or mode` of each respective column.

<img src="https://recodehive.com/wp-content/uploads/2021/05/Message-from-Founder-1024x576.png">
 
# <a name="5 Data Analysis and Visualization">Data Analysis and Visualization</a>

After cleaning and handling outliers in all three datasets, we started looking for valuable insights that we can draw from it.

<img src="https://recodehive.com/wp-content/uploads/2021/05/Message-from-Founder-1024x576.jpg">

# <a> Columns Spliting </a>

Some columns in the dataset may pose challenges for analysis, particularly those where survey questions allow for multiple selections as answers. Handling such columns may require special consideration and potentially different analytical techniques compared to columns with single-selection answers.

So we First break down the columns.
```
def split_multicolumn(col_series):
    result_df = col_series.to_frame()
    options = []

    for idx, value in col_series[col_series.notnull()].items():
        for option in value.split(';'):
            if not option in result_df.columns:
                options.append(option)
                result_df[option] = False

            result_df.at[idx, option] = True
    return result_df[options]
```


## <a name=" Distribution of respondents based on country"> Distribution of respondents based on country</a>

```
Country = df['Country'].value_counts().head(10)
plt.figure(figsize=(12,7))
sns.barplot( x=Country, y=Country.index)
plt.ylabel(None)
plt.title('Respondent and their Countries')
plt.xlabel('Numbers of Respondent');
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/31e4de1f-64a1-4a8b-b865-15bec87d55e8)

<p> As depicted in the graph, the data reveals that the United States boasts the highest number of respondents, totaling nearly 2800 responses. Following closely are India and the United Kingdom, indicating substantial participation from these regions.</p>

# Some of the key insights
## **Q :-** What is the most loved language this year and last year ?
```
most_loved_language = LanguageWorkedWith_df & LanguageDesireNextYear_df
most_loved_language_total = (most_loved_language.sum()*100 / LanguageWorkedWith_df.sum())
most_loved_language_total = most_loved_language_total.sort_values(ascending=False)
plt.figure(figsize=(12,12))
sns.barplot(x=most_loved_language_total, y=most_loved_language_total.index)
plt.title("Most loved Language")
plt.xlabel('Count')
plt.ylabel(None)
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/2ddf164d-6c26-4a7f-83b8-eb4190820b57)

## **Q :-** Countries with the highest Workweek hours also responses is more than 100 ?
```
workweek_hour = df.groupby('Country')[['WorkWeekHrs']].mean().sort_values('WorkWeekHrs', ascending=False)
highest_workweek_country = workweek_hour.loc[df.Country.value_counts() > 100].head(15)
plt.figure(figsize=(14,8))
sns.barplot(x=highest_workweek_country['WorkWeekHrs'],y=highest_workweek_country['WorkWeekHrs'].index)
plt.ylabel(None)
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/f402688e-3062-453e-baa5-24e9e9f17341)

## **Q :-** What is total years of experience in coding of the respondents, their age when they started coding, and whether they consider it as a hobby ?
```
plt.figure(figsize=(12,6))
sns.scatterplot(x=df['Age'], y=df['YearsCodePro'], hue=df['Hobbyist'], data=df)
plt.ylabel('Experience in years');
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/4970345d-10f5-45fc-ba14-eb3cd12b97f3)


# <a> Most used Programming languages among the respondents. </a>
```
LanguageWorkedWith_total= LanguageWorkedWith_df.mean().head(25).sort_values(ascending=False)*100
plt.figure(figsize=(12,12))
sns.barplot(x=LanguageWorkedWith_total, y=LanguageWorkedWith_total.index)
plt.ylabel('Languages')
plt.xlabel('Count')
plt.title('Programming Languages used last year')
plt.show()
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/f0378084-80b9-46b7-87ee-1e9e858c29c5)

# <a> Language Desire Next Year among the respondents. </a>
```
LanguageDesireNextYear_df = split_multicolumn(df.LanguageDesireNextYear)
LanguageDesireNextYear_total = LanguageDesireNextYear_df.mean().head(25).sort_values(ascending=False)*100

plt.figure(figsize=(12,12))
sns.barplot(x=LanguageDesireNextYear_total, y=LanguageDesireNextYear_total.index)
plt.ylabel('Languages')
plt.xlabel('Count')
plt.title('Programming Languages Desire Next Year')
plt.show()
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/bfe4f6c0-852a-45b5-9852-403ab2fa8cb5)



## <a> Age of Respondents </a>

<p> Age serves as a significant factor in categorizing individuals into specific age groups, enabling the analysis of population distribution across different age brackets. This segmentation facilitates the examination of the number of respondents within each age category, providing valuable insights into demographic patterns and trends.</p>

```
bins=np.arange(10,80,3)
sns.histplot(x=df['Age'],bins=bins,data=df)
plt.xlabel('Ages')
plt.ylabel('Number of Respondent')
plt.title('Agewise Respondent')
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/9cba9625-f82b-403e-9965-dccaa21621e7)

<p>The data depicted in the above chart offers insight into the distribution of respondents across age groups, revealing that individuals between the ages of 20 to 40 represent the largest proportion of participants.</p>


## <a> Distribution of respondent based on their Gender </a>

Gender represents a crucial aspect for analysis, as it provides insight into the demographic composition of the surveyed population. By examining gender distribution, we gain valuable understanding of the gender representation within the surveyed cohort, facilitating more comprehensive demographic analysis.

```
sns.set_style('darkgrid')

gender_count= df['Gender'].value_counts()
labels= ['Men', 'Women','Others']
plt.figure(figsize=(14,7))
plt.pie(x=gender_count,labels=labels,autopct='%1.1f%%')
plt.legend()
plt.show()
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/1ef7860f-0d42-4697-aa87-59702959ac03)

<p>The analysis of the chart indicates that 92.6 percent of respondents identify as men, 6.4 percent as women, and 1 percent as other genders.</p>

## <a> Total years of coding experience </a>

<p>In addition to capturing educational backgrounds, the survey also inquired about respondents' coding experience. Participants were asked to specify the number of years they have been coding.</p>

```
bins=np.arange(10,50,2)
bins
sns.histplot(x=df['YearsCode'],bins=bins, data=df)
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/585f1ec2-2c84-4958-9d4c-0ae0b7b97791)

<p>The data depicted in the above chart illustrates that respondents with 10 to 20 years of coding experience constitute the largest proportion of participants."</p>


## <a> Education Level of respondents </a>

<p> By examining the education levels of respondents, we can discern whether individuals possess a technical background, which aids in understanding whether they are coding with or without a professional educational foundation. This analysis offers valuable insights into the diversity of backgrounds among participants and their engagement in coding activities. </p>

```
sns.countplot(y=df['EdLevel'])
plt.xticks(rotation=75)
plt.title('Which of the following best describes the highest level of education that you’ve completed?')
plt.ylabel(None);
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/ab08f966-4f61-4979-a7e1-edcd67901721)

```
education_level = df['EdLevel'].value_counts()*100 / df['EdLevel'].count()
plt.figure(figsize=(12,8))
sns.barplot(x=education_level,y=education_level.index)
plt.title('Which of the following best describes the highest level of education that you’ve completed?')
plt.ylabel(None);
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/2caddf6b-355d-4a91-a674-05ade0bc3c61)

<p> The insights gleaned from the above chart indicate that 55% and 25% of respondents hold bachelor's and master's degrees, respectively, which aligns with expectations. However, it's notable that approximately 13% of respondents are engaged in coding despite lacking a formal degree, demonstrating a commendable commitment to skill development and professional growth outside traditional educational pathways.</p>


## <a> What was your main or most important field of study? </a>
```
UndergradMajor = df['UndergradMajor'].value_counts() * 100 / df['UndergradMajor'].count()
plt.figure(figsize=(10,8))
sns.barplot(x=UndergradMajor, y=UndergradMajor.index)
plt.ylabel(None)
plt.xlabel('Percentage %')
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/1e78892b-77c6-4fd6-869a-5ebe01da5fa4)

<p> The analysis of the chart reveals that approximately 65% of respondents are affiliated with the fields of computer science or computer engineering."</p>


## <a> What is the Organization Size of Respondents? </a>
```
org_size = df['OrgSize'].value_counts()*100 / df['OrgSize'].count()
sns.barplot(x=org_size,y=org_size.index)

#sns.countplot(y=df['OrgSize'])
plt.xlabel('Number of Respondent')
plt.ylabel(None)
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/2e3695f4-f6e2-4b3f-8764-98dc981ef5ba)

## <a> What is the Job satisfaction of the respondents? </a>
```
jobSat = df['JobSat'].value_counts() * 100 / df['JobSat'].count()
sns.set_style('darkgrid')
plt.figure(figsize=(12,5))
plt.pie(x=jobSat,labels=jobSat.index,autopct='%1.1f%%')
plt.show()
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/cdb817cb-4b66-409c-913a-8ebdc7088680)
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/f711e038-09da-4cd1-93fd-e71b963cdcd7)

<p>Approximately 18% of respondents expressed dissatisfaction with their current job, which may be attributed to factors such as high work pressure or inadequate salary, leading to a sense of discontentment.</p>

## <a> What is the Work Location of the respondents? </a>
```
WorkLoc = df['WorkLoc'].value_counts()* df['WorkLoc'].count() * 100
labels= ['Office', 'Home', 'Others']
sns.set_style('darkgrid')
plt.figure(figsize=(12,5))
plt.title('Work Location')
plt.pie(x=WorkLoc,labels=labels,autopct='%1.1f%%')
plt.show();
```
![image](https://github.com/IamAbhay100/Stackoverflow-survey-Analysis/assets/91376854/cc6ac050-09e8-4dd2-af96-95b156e50a09)

"The analysis of the chart indicates that 32% of respondents are currently working from home, which may be a result of the COVID-19 pandemic."















































