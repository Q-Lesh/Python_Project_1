# What's It About?

This project presents a structured analysis of the US data job market, with a specific focus on data analyst roles. The goal was to translate raw job-market data into clear insights that support informed career and hiring decisions. Using Python-based data extraction and analysis, the project identifies the most in-demand skills, maps key salary patterns, and highlights where market demand and compensation align most strongly.

Download full dataset: https://github.com/Q-Lesh/Python_Project_1/releases/tag/dataset-v1

# What Was Analyzed?

1. Which skills are most in demand across the three most common data roles?
2. How are in-demand skills evolving over time for Data Analysts?
3. What is the relationship between specific skills and compensation for Data Analyst positions?
4. Which skills offer the strongest combined advantage of high market demand and high salary potential?

# What Tools Were Used?

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The core language used to process the dataset and extract meaningful insights.
    - **Pandas Library:** Used for data cleaning, transformation, and exploratory analysis.
    - **Matplotlib Library:** Provided the foundation for visualizing key patterns and trends.
    - **Seaborn Library:** Enabled more advanced and polished statistical visualizations.
- **Jupyter Notebooks:** Used to develop and document the analysis in a clear, step-by-step format that integrates code, commentary, and visual outputs.
- **Visual Studio Code:** Served as the primary environment for writing and refining Python scripts.
- **Git & GitHub:** Used for version control, reproducibility, and sharing the full analysis workflow, ensuring transparency and easy collaboration.

# Data Preparation and Cleanup

This section describes the data preparation workflow used to ensure the dataset was clean, consistent, and ready for reliable analysis.

## Import & Clean Up Data

The process begins with importing the required libraries and loading the dataset, followed by foundational cleaning steps to ensure completeness, consistency, and overall data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data (full dataset: https://github.com/Q-Lesh/Python_Project_1/releases/tag/dataset-v1)
df = pd.read_csv("data/data_jobs.csv")

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter the US Jobs

To ensure the analysis reflects the U.S. job market, I apply location-based filters to retain only roles listed within the United States.

```python
df_US = df[df['job_country'] == 'United States']

```

# The Analysis

Each Jupyter notebook in this project is designed to address a specific question about the data job market. The analysis is organized so that every notebook focuses on one aspect of the problem, guiding the investigation from raw data to clear, interpretable insights.

## 1. Which Skills Are Most in Demand Across the Three Most Common Data Roles?

To identify the skills most in demand for the three most common data roles, I first determined which job titles appeared most frequently in the dataset. For each of these roles, I then extracted and ranked the top five skills. This analysis highlights how skill requirements differ across popular data positions and clarifies which capabilities matter most depending on the role being pursued.

A full walkthrough of the methodology and results is available in the corresponding notebook: [2_Skill_Demand](2_Skill_Demand.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Likelihood of Skills Requested in the US Job Postings](images/Likelihood_of_Skills_Requested_in_US_Job_Postings.png)

*A bar chart illustrating salary levels for the three most common data roles, along with the top five skills associated with each role. The visualization highlights how specific skills align with higher compensation within each job category.*

### Insights:

- SQL emerges as the most requested skill for both Data Analysts and Data Scientists, appearing in more than half of all job postings for these roles.
For Data Engineers, the leading skill is Python, included in 68% of postings.
- Data Engineer roles show a higher concentration of specialized technical requirements, with frequent demand for cloud and big-data technologies such as AWS, Azure, and Spark. In contrast, Data Analysts and Data Scientists more often require broadly applicable analytical tools, including Excel and Tableau.
- Python demonstrates strong cross-role relevance, with especially high demand among Data Scientists (72%) and Data Engineers (65%), underscoring its importance as a core programming skill across the data domain.

## 2. How Are In-Demand Skills Evolving Over Time for Data Analysts?

To analyze how skill demand evolved throughout 2023 for Data Analysts, I filtered the dataset to include only data analyst roles and then grouped the associated skills by the posting month. This produced a monthly ranking of the top five skills, revealing how their popularity shifted over the course of the year.

A full breakdown of the methodology and results is available in the corresponding notebook: [3_Skills_Trend](3_Skills_Trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in the US](images/Trending_Top_Skills_for_Data_Analysts_in_the_US.png)  
*A bar chart illustrating how the top skills for Data Analysts trended throughout 2023 in the United States, highlighting month-by-month shifts in demand.*

### Insights:
- SQL remains the most consistently in-demand skill throughout 2023, though its share shows a gradual decline over the year.
- Excel exhibits a pronounced rise in demand beginning in September, ultimately surpassing both Python and Tableau by year’s end.
- Python and Tableau maintain relatively stable demand, with moderate fluctuations, reinforcing their status as core competencies for data analysts.
- Power BI, while less prominent overall, demonstrates a modest upward trend, especially in the final months of the year.

## 3. What Is the Relationship Between Specific Skills and Compensation for Data Analyst Positions?

To identify the highest-paying roles and skills, I filtered the dataset to include only U.S.-based positions and calculated the median salary for each role. Before focusing on skills, I examined the salary distributions of common data roles—Data Scientist, Data Engineer, and Data Analyst—to establish a baseline understanding of how compensation varies across the field.

A detailed walkthrough of the methodology and results is available in the corresponding notebook: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in the US](images/Salary_Distributions_of_Data_Jobs_in_the_US.png)  
*A box plot illustrating the salary distributions for the six most common data job titles, highlighting differences in median pay, variability, and overall compensation ranges across roles.*

