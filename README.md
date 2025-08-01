# Overview
  Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

  The data sourced from Luke Barousse's Python Course which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# Questions
Below are the questions I want to answer in my project:

- What are the skills most in demand for the top 3 most popular data roles?
- How are in-demand skills trending for Data Analysts?
- How well do jobs and skills pay for Data Analysts?
- What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I've used!!

1) Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
  - Pandas Library: This was used to analyze the data.
  - Matplotlib Library: Used to visualize the data.
  - Seaborn Library: Helped me create more advanced visuals.
2) Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
3) Visual Studio Code: My go-to for executing my Python scripts.
4) Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Analysis

## 1) What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills i should pay attention to depending on the role i'm targeting.

![alt text](image.png)

### Visualize Data

```python
fig, ax =plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
  df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
  sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], palette='dark:b_r', hue='skill_count')
  ax[i].set_title(job_title)
  ax[i].set_ylabel('')
  ax[i].set_xlabel('')
  ax[i].legend().set_visible(False)
  ax[i].set_xlim(0, 78)

  for n, v in enumerate(df_plot['skill_percent']):
    ax[i].text(v+1, n, f'{int(v)}%', va='center')

  if i != len(job_titles) -1:
    ax[i].set_xticks([])


fig.suptitle('Likelihood of Skills Requested in India Job Postings', fontsize=16)
fig.tight_layout(h_pad=0.5)
plt.show()
```

### Results

![alt text](image-1.png)

### Insights
- Python is a versatile skill, highly demanded skill across all three roles, but most prominently for Data Scientist (62%)
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for noth roles.
- Data Engineers require more sprecialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis toils (Excel, Tableau)


## 2) How are in-demand skills trending for Data Analysts in India?
For this analysis I began by filtering the dataset to focus specifically on data analyst roles in the United States. To analyze trends over time I created a new column to represent the month of each job posting. Next, I used exploding techniques to break down individual skills listed within each posting, ensuring that every skill could be analyzed independently.

I then built a pivot table, organized by monthly skill counts, which also included total counts to rank the most frequent skills. Using these totals I calculated percentage-based values for further analysis. Finally, I visualized the top five most in-demand skills by plotting their percentage occurrence across each month in 2023, revealing key trends in skill demand over time.

