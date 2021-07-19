# JobsDB HK Web Scrape
> A project to analyze **IT job market** in Hong Kong by web scraping [hk.jobsdb.com.](https://hk.jobsdb.com/hk?utm_campaign=hk-c-ao-[c]_dbhk_google_all_sem_brand_brand_en_exact_ao&utm_source=google&utm_medium=cpc&utm_term=&pem=google&gclid=Cj0KCQjw_8mHBhClARIsABfFgpgl6mFDkwSQ6cA0qzNhyC_mcrNR1joV9NfHVt-SKniEbKZwVdbx7dQaAmwqEALw_wcB) 

<img src="images/Screenshot%20(82).png">

## Introduction
As IT industry is growing day by day, IT jobs are at high demand of all time. There have many people are interested in getting into the industry but don't know much about the job market. On the other hand, more academic platforms, training centers are investing in IT related courses.

Therefore, this project is to analyse the Hong Kong IT job market, provide the market insights for people who are keen to be a part of it. 
To achieve that, we will web scrape the IT job posts on hk.jobsdb.com to find the demand, requirements and salaries of different IT jobs.

## Table of Contents
* [Introduction](#introduction)
* [Technologies Used](#technologies-used)
* [Data Collection](#data-collection)
* [Data Preprocessing](#data-preprocessing)
* [Analysis](#analysis)
* [Conclusion](#conclusion)
* [Challenges](#challenges)
* [Next Steps](#next-steps)


## Technologies Used
- Python 3.8.5
- Pandas 1.1.3
- Beautifu Soup 4.9.3
- Requests 2.24.0
- Regex 2020.10.15
- Matplotlib 3.3.2
- Seaborn 0.11.0

## Data Collection
We used BeautifulSoup and Requests to do web scraping. After selecting **Information Technology (IT)** as **Job Function**, we scraped all the job posts URL on each page then scraped information of each job post. 

JobsDB have limitation of only showing job posts for up to the last 30 days so we ended up having 7000+ job posts.
```
title.append(job.find("div", {"data-automation":"detailsTitle"}).h1.get_text())
company.append(job.find("div", {"data-automation":"detailsTitle"}).span.get_text())

j_info=job.find_all(class_="FYwKg _11hx2_0")
info_list=[i.getText() for i in j_info] 
location.append(info_list[0])
salary.append(info_list[1])
post_date.append(info_list[-1])

add_info=job.find_all(class_="FYwKg zoxBO_0")
add_list=[i.getText() for i in add_info]
filtered_add_list=add_list[4:12]
career_level.append(filtered_add_list[1])
qualification.append(filtered_add_list[3])
experience.append(filtered_add_list[5])
job_type.append(filtered_add_list[7])
industry.append(add_list[-3])
```
We collected 10 information for our analysis, included:
- Job Title
- Company
- Location
- Salary
- Posted Date
- Career Level 
- Qualification
- Experience
- Job Type
- Industry

## Data Preprocessing
We dropped duplicated job posts and Unnamed column. 

5322 jobs left after dropping duplicated job posts.

```
df=df.drop("Unnamed: 0", axis=1)
df.drop_duplicates(inplace=True)
```
Since some of the salaries showed with 'HK$' at the beginning, e.g. **HK$20,000 - HK$25,000/month** or **Above HK$18,000/month**, we needed to extract only digit and calculate the mean if the salary is showed in a range for later analysis.

```
df["salary"]=df["salary"].apply(lambda x: x.replace(",","").replace("HK$",""))
df["salary"]=df["salary"].apply(lambda x: "Not Specified" if "Posted" in x else x)
df["salary"]=df["salary"].apply(lambda x: str(re.search('(\d+)((\s)(-)(\s)(\d+))?',x).group()) if "/" in x else x)
def average(x):
    if "-" in x:
        y = x.split(" - ")
        z=0
        for i in y:
            z+=int(i)
        return round(z/2)
    else:
        return x
df["salary"]=df["salary"].apply(lambda x: average(x))
```
For Posted Date, all started with 'Posted on' so we extracted only the date and turned it into Datetime data type.

```
df["post_date"]=df["post_date"].apply(lambda x: re.search("\d.+",x).group())
df['post_date']=pd.to_datetime(df['post_date'])
```
Since the main target customers for this project are people who are new to the IT industry, the entry level job market is their main concern. Therefore, we created four columns for better demonstration.
- intership - Is it an internship or not?
- ft/pt - Is it a full-time or part-time job?
- data - Is it a data-related job? (because we are data science student)
- entry- Is it an entry level job?
```
df["internship"]=df["job_type"].apply(lambda x: "1" if "Internship" in x else "0")
df["ft/pt"]=df["job_type"].apply(lambda x: "p" if "Part Time" in x else "f")
df["data"]=df["title"].apply(lambda x: "1" if x.lower().find('data') >=0 else "0")
df["entry"]=df["career_level"].apply(lambda x: "1" if x.lower().find('entry level') >=0 else "0")
```
Here is the final data frame:

<img src="images/Screenshot%20(83).png">

## Analysis
1. Entry Level jobs
There had over 30% jobs were classifered as 'entry level' which is a good news for our target customer.

<img src="images/Screenshot%20(85).png" width="650" height="600">

2. Internship
There had 70 internships out of 5322 jobs. It seemed such a small number but consider that 70 internships opened within 30 days is still pretty good.

<img src="images/Screenshot%20(90).png" width="750" height="600">

3. Salary
It showed that the salary of entry level jobs range from HK$13,000 to HK$38,000/month. When the top 3 industries are 'Financial Services', 'Education' and 'Information Technology (IT)'.

<img src="images/Screenshot%20(88).png" width="1000" height="800">

4. Qualification
Almost 50% of entry level jobs required a Degree, over 38% accept 'Non-degree Tertiary'.

<img src="images/Screenshot%20(89).png" width="750" height="600">

More analyses can find in.

## Conclusion
To conclude, we found that there was no sign of shortage for entry level positions. The market welcome candidates whose are new to the industry and provided enough opportunities. Also, the industry was staying competitive while most of the entry level jobs provided salary over 20k per month. However, even a degree was not a must for entering IT industry, training or education is still recommended in order to enhance the chance of getting hired or landing a better paid job. 

## Challenges
1. Web Scraping - information provided in each job posts were different. Information mixed up or collected in the wrong columns by using the indexing method so we needed to change them manually sometimes and it's not ideal.
2. Many data were not provided by the companies which made the analysis not comprehensive and representative enough.

## Next Steps
1. Improve web scraping code to increase efficiency and fit for all job posts.
2. Web scrape more job hunt websites to increase data base for more accurate analysis.
