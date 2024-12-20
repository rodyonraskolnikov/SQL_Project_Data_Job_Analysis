# Introduction
📊 Explore the data job market! This project focuses on data analyst roles, uncovering 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salaries in data analytics.

🔍 Curious about SQL queries? Find them in the [project_sql folder](/project_sql/)!

# Background
This project was created using the educational project content offered by Luke Barousse’s [SQL Course](https://lukebarousse.com/sql). The project focuses on analyzing the data analyst job market to identify top-paying and in-demand skills, providing a streamlined approach for others to discover optimal career opportunities.

The data provides insights into job titles, salaries, locations, and essential skills. It is sourced from Luke's course.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
To conduct a thorough exploration of the data analyst job market, I utilized several essential tools:

- **SQL:** The foundation of my analysis, enabling me to query the database and extract valuable insights.

- **PostgreSQL:** The database management system selected for efficiently storing and managing job posting data.

- **Visual Studio Code:** My preferred tool for managing databases and running SQL queries.

- **Git & GitHub:** Used for version control, sharing SQL scripts, and tracking project updates.

# The Analysis
Every query in this project was designed to explore specific elements of the data analyst job market. Here is the approach I took for each question:

### 1. Top Paying Data Analyst Jobs
To determine the highest-paying roles, I filtered data analyst positions based on average annual salary and location, with an emphasis on remote jobs. This query showcases lucrative opportunities within the field.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg, 
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
 WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

Here's an overview of the top data analyst roles in 2023:

- **Salary Range:** The top 10 highest-paying data analyst positions offer salaries ranging from $184,000 to $650,000, highlighting the field's strong earning potential.

- **Diverse Companies:** Employers such as SmartAsset, Meta, and AT&T are among the organizations providing competitive salaries, showcasing opportunities across various industries.

- **Variety in Titles:** The roles range from Data Analyst to Director of Analytics, emphasizing the wide array of positions and specializations available within the data analytics field.

![Top Paying Roles](assets_new/1_top_paying_roles.png)
*Clustered bar chart showing the average yearly salary for each job title and company; ChatGPT generated this graph using JSON of my SQL query results*

# What I Learned
# Conclusions
