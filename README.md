# Data Portfolio Projects

Greetings, this is project portfolio page by Hilary Wong Feng Teng! ⭐

If you want to connect with me, here is my social media:
- Linkedin: [Hilary Wong Feng Teng](https://www.linkedin.com/in/hilary-wong-feng-teng-4a7596238/)


## SQL Projects
### SQL Project 1: Analyzing Students' Mental Health
- This project is about analysing international students risk of mental health, and that social connectedness and acculturative stress that are predictive of depression.
✅: [SQL Student Mental Data Analysis Project](SQL_Mental_proj.md)
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
![image](https://github.com/user-attachments/assets/a7795033-bb86-4cd4-a999-5d475e1571ef)

