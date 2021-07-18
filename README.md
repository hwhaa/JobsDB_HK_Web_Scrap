# JObsDB_HK_Web_Scrap
> A project to analyze IT job market in Hong Kong by web scraping [hk.jobsdb.com.](https://hk.jobsdb.com/hk?utm_campaign=hk-c-ao-[c]_dbhk_google_all_sem_brand_brand_en_exact_ao&utm_source=google&utm_medium=cpc&utm_term=&pem=google&gclid=Cj0KCQjw_8mHBhClARIsABfFgpgl6mFDkwSQ6cA0qzNhyC_mcrNR1joV9NfHVt-SKniEbKZwVdbx7dQaAmwqEALw_wcB) 

## Introduction
As IT industry is growing day by day, IT jobs are at high demand of all time. There have many people are interested in getting into the industry but don't know much about the job market.
Therefore, this project is to analyse the Hong Kong IT job market, provide the market information for people who are keen to be a part of the it. 
To achieve that, we will web scrape the IT job posts on hk.jobsdb.com to find the demand, requirements and salaries of different IT jobs.

## Table of Contents
* [Introduction](#introduction)
* [Technologies Used](#technologies-used)
* [Data Collection](#data-collection)
* [Data Preprocessing](#data-preprocessing)
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
We used BeautifulSoup and Requests to do web scrapping. After selecting **Information Technology (IT)** as **Job Function**, we scraped all the job posts URL on each page then scraped information of each job post. 
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

## EDA

## Conclusion

## Challenges

## Next Steps
