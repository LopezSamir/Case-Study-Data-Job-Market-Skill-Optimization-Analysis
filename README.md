# Case-Study-Data-Job-Market-Skill-Optimization-Analysis

This project is a work in progress with the general plan being:

* Use SQL to analyze the dataset from my last project on the data job market.
* Visualize my data analysis/queries
* Create a written report and add these visualizations
* Upload Written report to GitHub Portfolio and this README


## Background on the Case Study

I recently completed the Google Data Analytics Certificate through Coursera and at the end of the course is instructions for an optional Case Study Capstone Project. Of the three options, it was the last option of a personal case study that stood out to me. I have spent the last couple of months working with a free dataset I found online which I analyzed using different tools to build myself a portfolio. The dataset contains detailed information on the data jobs market and has been very helpful to me in my journey towards learning more about data analytics. As such, I have decided to base my first case study on my previous project in which I used SQL to answer real data market questions. I will be turning that data into charts and graphs that I'll use to create a presentation, completing this personal case study before moving on to another case study that better follows the exact instrustions from the Google Data Analytics Certificate course.


# Introduction

This case study project is the culmination of my work over the last 3 months (Feb-Apr 2026), in which I built a portfolio of projects focused on the data analyst job market. As this is a personal case study, I have tasked myself with the following question: What is the most efficient way to become a skilled data analyst?

In other words, I need to first analyze the data job market. Then I need to visualize my findings. Lastly, I need to create a presentation to share my findings and insights.

