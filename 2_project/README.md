# The Analysis

## What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills i should pay attention to depending on the role i'm targeting

View my notebook with detailed steps here[2_skill_demand.ipynb](2_project/2_skill_demand.ipynb)

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

Visualization of top skills for data roles(2_project/images/top_skills.png)

### Insights
- Python is a versatile skill, highly demanded skill across all three roles, but most prominently for Data Scientist (62%)
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for noth roles.
- Data Engineers require more sprecialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis toils (Excel, Tableau)