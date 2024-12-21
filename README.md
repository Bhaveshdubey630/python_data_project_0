# The Overview 
Welcome to my analysis of the data job market, with a particular focus on data analyst roles. This project is a culmination of my learning journey through Luke Barousse's Python course. It reflects my first steps into Python programming and data analysis, demonstrating how I applied foundational skills to gain insights into the job market. My aim was to navigate and understand the data analytics job market better and explore the critical skills that can open optimal job opportunities for aspiring data analysts.

Through a detailed analysis of job roles, salaries, and essential skills, I aimed to answer key questions about the most demanded and highest-paying skills in the field. By combining Python programming and data visualization techniques, this project provided a hands-on approach to solving real-world problems.

# Questions Explored

Here are the core questions I sought to answer through this project:

What are the skills most in demand for the top three most popular data roles?

How are in-demand skills trending for Data Analysts?

How well do jobs and skills pay for Data Analysts?

What are the optimal skills for data analysts to learn (i.e., those that are both high-demand and high-paying)?

# Tools Used

This project leveraged several essential tools and libraries that formed the backbone of my analysis:

* Python: The primary language for data analysis and scripting.

* Pandas: Used for data manipulation and analysis.

* Matplotlib: Enabled me to create detailed data visualizations.

* Seaborn: Helped in producing advanced and aesthetically pleasing visualizations.

* Jupyter Notebooks: Provided an interactive platform to run Python scripts and document my thought process.

* Visual Studio Code: My go-to editor for executing Python scripts and organizing code files.

* Git & GitHub: Essential for version control, sharing my work, and tracking my project’s progress.

# Data Preparation and Cleanup

Data preparation was a critical part of the project to ensure accuracy and usability. Below are the steps I took:

### Import and Clean Data

I started by importing essential libraries and loading the dataset, followed by data cleaning tasks to remove inconsistencies and prepare the data for analysis.

``` python 
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
### Focus On India and U.S jobs- 

``` python 
# Filtering U.S. Jobs
df_US = df[df['job_country'] == 'United States'].copy()
df_IND = df[df['job_country'] == 'India']
```




# The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

to find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here - [2_skill_demand](3_project\2_skill_demand.ipynb)

## Visualize data

``` python 

fig,ax = plt.subplots(len(job_titles), 1)