As mentioned before, I will be using the same dataset from my previous project, which you can find in my GitHub repository titled [SL_Project_3_SQL_Data_Job_Analysis](https://github.com/LopezSamir/SL_Project_3_SQL_Data_Job_Analysis). I will be using the already set-up tables from that project so that I don't have to make it all over again.

# Data Analysis using SQL

## What questions am I looking to answer with SQL queries?

1. What is the average annual salary for remote Data Analyst positions that have a listed salary?
2. What are the 5 skills most frequently requested in remote Data Analyst job postings?
3. Which skills are associated with higher salaries?
4. What are the most optimal skills to learn?

## 1. What is the average annual salary for remote data analyst positions that have a listed salary?

In order to find the average salary of all remote data analyst jobs in the US, I created the following query:

```sql
SELECT
    ROUND(AVG(salary_year_avg), 2) AS Average_Salary
FROM
    job_postings_fact
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
```

<img width="119" height="68" alt="average salary" src="https://github.com/user-attachments/assets/752de5f0-dd11-4e17-b285-55d2a3331a8d" />

### Key Takeaway

* The average salary for remote data analysts in the US is $94,770. 

## 2. What are the 5 skills most frequently requested in remote data analyst job postings?

In order to find the most demanded skills for data analysts, I created the following query:

```sql

SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5

```

<img width="214" height="134" alt="most demanded skills" src="https://github.com/user-attachments/assets/cf963735-3c0b-45eb-aae2-28660b5e79df" />

### Key Takeaways

* SQL has 7,291 mentions, which is nearly 58% more than the second-place skill (Excel). SQL's demand count makes it a non-negotiable skill for any aspiring data analyst.
* Excel (4,611) only narrowly edges out Python (4,330). Both are core skills for data analysts, however the versatility offerred by Python makes it the more desired skill for analysts who are later in their career. For newer analysts, it is best to learn both and focus on Excel over Python until they have enough experience to require Python in a work environment.
* Tableau (3,745) leads Power BI (2,609) by 44%, suggesting Tableau is still the dominant BI tool in job postings. A new analyst should learn the basics of both, while focusing more on the one they prefer, or the one used at their place of work.

## 3. Which skills are associated with higher salaries?

In order to find which skills come with the highest salaries, I created the following query:

```sql

SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25

```

<img width="202" height="488" alt="25 highest paying skills" src="https://github.com/user-attachments/assets/0a89078a-5b43-4b95-bc75-9524835b00ef" />

### Key Takeaway 1

* The high-paying skills require foundational knowledge first. You can't effectively use PySpark without knowing Python. You can't work with Databricks without understanding SQL.

These skills initially seemed very appealing to me to learn as soon as possible as a new analyst, however I needed to first get a better picture of just how valuable they are. So, I edited the query to show not only the average salary of the top 25 highest paying skills, but also how often they are demanded for data analyst roles.

```sql

SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25

```

<img width="281" height="483" alt="25 highest paying skills with demand count" src="https://github.com/user-attachments/assets/60a270af-ef57-45fd-bac2-572d123fc802" />

### Key Takeaway 2

* The top 25 highest paying skills are too uncommon for a new analyst to learn in hopes of being employed. These skills are for senior level analysts that fill extremely specific roles. The highest paying skill which is pyspark ($208,172) is mentioned in job postings only 2 times. It is a similar story for the rest of the top 25.

## 4. What are the most optimal skills to learn?

In order to find the most optimal skills to learn, I created the following query:

```sql

SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
Where
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25

```

<img width="401" height="557" alt="most optimal skills" src="https://github.com/user-attachments/assets/306ec5d6-3826-445e-b42a-a354406af6d1" />

This query has a useful output, however I noticed a few things that I don't like about it: 

1. The demand count is still too low for me to consider some of these skills.
2. I don't really need a skill_id column for this.
3. I also need to get rid of duplicates in the output results for sas.

So I rewrote the query into the following:

```sql

SELECT
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skills
HAVING
    COUNT(skills_job_dim.job_id) > 20
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 15

```

<img width="341" height="311" alt="most optimal skills fixed" src="https://github.com/user-attachments/assets/3a252e5b-4265-4c0d-8119-034f207fe550" />

### Key Takeaways

* SQL has the highest demand (398) out of any skill that appeared on a job posting with a given salary. On top of that it also has a high average salary of $97,237. It ranks number 1 on my list of most optimal skills for an entry-level data analyst to focus on.
* Number 2 on my list of most optimal skills for an entry-level data analyst to focus on is Tableau/Power BI. It is mandatory for any good analyst to be proficient at visualizing their data, and these two are the leading choices.
* To round off the top 3, ranking 3rd on my list of most optimal skills for an entry-level data analyst to focus on is Python. Python had the second highest demand (236) on the list while also having a higher average salary than SQL, at $101,397.

# Visualizing SQL Queries with Tableau/PowerBI

In progress - Visualizations coming soon.

For this project, I will be using Power BI as my choice of visualization tool.


## 1. Save query outputs as csv files

<img width="220" height="83" alt="save outputs as csv files" src="https://github.com/user-attachments/assets/563e1d4b-1f5b-42ed-b2bd-40bf4d741e20" />

## 2. Import csv files into an excel file


<img width="319" height="92" alt="import csv into excel" src="https://github.com/user-attachments/assets/6e32c2e4-26c3-410c-9fc9-c3b1404c63d9" />




<img width="589" height="905" alt="csv files in excel" src="https://github.com/user-attachments/assets/f7b94c47-02b8-4916-a2a9-41ff72bb3374" />

## 3. Import excel file into Power BI


<img width="136" height="149" alt="data in power bi" src="https://github.com/user-attachments/assets/784433dd-3b0e-48a6-b70b-299836b1362c" />

## 4. Make Visualizations for each query


<img width="973" height="560" alt="initial dashboard" src="https://github.com/user-attachments/assets/557c20bf-f743-49db-adc3-02135d5ba37f" />

## 5. Add a background and borders, and align the visualizations to look more like a dashboard.

<img width="979" height="553" alt="dashboard V02" src="https://github.com/user-attachments/assets/ac7a8823-9460-4246-b14d-50be903adb4d" />

## 6. Make it cleaner and easier to read/understand

<img width="962" height="541" alt="Dashboard V03" src="https://github.com/user-attachments/assets/689ecdba-126d-4037-8c78-d46a35eb6861" />

## 7. Capitalize skills

<img width="1007" height="529" alt="start capitalizing skills" src="https://github.com/user-attachments/assets/e6871d27-e6ca-43c6-b6b8-acaf99724e96" />

## 8. Final Dashboard 


<img width="968" height="551" alt="Final Dashboard Screenshot" src="https://github.com/user-attachments/assets/8ec74488-c3ee-4037-9ad8-ef727dbc988e" />






# Conclusion

This case study set out to answer one question: What is the most efficient way to become a skilled data analyst?

The data made the answer clear. SQL is the single most important skill to learn first. It has the highest demand of any skill at 398 postings and a strong average salary of $97,237. Python and Tableau/Power BI round out the core three, and together these skills represent the optimal foundation for any new analyst looking to break into the field. Once those are solid, cloud tools like Snowflake, Azure, and AWS are the next logical step. They consistently pay $10–15K more than the foundational skills and have enough demand to be worth learning. The top 25 highest-paying skills like PySpark may look appealing, but the data shows they appear in too few postings to be a practical target for someone just starting out like me. 

My plan for the future is: Learn the core three to get hired, then add cloud skills to get paid more once I have gained experience working as an analyst.












