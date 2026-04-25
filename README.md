# Case-Study-Data-Job-Market-Skill-Optimization-Analysis


## Background on the Case Study

I recently completed the Google Data Analytics Certificate through Coursera and at the end of the course is instructions for an optional Case Study Capstone Project. Of the three options, it was the last option of a personal case study that stood out to me. I have spent the last couple of months working with a free dataset I found online which I analyzed using different tools to build myself a portfolio. The dataset contains detailed information on the data jobs market and has been very helpful to me in my journey towards learning more about data analytics. As such, I have decided to base my first case study on my previous project in which I used SQL to answer real data market questions. I will be turning that data into charts and graphs that I'll use to create a presentation, completing this personal case study.


# Introduction

As this is a personal case study, I have tasked myself with the following question: What is the most efficient way to become a skilled data analyst?

In other words, I need to first understand the data job market. Then I need to find the skills required to be a Data Analyst. Lastly, I need to create a presentation to share my findings and insights.

As mentioned before, I will be using the same dataset from my previous project, which you can find in my GitHub repository titled [SL_Project_3_SQL_Data_Job_Analysis](https://github.com/LopezSamir/SL_Project_3_SQL_Data_Job_Analysis). I will be using the already set-up tables from that project so that I don't have to make it all over again.

# Step 1: What Questions am I looking to Answer

1. What is the average salary of a remote data analyst role in the United States?
2. What are the most in-demand skills for remote data analyst roles in the United States?
3. Which skills are associated with higher salaries?
4. What are the most optimal skills to learn?

Once I have answered all of these questions using SQL, I will to answer my main question of:

What is the most efficient way to become a skilled data analyst?

## 1: What is the average salary of a remote data analyst role in the United States?

I made the following sql query in order to find the average salary of all remote data analyst jobs in the US.

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














