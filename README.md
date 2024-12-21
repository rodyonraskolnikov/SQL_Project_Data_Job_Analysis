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

### 2. Skills for Top Paying Jobs
To identify the skills needed for the highest-paying jobs, I combined job postings with skills data to uncover insights into what employers prioritize for high-compensation positions.
```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
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
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```
Here’s a summary of the most in-demand skills for the top 10 highest-paying data analyst roles in 2023:

- **SQL** takes the lead, appearing in 8 of the roles.
- **Python** is a close second, featured in 7 positions.
- **Tableau** is also highly valued, being listed in 6 roles.
Other skills, such as **R**, **Snowflake**, **Pandas**, and **Excel**, show varying levels of demand.

![Top Paying Skills](assets_new/2_top_paying_roles_skills.png)
*Bar chart illustrating the frequency of skills in the top 10 highest-paying data analyst roles; this chart was created by ChatGPT based on the results of my SQL query.*

### 3. In-Demand Skills for Data Analysts

This query highlighted the skills most commonly mentioned in job postings, emphasizing areas with the highest demand.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Here’s an overview of the most in-demand skills for data analysts in 2023:

- **SQL** and **Excel** continue to be core competencies, highlighting the importance of strong fundamentals in data processing and spreadsheet management.
- **Programming** and **Visualization Tools** such as **Python**, **Tableau**, and **Power BI** are critical, reflecting the growing significance of technical expertise in data visualization and decision-making.

| Skills   | Demand Count |
|----------|--------------|
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

*Table showcasing the demand for the top 5 skills in data analyst job postings*

### 4. Skills Based on Salary
Analyzing the average salaries linked to various skills uncovered the highest-paying ones.
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
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
Here’s a summary of the findings on top-paying skills for Data Analysts:

- **Strong Demand for Big Data and Machine Learning Skills:** Analysts with skills in big data tools (PySpark, Couchbase), machine learning platforms (DataRobot, Jupyter), and Python libraries (Pandas, NumPy) command the highest salaries, emphasizing the value of data processing and predictive modeling capabilities in the industry.

- **Proficiency in Software Development and Deployment:** Expertise in tools like GitLab, Kubernetes, and Airflow highlights a lucrative intersection of data analysis and engineering, with high demand for skills that enable automation and seamless data pipeline management.

- **Cloud Computing Skills in High Demand:** Knowledge of cloud and data engineering tools (Elasticsearch, Databricks, GCP) reflects the increasing reliance on cloud-based analytics, showcasing how cloud expertise can significantly enhance earning potential in the field.

| Skills        | Average Salary ($) |
|---------------|-------------------:|
| pyspark       |            208,172 |
| bitbucket     |            189,155 |
| couchbase     |            160,515 |
| watson        |            160,515 |
| datarobot     |            155,486 |
| gitlab        |            154,500 |
| swift         |            153,750 |
| jupyter       |            152,777 |
| pandas        |            151,821 |
| elasticsearch |            145,000 |

*Table displaying the average salaries for the 10 highest-paying skills for data analysts*

### 5. Most Optimal Skills to Learn

This query combined demand and salary data to identify skills that are both highly sought-after and well-compensated, providing a strategic direction for skill enhancement.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|-------------------:|
| 8        | go         | 27           |            115,320 |
| 234      | confluence | 11           |            114,210 |
| 97       | hadoop     | 22           |            113,193 |
| 80       | snowflake  | 37           |            112,948 |
| 74       | azure      | 34           |            111,225 |
| 77       | bigquery   | 13           |            109,654 |
| 76       | aws        | 32           |            108,317 |
| 4        | java       | 17           |            106,906 |
| 194      | ssis       | 12           |            106,683 |
| 233      | jira       | 20           |            104,918 |

*Table of the most lucrative skills for data analysts, ranked by salary*

Here’s an overview of the most essential skills for Data Analysts in 2023:
- **In-Demand Programming Languages:** Python and R are among the most sought-after, with demand counts of 236 and 148, respectively. Their average salaries, $101,397 for Python and $100,499 for R, suggest these skills are highly valued yet widely accessible.
- **Cloud Tools and Technologies:** Expertise in platforms like Snowflake, Azure, AWS, and BigQuery is in high demand and commands strong salaries, reflecting the increasing reliance on cloud solutions and big data tools in analytics.
- **Business Intelligence and Visualization Tools:** Tools like Tableau and Looker, with demand counts of 230 and 49, and average salaries of $99,288 and $103,795, respectively, underscore the importance of turning data into actionable insights through visualization and business intelligence.
- **Database Technologies:** Proficiency in traditional and NoSQL databases (e.g., Oracle, SQL Server, NoSQL) remains critical, with average salaries between $97,786 and $104,534, highlighting the continued need for robust data storage and management skills.

# What I learned

Throughout this journey, I have significantly enhanced my SQL skillset with advanced capabilities:

- **🧩 Advanced Query Development:** Gained expertise in crafting complex SQL queries, seamlessly merging tables, and utilizing WITH clauses for efficient temporary table management.
- **📊 Data Aggregation Proficiency:** Developed strong skills in using GROUP BY and leveraging aggregate functions such as COUNT() and AVG() to effectively summarize data.
- **💡 Analytical Expertise:** Enhanced problem-solving abilities by transforming real-world questions into insightful and actionable SQL queries.

# Conclusions

### Insights

Key insights from the analysis include:

1. **Top-Paying Data Analyst Roles:** Remote data analyst positions offer a wide salary range, with the highest reaching an impressive $650,000.
2. **Critical Skills for High Salaries:** Advanced SQL proficiency is a key requirement for securing the highest-paying roles, making it a crucial skill to master.
3. **Most Sought-After Skills:** SQL stands out as the most in-demand skill in the data analyst job market, emphasizing its importance for job seekers.
4. **Skills Linked to Higher Earnings:** Niche expertise in areas like SVN and Solidity is associated with the highest average salaries, highlighting the value of specialized knowledge.
5. **Optimal Skills for Career Growth:** SQL emerges as a top skill, combining high demand with a strong average salary, making it a strategic choice for data analysts aiming to maximize their market value.

### Closing Thoughts

This project improved my SQL expertise and offered valuable insights into the data analyst job market. The analysis results provide a roadmap for prioritizing skill development and streamlining job search strategies. By focusing on skills that are both in high demand and command competitive salaries, aspiring data analysts can strengthen their position in a competitive job market. This exploration underscores the importance of ongoing learning and staying adaptable to evolving trends in data analytics.

Finally, special thanks to the great [Luke Barousse](https://lukebarousse.com/), whose guidance made this project possible.

