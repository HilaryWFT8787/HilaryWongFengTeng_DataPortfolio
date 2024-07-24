# SQL portfolio 1
## SQL Project about Analyzing Students' Mental Health

> [!NOTE]
> This project description is sourced from DataCamp’s project on mental health of students going to a university in a different country. DataCamp is an online platform offering data science and data analysis courses and projects. The original project can be found there:

![image](https://github.com/user-attachments/assets/987c1097-b0c8-43ff-8788-48c0e3aec090)

_Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards._

_The study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression._

_Explore the `students` data ([students.csv](students.csv)) using PostgreSQL to find out if you would come to a similar conclusion for international students and see if the length of stay is a contributing factor._

## Data Table Schema
The data table schema is presented as followed:

![image](https://github.com/user-attachments/assets/46a7f10f-f50d-4eda-ad75-3049bdc6607b)

With that, the dataset's column descriptions provided valuable information for me in this investigation.

## The Task
Return a table with nine observations and five columns summarizing data for the international students: `stay`, `count_int`, `average_phq`, `average_scs`, and `average_as`, **in that order**. The average columns should contain the average of the `todep`, `tosc`, and `toas` columns for each length of stay, **rounded to two decimal places**. The `count_int` column should be the number of _international students_ for each length of stay. Sort the results by the length of stay in **descending order**.

## Data Exploration
In every data project, it is typical to explore the data to gain better understanding of the data. Henceforth, I had executed a SQL statement to view the raw dataset and the following table was created from the data named under 'students.csv':
```
SELECT * 
FROM students;
```
![image](https://github.com/user-attachments/assets/ef3362ed-cbdb-412a-8ecc-f96baf9a7d88)
> *Only the first 10 rows and 10 coloumns are shown.

The dataset had return total of 286 rows (or records) of result with 50 columns (or variables). The dataset's size makes it challenging to ensure accuracy for the number of columns returned. With the aforementioned table schema above, the most important variables for the study are mentioned and will be focused moving forward. While all the variables are straight to the point, there a couple of variable required for further clarification and understanding, such as: `todep`, `tosc`, and `toas`.

Variable `todep` is denoted as the total score of depression (PHQ-9 test). PHQ-9, which also stands for Patient Health Questionnaire-9, is a multipurpose instrument for screening, diagnosing, monitoring and measuring the depression severity. This PHQ-9 test consist of 9 questions to determine the depression score ranking from "None" to "Severe". The interpretation of depression severity can be view as followed:

![image](https://github.com/user-attachments/assets/585419ba-1b89-449f-b5cd-317e1f91a3d5)
> Interpretation source is retrieved from https://www.hiv.uw.edu/page/mental-health-screening/phq-9

As for the variable `tosc`, it denoted as the total score of social connectedness (SCS test). SCS, also indicated as social connectedness scale, is a A 20-item Likert scale was used to measure participants' perceived social connectedness, ranging from strongly disagree (1) to strongly agree (6). The higher the sum of the scores for all 20 items on the SCS indicate, the greater sense of social connectedness. (Lee et al., 2001)

The variable `toas` is defined as the total score of acculturative stress (ASISS test). ASISS, which also denoted as Acculturative Stress Scale for International Student, a practical approach to stress measurement that involve stress score based on respective acculturative stress statement such as perceived discrimination, homesickness, perceived hate, fear, stress due to change, guilt and non-specific miscellaneous. The ASSIS score range is range between 36 to 180 where the higher the scores accumulate indicates the higher the acculturative stress the person endure. (Sandhu & Asrabadi, 1994)

After ensuring all variable are fully unerstand, the first step to start the data analysation is to ensure there any empty row of records present in the dataset that might potentially affect the final result. With the the following SQL statement have been implimented to validate the present of null record.
```
SELECT *
FROM students
WHERE inter_dom NOT LIKE 'I%' AND inter_dom NOT LIKE 'D%' OR inter_dom IS NULL;
```
![image](https://github.com/user-attachments/assets/da1a8a6f-6483-4e84-9fc5-41ace3c53776)

With this, it is identifiable that there are 18 null records present in this dataset.

Moreover, we can perform more SQL statement to discover more meaning behind the dataset.
```
SELECT region, count(inter_dom)
FROM students
WHERE inter_dom LIKE 'I%'
GROUP BY region;
```
![image](https://github.com/user-attachments/assets/66ce6b6f-fd4f-4132-a36b-cfb9f58c39a3)

From here, we can identify that out of all the internation students went to this university, there are total of 122 students from Southeast Asia, 18 students from South Asia, 48 students form East Asia, 2 students from Japan and 11 students from other regions.

## Task Execution
```
SELECT 
  stay,
  COUNT(stay) AS count_int
FROM students
WHERE inter_dom LIKE 'Inter'
GROUP BY stay
ORDER BY stay;
```
![image](https://github.com/user-attachments/assets/e330fd1b-62a6-48d1-a091-27ee99f462e4)

From this step, the it previews the length of year the international student stay in years from ranging 1 year to 10 years. I had used GROUP BY clause and ORDER BY clause to organise the data better. This result also indicating that there are 95 students stayed for 1 year in this university while only having 1 student manage to stay up to 10 years away from hometown. As we can see that the number of student is lesser as they gradually stay aboard more years. 

```
SELECT 
  stay, 
  COUNT(stay) AS count_int,
  AVG(todep) AS average_phq, 
  AVG(tosc) AS average_scs, 
  AVG(toas) AS average_as
FROM students
WHERE inter_dom LIKE 'Inter'
GROUP BY stay
ORDER BY stay;
```
![image](https://github.com/user-attachments/assets/a2b0ff5a-a029-4b98-8451-ba0fb227a4d8)

Here I had outputed the average of the depression score, average of social conectedness score and the average of acculturative stress score to fulfill the requirements and the further understand the result of this data. 


```
SELECT 
  stay, 
  COUNT(stay) AS count_int, 
  ROUND(AVG(todep), 2) AS average_phq, 
  ROUND(AVG(tosc), 2) AS average_scs, 
  ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom LIKE 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```
![image](https://github.com/user-attachments/assets/3ec7d493-15de-4fa0-8552-752be893f94b)

This final SQL statemet outputed to satisfied all the requirements for the task and further organise the data result more clearly. Furthermore, the result is displayed that the international student with longer stay expose with higher in depression and lower in social conectedness. Hindsight, it is noticeable that the level of acculturative stress is commonly more higher around the students stay period is lower. This may indicate these international students still getting use to new environment and hard to convey unexpected issue such as perceived discrimination, homesickness, culture shock and fear. 

From this project it is determined that the international students is in more acculturative stress in recently year of stay but gradually grows to have signs of depression with students who have longer period of stay. The goverment should provide a mental health care vanues for international student to cope being away their home. Moreover, the university and also get in hand to promote social connection event for the international students to collaborate with the domestic students. Furthermore, the university can also brainstorm on having international week event for the international students to express their love for their hometown and culture as a foreign students to other people while also share their experience being outside of Japan.

## Reference
DataCamp. (n.d.). DataCamp. https://www.datacamp.com/ 

Lee, R. M., Draper, M., & Lee, S. (2001). Social connectedness, dysfunctional interpersonal behaviors, and psychological distress: Testing a mediator model. Journal of Counseling Psychology, 48(3), 310–318. https://doi.org/10.1037/0022-0167.48.3.310 

Patient Health Questionnaire-9 (PHQ-9) - Mental health screening - National HIV curriculum. (n.d.). https://www.hiv.uw.edu/page/mental-health-screening/phq-9 

Sandhu, D. S., & Asrabadi, B. R. (1994). Acculturative stress scale for international students [Dataset]. In PsycTESTS Dataset. https://doi.org/10.1037/t62213-000