#### Insights

- Salary ranges vary substantially across job titles. Senior Data Scientist roles show the highest earning potential, reaching up to $600K, underscoring the industry’s premium on advanced analytical expertise and experience.

- Senior Data Scientist and Senior Data Engineer positions exhibit a large number of high-end outliers, indicating that exceptional expertise, niche specialization, or specific market conditions can lead to significantly higher compensation.
In contrast, Data Analyst roles display a more concentrated and consistent salary range, with fewer extreme outliers.

- Median salaries increase with seniority and technical specialization. Senior-level roles not only offer higher median pay but also greater variability in compensation, reflecting broader responsibility levels and a wider spread of employer expectations.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed the analysis to Data Analyst roles specifically, examining both the highest-paying skills and the skills most frequently requested in job postings. Two bar charts were used to present these findings, highlighting the contrast between market demand and compensation potential for each skill.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results
Below is a summary of the highest-paying and most in-demand skills for Data Analysts in the United States, highlighting how compensation and market demand align across key competencies.

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](images/Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_the_US.png)
*Two bar charts illustrating the highest-paying skills and the most in-demand skills for Data Analysts in the United States, presented side by side to highlight differences between market demand and compensation.*

#### Insights:

- Specialized technical skills such as `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salary levels, in some cases reaching up to $200K. This indicates that advanced or niche technical competencies can significantly enhance earning potential for Data Analysts.

- Foundational skills including `Excel`, `PowerPoint`, and `SQL` remain the most in-demand, even though they do not command the highest salaries. Their prevalence underscores their importance for day-to-day analytical work and overall employability.

- A clear separation emerges between skills that drive higher compensation and those that dominate employer demand. For Data Analysts aiming to strengthen career prospects, developing a balanced skill set that incorporates both high-value specialized skills and widely required foundational skills is essential.

## 4. Which Skills Offer the Strongest Combined Advantage of High Market Demand and High Salary Potential?

To determine the most optimal skills for Data Analysts to learn—that is, the skills that are both highly paid and highly in demand—I calculated each skill’s demand percentage and its median salary. Plotting these metrics together makes it easy to identify which skills offer the strongest combined advantage.

A full breakdown of the methodology and results is available in the corresponding notebook: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the US](images/Most_Optimal_Skills_for_Data_Analysts_in_the_US.png)    
*A scatter plot illustrating the most optimal skills for Data Analysts in the United States by mapping both median salary and demand percentage, highlighting the skills that offer strong value across both dimensions.*

#### Insights:

- `Oracle` stands out with the highest median salary at nearly $97K, despite appearing in a relatively small share of job postings. This indicates that specialized database expertise can command a premium in data analyst roles.

- Widely required foundational skills such as `Excel` and `SQL` appear frequently in job listings but are associated with lower median salaries compared to more specialized tools like `Python` and `Tableau`, which offer both higher earning potential and moderate demand.

- Skills such as `Python`, `Tableau`, and `SQL Server` occupy a strong position on both dimensions—they are reasonably common in job postings and align with higher salary levels. This combination suggests that these tools provide particularly strong career value for data analysts.

### Visualizing Different Technologies

Let’s also visualize the different technologies within the scatter plot by assigning color labels based on their category (for example, {Programming: Python}). This adds an additional layer of interpretability by distinguishing skills not only by demand and salary, but also by their functional domain.

#### Visualize Data

```python
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