``` Python

# Step 1: Convert the index (month numbers) into a column
df_DA_percent = df_DA_percent.reset_index()

# Step 2: Convert month numbers to abbreviated names
# Ensure the 'job_posted_month' column is treated as integers for conversion
df_DA_percent['job_posted_month'] = df_DA_percent['job_posted_month'].apply(
    lambda x: pd.to_datetime(f'2023-{int(x)}-01').strftime('%b')
)

# Step 3: Set the cleaned month names as the new index
df_DA_percent = df_DA_percent.set_index('job_posted_month')

# Optional Step 4: Sort the months properly
month_order = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
               'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
df_DA_percent = df_DA_percent.loc[month_order]

df_DA_percent

```
``` Python
# Visualization of top skills trends

# Select top 5 most in-demand skills for cleaner visualization
top_skills = df_DA_percent.mean().sort_values(ascending=False).head(5).index

# Create the plot
plt.figure(figsize=(12, 8))

# Plot trends for top skills
for skill in top_skills:
    plt.plot(df_DA_percent.index, df_DA_percent[skill], marker='o', linewidth=2, label=skill)

plt.title('Trending Skills for Data Analysts in India (2023)', fontsize=16, fontweight='bold')
plt.xlabel('')
plt.ylabel('Percentage of Job Postings', fontsize=12)
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(True, alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

### Results

![alt text](image-2.png)

### Insights

- SQL remains the #1 skill throughout the year, consistently demanded in 50-72% of job postings.
- Shows remarkable resilience with peaks in February (~72%) and May (~70%).
- February and May emerge as peak hiring months across multiple skills.
- November shows a significant dip for most skills (SQL, Python, Tableau) - possibly due to year-end budget constraints or holiday seasons.
- Strong recovery in December suggests companies preparing for the new year.


## 3) How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Roles

``` Python
sns.boxplot(data=df_india_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distributions in India')
plt.xlabel('Yearly Salary (INR)')
plt.ylabel('')
ticks_x = plt.FuncFormatter(lambda y, pos: f'{int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
![alt text](image-4.png)

### Insights

- These roles generally command the highest median salaries, around 150,000 INR annually.
- Their salaries are strong, similar to engineers, with a good range of earnings.
- These roles typically have lower starting salaries and less variation, with a median around 75,000-100,000 INR.
-  While varied, Senior Data Engineers can earn considerably more, with some reaching up to 250,000 INR.
- Exceptional performance or niche skills can lead to significantly higher salaries, as seen with a Data Analyst earning over 600,000 INR.


### Plot 1: Highest Paid Skills for Data Analysts in India

### Analysis: 
- This plot highlights specific technologies and concepts associated with top salaries for data analysts. There's a clear distinction in salary ranges between two groups of skills, indicated by different bar colors.

### Insights:
- Specialized backend/DevOps-related skills like PostgreSQL, PySpark, GitLab, Linux, and MySQL command the absolute highest median salaries, reaching above (INR)160K. This suggests that data analysts who also possess strong engineering or infrastructure skills are highly valued.

- Skills like GDPR, MongoDB, Scala, Neo4J, and Databricks also command very high salaries, typically in the 150K-160K range. These are often associated with big data processing, NoSQL databases, and compliance.

### Results: 
- Data analysts aiming for the highest compensation should consider specializing in niche, high-demand technical skills, particularly those related to data engineering, cloud infrastructure, and advanced database management.

### Plot 2: Most In-Demand Skills for Data Analysts in India

Analysis: This plot shows a broader range of skills, indicating what is generally sought after in the data analyst job market and their corresponding median salaries. The salary range here is generally lower than the "Highest Paid Skills" plot.

### Insights:

- SQL, Python, and Power BI appear to be highly valued, with median salaries reaching around 100K - 110K. These are foundational and widely used skills for data manipulation, analysis, and visualization.

- Cloud platforms like Azure, AWS, and Oracle are also in demand, with salaries ranging from around $80K to $100K. This indicates a growing need for data analysts who can work with cloud-based data solutions.

- Excel, R, and Tableau are still relevant, but their median salaries are generally lower compared to other listed skills, often below $100K. This suggests they might be more entry-level or supplementary skills.

### Results: 
- For general employability and a solid career foundation as a data analyst, mastering core skills like SQL, Python, and a visualization tool (e.g., Power BI or Tableau) is crucial. Adding cloud knowledge (Azure, AWS) can significantly boost earning potential and career opportunities.


# 4) What are the most optimal skills to learn for Data Analysts?
- To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

## Visualize data
``` Python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```

## Insights
- Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})
``` Python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  
    palette='bright',  
    legend='full'  
)
plt.show()
```
- High Demand for Core Data Skills: SQL and Python are the most in-demand skills, appearing in nearly 50% and 40% of data analyst jobs respectively, with median salaries around INR 95,000 to INR 100,000. This highlights their foundational importance in the field.

- MongoDB Offers Highest Salary Potential: While not as highly demanded as SQL or Python (appearing in under 10% of jobs), MongoDB commands the highest median yearly salary at over INR 160,000, suggesting a niche but highly compensated skill.

- Variety in Tool Compensation and Demand: Skills like Power BI and Tableau are moderately in demand (around 20-25% of jobs) and offer good salaries (around INR 110,000), indicating their value in visualization and business intelligence. Conversely, general office tools like Word and PowerPoint are less impactful on salary despite some demand.

- Emerging Cloud and Database Skills: Cloud platforms (Azure, AWS) and various databases (Oracle, SQL Server, Hadoop) show varying levels of demand and salary, indicating a growing need for data analysts with infrastructure and diverse data storage expertise.

### Results
![alt text](image-5.png)

# Conclusion
  This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights i got enhance my understanding and provide actionable guidance for anyone looking to advance their carrer in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.