for i , job_title in enumerate(job_titles) :
    df_plot = df_skills_perc[df_skills_perc["job_title_short"]==job_title].head(5)
    sns.barplot(data=df_plot , x= "skill_percent" , y ="job_skills" , ax=ax[i] , hue = "skill_count" , palette="dark:b_r", legend=False )
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 75)
    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    for n,v in enumerate(df_plot["skill_percent"]):
        ax[i].text(v+1,n,f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in INDIA Job Postings', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
  
``` 

### Results

![visualization of top skills](3_project\images\output.png)

### Insights

1-SQL and Python: Common to all three roles, with the highest demand for SQL in Data Engineers and Python in Data Scientists.

2-Cloud Technologies: AWS and Azure are crucial for Data Engineers, with limited demand for Data Scientists and Data Analysts.

3- Visualization Tools: Tableau and Power BI are most relevant for Data Analysts, with Tableau also being useful for Data Scientists.

4-Big Data Frameworks: Spark is a key differentiator for Data Engineers.

5-Role-Specific Focus:
Data Analysts prioritize visualization and reporting.
Data Engineers focus on cloud and big data technologies.
Data Scientists excel in programming, statistics, and machine learning.

## 2. How are in-demand skills trending for Data Scientists in India?

To find how skills are trending in 2023 for Data Scientists, I filtered data Scientist positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data Scientists by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_skill_trend](3_project\3_skill_trend.ipynb)

### Visualize data 
``` python 
from matplotlib.ticker import PercentFormatter

df_plot = df_DS_IND_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Scientists in the IND')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show() 
```
### Results 

![](3_project\images\data_scientist_skill_trend_in_india_monthly.png)

### Insights 

1 - Python consistently remains the most demanded skill for data scientists, peaking at 70% in June and maintaining a stable range of 65%-70% throughout the year.

2- SQL ranks second in demand, averaging 50% throughout 2023.
Together, Python and SQL account for a majority of the skill requirements, reflecting their foundational importance in the data science domain.

3- Tableau (13% demand) is not a core requirement but is useful for enhancing data storytelling and visual communication skills.

4- AWS demand, though lower than programming and querying languages, shows that cloud computing is increasingly becoming a valued skill.


## 3 - How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_salary_analysis](3_project/4_salary_analysis.ipynb)

### Visualize data

``` python 
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results 

![Data analyst salary comparison with other jobs](3_project\images\data_analyst_salary_comparison.png)
*boxplot doing salary analysis within top 6 jobs*

insights -

* There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

* Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

* The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

### visualize data

``` python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analyst
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

### Results

![in demand skill vs highest paid skill for data analyst](3_project\images\highest_paid_vs_in_demand_skill.png)

### insights 

* The highest paying skills are heavily focused on advanced programming (e.g., Solidity for blockchain, dplyr for data manipulation in R), version control (e.g., Bitbucket, Gitlab), and specialized machine learning and cloud platforms (e.g., Hugging Face, MXNet, Couchbase).

* The most in-demand skills include Python, Tableau, R, SQL, Excel, and Power BI, all of which are fundamental to data analysis workflows

* Data analysts seeking to maximize both employability and earning potential should aim to balance their skill set by mastering both in-demand tools like Python, SQL, and Tableau while also gaining proficiency in specialized tools such as Hugging Face or Solidity.


## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [5_optimal_skills](3_project/5_optimal_skill.ipynb)

### Visualize data

``` python 
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
### Results 
![](3_project\images\optimal_skills.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

### Insights - 

* While less frequent in job postings, Oracle commands a premium in the data analyst market with a median salary approaching $98K. This highlights the value of specialized database skills in the field.

* Skills like Excel and SQL are highly sought after, reflected in their frequent appearance in job postings. However, their median salaries are relatively moderate compared to specialized tools like Python and Tableau. This suggests a potential market saturation for these skills.   

* Basic skills like PowerPoint and Word are still in demand but have lower median salaries, suggesting they might be considered entry-level or foundational skills.

## Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

``` python 
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
``` 

### results 

![](3_project\images\optimal_skills_by_technology.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology*

### insights

* The cluster of "programming" skills (blue) at higher salary levels reinforces the notion that proficiency in languages like Python is highly valued and can lead to significant earning potential in data analytics.

* The "cloud" category (red) might represent cloud computing skills (e.g., AWS, Azure, GCP). Its presence at higher salary levels suggests that cloud-based data analysis and technologies are becoming increasingly important and well-rewarded.

* "Databases" (red) skills, particularly Oracle, command a premium. This suggests that expertise in database management and manipulation is highly sought after and well-compensated, especially for specialized roles.

* "Analyst tools" (green) like Tableau and Power BI are widely used and offer competitive salaries. They seem to be foundational skills for many data analyst roles, providing a strong base for career advancement.


# What I learned

Through this project, I not only gained insights into the data analyst job market but also significantly enhanced my technical and analytical skills. Here are some key takeaways:

1- Advanced Python Skills: I became proficient in using Python libraries such as Pandas, Seaborn, and Matplotlib for data manipulation and visualization.

2- Importance of Data Cleaning: I learned that data cleaning is foundational to any analysis, as it ensures the accuracy and reliability of the results.

3- Market Dynamics: The project emphasized the importance of aligning skills with market trends and demonstrated how strategic skill development can enhance career prospects.

# Insights Gained

This project yielded several actionable insights into the data analyst job market:

1- Demand vs. Salary: Skills such as Python and SQL are both in high demand and command competitive salaries. Specialized skills can significantly enhance earning potential.

2- Trends in Skill Demand: Skill trends are dynamic and evolve over time, underlining the need for continuous learning to stay relevant.

3- Strategic Learning: Understanding which skills are both in-demand and lucrative is essential for making informed career decisions in data analytics.

# Challenges Faced

The project presented some challenges, which became valuable learning opportunities:

1- Data Inconsistencies: Handling missing or inconsistent data entries required thorough cleaning techniques to ensure the integrity of the analysis.

2- Complex Visualizations: Creating effective visualizations of large datasets was challenging but essential for conveying insights clearly.

3- Balancing Scope: Striking a balance between breadth and depth of analysis was crucial to ensure comprehensive coverage without overcomplicating the project.

# Conclusion

This project marks the beginning of my journey into Python programming and data analysis. By analyzing the data analyst job market, I gained a deeper understanding of the skills and trends shaping this dynamic field. The insights from this project provide actionable guidance for aspiring data analysts looking to enhance their careers.

As the job market continues to evolve, ongoing analysis will be essential to stay ahead in the field of data analytics. This project is a strong foundation for future explorations, and I look forward to building on these skills as I continue to learn and grow in the tech industry.