#### Results

![Most Optimal Skills for Data Analysts in the US with Coloring by Technology](images/Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Coloring_by_Technology.png)  
*A scatter plot illustrating the most optimal skills for Data Analysts in the United States, combining median salary and demand percentage, with color labels indicating each skill’s technology category.*

#### Insights:

- `Programming` skills (colored blue) form a clear cluster at higher salary levels, suggesting that programming proficiency provides a strong salary advantage within data analyst roles.

- Database technologies (orange), such as Oracle and SQL Server, are linked to some of the highest median salaries in the dataset. This underscores the industry’s strong valuation of data management and data infrastructure expertise.

- Analyst tools (green), including Tableau and Power BI, appear frequently in job postings and offer competitive compensation. Their combination of versatility, prevalence, and solid salary levels highlights the importance of visualization and business intelligence tools in modern data analytics work.

# What I Learned from This Project

Throughout this project, I expanded my understanding of the data analyst job market and strengthened my technical capabilities in Python, particularly in data manipulation and visualization. Key takeaways include:

- **Enhanced Python Proficiency**: Working extensively with libraries such as Pandas for data manipulation and Seaborn/Matplotlib for visualization enabled me to execute more complex analytical workflows effectively.
- **Critical Role of Data Cleaning**: The project reinforced how essential rigorous data cleaning and preparation are for producing accurate, reliable insights.
- **Strategic Understanding of Market Skills**: By analyzing the relationship between skill demand, salary, and role prevalence, I gained a clearer perspective on how to prioritize skill development to align with market needs and maximize career opportunities.


# Key Insights

This project provided several broader insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear relationship between the demand for specific skills and the salaries associated with them. Specialized and technically advanced skills such as Python and Oracle tend to command higher compensation.
- **Market Trends**: Shifts in skill demand throughout the year underscore how dynamic the data job market is. Staying aligned with these trends is essential for long-term career development in data analytics.
- **Economic Value of Skills**: Identifying which skills are both highly sought after and well compensated can help data analysts prioritize their learning to maximize economic returns and career flexibility.


# Challenges and How I Addressed Them

This project came with several challenges, each of which offered valuable learning opportunities:

- **Data Inconsistencies**: Managing missing or inconsistent entries required careful handling and systematic data-cleaning practices to maintain analytical accuracy.
- **Complex Data Visualization**: Creating visualizations that accurately represented complex relationships while remaining clear and interpretable was a key challenge, but essential for communicating insights effectively.
- **Balancing Breadth and Depth**: Determining when to dive deeper into specific analyses and when to preserve a broader perspective required ongoing judgment to ensure clarity, completeness, and relevance.


# Conclusion

This exploration of the data analyst job market has been highly informative, shedding light on the skills, trends, and market dynamics shaping this evolving field. The insights gained strengthen my understanding of the landscape and offer practical guidance for anyone aiming to advance within data analytics. As the market continues to shift, ongoing analysis will remain essential to staying competitive. This project provides a solid foundation for future investigations and reinforces the importance of continuous learning and adaptability within the data profession.

