# Hilary_portfolio1
## SLQ Project about Analyzing Students' Mental Health

> [!NOTE]
> This project description is sourced from DataCamp’s project on mental health of students going to a university in a different country. DataCamp is an online platform offering data science and data analysis courses and projects. The original project can be found there:

![image](https://github.com/user-attachments/assets/987c1097-b0c8-43ff-8788-48c0e3aec090)

_Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards._

_The study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression._

_Explore the `students` data using PostgreSQL to find out if you would come to a similar conclusion for international students and see if the length of stay is a contributing factor._

## Data Table Schema
The data table schema is presented as followed:

![image](https://github.com/user-attachments/assets/46a7f10f-f50d-4eda-ad75-3049bdc6607b)

With that, the dataset's column descriptions provided valuable information for me in this investigation.

## The Task
Return a table with nine observations and five columns summarizing data for the international students: `stay`, `count_int`, `average_phq`, `average_scs`, and `average_as`, **in that order**. The average columns should contain the average of the `todep`, `tosc`, and `toas` columns for each length of stay, **rounded to two decimal places**. The `count_int` column should be the number of _international students_ for each length of stay. Sort the results by the length of stay in **descending order**.

## Data Exploration
In every data project, it is typical to explore the data to gain better understanding of the data. Henceforth, I had executed a SQL statement to view the raw dataset and the following table was created:
```
SELECT * 
FROM students;
```
![image](https://github.com/user-attachments/assets/e4d85db5-33a1-43b4-a7f3-96974672ed60)
> *Only the first 10 rows and 6 coloumns are shown.

The dataset had return total of 286 rows of result with 50 columns. The dataset's size makes it challenging to ensure accuracy for the number of columns returned. With the aforementioned table schema above, the most important variables for the study are mentioned and will be focused moving forward. While all the variables are straight to the point, there a couple of variable required for further clarification and understanding, such as: `todep`, `tosc`, and `toas`.

Variable `todep` is denoted as the total score of depression (PHQ-9 test). 





