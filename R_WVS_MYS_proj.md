# R Programming Project: Analysation on World Values Survey Wave 7 (2017-2020)

> [!NOTE]
> This project is fully ustilised R programming langauge. The dataset can be found [Here](MYS.csv) and it explaination can be found [Here](R_Proj_1_MYS_detailed.pdf)

## Executive Summary
This report is aiming for analyzing the chosen dataset from the World Values Survey Wave 7 in Malaysia (2017-2020), via various multivariate techniques such as Principal Component Analysis, K-means Clustering and Exploratory Factor Analysis. Variables that are chosen to be employed in this analysis are as follows: Age Group, Self-perception of social class (Class), Feeling of happiness (Q46), Self-rated health (Q47), Freedom of choice in life (Q48), Satisfaction in life (Q49) and Satisfaction in financial household (Q50).


## Introduction
In this modern era defined by the intricate interplay of cultural shifts, economic dynamics and evolving societal values, it is interesting to know the state of an individual well-being, therefore the exploration of individual well-being stands at the forefront of this social research. For this report, the data used are all from the World Values Survey Wave 7 in Malaysia (2017-2020) dataset. It is an international research program devoted to the scientific and academic study of the social, political, economic, religious and cultural values of people around the world. It serves as a rich repository of global insights, capturing the nuances of human experiences from different cultures and societies. Within this expansive dataset, we have specifically chosen a few variables that will help us to dive deeper into exploring the individual well-being in Malaysia, unraveling the complex tapestry of well-being. These key variables are Q46 (feeling of happiness), Q47 (self-rated health), Q48 (freedom of choice and control in life), Q49 (satisfaction with life), Q50 (satisfaction with the financial situation of households), alongside demographic indicators Age and Class. With that, the respective variables are presented as follows for clearer explanation:
![image](https://github.com/user-attachments/assets/ad591a3d-6040-43f9-beb1-90a4fcbf6231)

At the heart of this multivariate analysis contains the selected variables, which each uniquely representing the mosaic of the individual well-being in Malaysia. Q46 and Q47 offer a glimpse into the subjective experiences that shape one’s sense of happiness, tapping into the individual emotional and physical health. On the other hand, Q48 and Q49 dive into the freedom of choice and overall life satisfaction, respectively, giving us a clearer view of personal agency and contentment. Lastly, Q50 centered on household financial satisfaction, reflects the satisfaction level of each individual regarding their economic well-being. On top of these five main variables, the variables Age and Class are also chosen to serve as a pivotal demographic marker, providing insights of how these dimensions of well-being vary across different age groups and classes in society. For this report the table below shows the description of these variables according to our categorization:

![image](https://github.com/user-attachments/assets/429ad96f-24d0-44a7-9a83-8865e4fed8b9)

## Aim of this analysis report
The primary aim for this multivariate analysis is to unravel the intricate connections between the chosen variables and the demographic factors (age and class), discerning the patterns, correlations and potential disparities of the variables that illuminate the determinants of individual well-being across the diverse populations in Malaysia. Specifically, we seek to elucidate how individual well-being, life satisfaction and economic perceptions intertwine with the two demographic factors, allowing us to have a clearer understanding of the determinants of individual well-being in this modern society, and offer insights that may inform targeted interventions or policies in Malaysia that aimed to enhance the quality of life for this diverse population.

## Data Exploration & Analysis

As aforementioned, the aim of this report is to identify and analyze Malaysia World Value Survey Wave 7 dataset.  Data exploration and analysis is essential and functional for us to analyze this huge dataset, which will allow us to have a clearer understanding and uncover the characteristics, initial patterns and different points of interest in this research. It is necessary to perform data visualization on our chosen variables to dig deeper into each variable and their relationships with the demographic variables which are age and class to have a broader understanding on these specific variables of interest.

### Data Selection and Data Cleaning
Before any execution of data visualization commences, data selecting and cleaning is performed to ensure the selected data can be visualized smoothly without missing data. 500 observations are chosen at random with seed set on value of 8 from the general MYS dataset.
Afterwards, the selected dataset is utilized the function of which(is.na()) in R to identify any missing data present in the chosen dataset. With that, the result has shown that there is no missing value from the selected dataset. The colMeans() function also implemented for similar reason, and further support that there is no missing value from the dataset too. Henceforth, we can proceed with data visualization and analysis. 

```
### PACKAGES ###
library(ggplot2)
library(tidyr)
library(dplyr)
library(magrittr)
library(corrplot)
library(gridExtra)
library(sqldf)
library(psych)
library(tidyverse)
library(psy)
library(BinMat)
library(MASS)
library(mvnormtest)
library(ICSNP)
library(biotools)
library(HSAUR2)
library(factoextra)
library(FactoMineR)
library(ggpubr)


### DATA PRELIMINARIES ###
MYSdata = read.csv(file= "MYS.csv", header = TRUE) 
set.seed (8)
index = sample (1:nrow(MYSdata), 500)
mydata = MYSdata[index,]
newdata = MYSdata[index, c("Q46", "Q47", "Q48", "Q49", "Q50")]
```

```
head(mydata)

#check missing data
which(is.na(mydata)) 
colMeans(mydata, na.rm = TRUE)

summary(newdata)

mydata$Class <- as.factor(mydata$Class)

str(mydata$Class)
```
![image](https://github.com/user-attachments/assets/005b5edb-d7ee-4395-a015-009cedd16ea9)


From table 3, it is notable that all the variables are roughly evenly spread that identify from the respective min, max, mean and median. Firstly, the Q46 variable has a median of 2 and a mean of 1.966, meaning that most of the individuals are at a scale value of 2 for the feeling of happiness, which is the feeling of rather happy. The 3rd quantile also shows that 75% of the individuals are at a scale value of 2 or below for Q46. Secondly, we can see that the Q47 variable has a median of 2 and a mean of 2.216, meaning that most of the individuals are at a scale value of 2 for the self-rated health, which is good self-rated health. The 3rd quantile also shows that 75% of the individuals are at a scale value of 3 or below for the Q47 variable. Thirdly, we can see that the Q48 variable has a median of 7 and a mean od 7.118, meaning that most of the individuals are at a scale value of 7 for the freedom of choice and control in life, which is rather great deal of choice. The 3rd quantile also shows that there are 75% of individuals are at a scale value of 8 or below. Similarly to Q48, Q49 also has a median value of 7 and a mean of 6.94, meaning that most of the individuals are at a scale value of 7 for the satisfaction with life, which is rather satisfied. The 3rd quantile also shows that 75% of individuals are at a scale value of 8 or below for Q49 variable. Lastly, the Q50 variable has a median of 6 and a mean of 6.234, meaning that most of the individuals are at a scale value of 6 for the satisfaction with the household financial situation, which is rather satisfied. The 3rd quantile also shows that there are 75% of the individuals are at a scale value of 8 or below and 25% being at 5 or below for the Q50 variable.

### Data Visualisation
```
###FURTHER DATA EXPLORATION###

#mutate the data for more organise data
mutate.data <- mydata
mutate.data <- mutate.data %<>%
  mutate(
    Age_Group = ifelse(Age >= 60, "Senior", 
                       ifelse(Age >= 50, "Old Age Adult", 
                              ifelse(Age >= 40, "Middle Age Adult", 
                                     ifelse(Age >= 30, "Young Adult", "Youth")))),
    Age_Group = as.factor(Age_Group)) 

mutate.mydata <- mutate.data[index, c("Q46", "Q47", "Q48", "Q49", "Q50", "Age_Group", "Class")]


# Assuming 'mutate.data' is your data frame
mutate.data$Age_Group <- factor(mutate.data$Age_Group, levels = c("Youth","Young Adult" ,"Middle Age Adult", "Old Age Adult", "Senior"))

#Class vs Q50
mydata_C_Q50 <- ggplot(mydata, aes(x = Class, y = Q50, fill = Class)) +
geom_violin(aes(fill = Class)) +
geom_boxplot(width = 0.1) +
ggtitle("Class vs Satisfaction in Financial Household") + ylab("Satisfaction in Financial Household") +
  theme(plot.title = element_text(size = 14, face = "bold"), legend.key.size
= unit(0.6, "cm")) +
scale_fill_manual(values=c('#FF8300','#37C854','#E8DC17', "#0000FF", "#FF0000"))

mydata_C_Q50

#Class vs Q48
mydata_C_Q48 <- ggplot(mydata, aes(x = Class, y = Q48, fill = Class)) +
geom_violin(aes(fill = Class)) +
geom_boxplot(width = 0.1) +
ggtitle("Class vs Freedom of Choice") + ylab("Freedom of Choice") +
  theme(plot.title = element_text(size = 14, face = "bold"), legend.key.size
= unit(0.6, "cm")) +
scale_fill_manual(values=c('#FF8300','#37C854','#E8DC17', "#0000FF", "#FF0000"))

mydata_C_Q48
```

![image](https://github.com/user-attachments/assets/8a13094f-7177-4fe8-810e-4f711a87f50c)
> Figure 2: Class vs Satisfaction in Household Financial Situation (Q50)

![image](https://github.com/user-attachments/assets/b7563d27-4487-4bd2-a923-003fcf3bc80b)
> Firgure 3: Class vs Freedom of Choice and Control in Life (Q48)

Figure 2 shows the class versus satisfaction with the financial situation of the household by using violin plots. Overall, the plot labels the scales from 1 to 10 for the y-axis which is the satisfaction in financial situation of household, with the lowest scale indicating a completely dissatisfied and the highest scale indicating a completely satisfied. On the other hand, there are five classes being used in order to explore the perspective of individuals from different classes on their own household financial situation. The class is categorized into 5 categories, with 1= Upper Class, 2 = Upper Middle Class, 3 = Lower Middle Class, 4 = Working Class and 5 = Lower Class. We will also use the same class categories later on our other violin plots and for further data analysis.

Firstly, we can observe that the class 1 which is the upper class and class 2 which is the upper middle class are both having the highest median value of 7 for their satisfaction in household financial situation. This clearly reflects that individuals who are from the upper class usually are satisfied with their financial situation, which is reasonable as they usually are richer compared to those individuals from lower classes. Not just that, the 75th percentile of class 1 is at the value around 9, meaning that there are 75% of individuals from class 1 has a scale of 9 or less for the satisfaction in financial situation, with the minimum value being at scale 5. This again shows that the upper classes usually are satisfied with their financial situation with scale five being the lowest value for that group. On the other hand, the 75th percentile for class 2 is at the value around 8, meaning that 75% of individuals from the class 2 said that they have a scale of 8 or lower for their satisfaction level, with most of them having a scale of 7 as the violin plot is wider at that scale. This means that class 2 usually also has higher satisfaction than the other classes for their financial situation, except slightly lower than class 1. Additionally, class 2 also has a very slight negative skewed violin plot, meaning most of them has high scale for satisfaction level.

Secondly, we can see those individuals from class 3, 4 and 5 show a very similar satisfaction level with their household financial situation. All three of them have the same lowest median score of around 6, which is close to the average value of 6.234 for the Q50 variable as shown on the summary statistics table above. We also noticed that the 75th percentile of class 4, which is the working class, lie at the value around 7 which is the lowest 75th percentile value among the 5 classes. This means that 75% of the individuals from class 4 have a scale of 7 or lower, this may be the reason why they are in the working class, as they are still working to earn more money and trying to improve their financial situation. Another noticeable thing is that the 25th percentile value for class 2 to class 5 are all the same, lying on the value 5. The main reason for this could be different classes usually have different needs in their daily life, causing there still some portion from each class that is not really satisfied with their financial situation. Although that is the case, there are still some individuals from class 2 to 5 that are completely satisfied with their financial situation and a very small amount being completely dissatisfied with their financial situation as being shown by the violin plot’s whiskers.
In general, Figure 2 violin plot indicates that individuals from the upper classes tend to be more satisfied with their household financial situation. Oppositely, individuals from lower classes tend to be less satisfied with their financial situation.

Figure 3 shows the class vs freedom of choice and control in life (Q48). Similarly to Figure 2, the plot labels the scales from 1 to 10, where the highest scales defines an individual that has a great deal of choice of freedom and control in their life and the lowest scales defines an individual having no choice at all in their life.

Among the five classes, individuals from class 1 and 2 are having the same highest median scale value of 8, and also there are higher concentration at the scale value of 8 for class 1 and 2. This means that the upper class and the upper-middle class usually have more control and freedom in their life as those individuals mostly are wealthier than the other remaining three classes. Not just that, the 75th percentile of class 1 lie at the scale value of around 9, meaning that there are 75% of the individuals from class 1 having a freedom level of 9 or below with level 5 being the minimum level. This is very reasonable as a wealthier individual usually don’t have any restraint on their life as they are able to afford most of things they want and do what they like.

Then, we can see the median values for class 3, 4 and 5 are the lowest which are 7, which is close to the average value of 7.118 for the Q48 variable as shown on the summary statistics table above. Then, there is a noticeable huge difference for the 75th percentile value of class 4 when compared to class 3 and 5. The 75th percentile value of class 4 is almost as high as class 1, which is scale 9 and is even higher than class 2. The reason for this could be working-class individuals are always earning money, which allows them to have more choices and control in their life, although the living standard might not be as high as the upper-class. Then, the 25th percentile value for class 2 to class 4 are all the same which lie on the value 6. The reason for this could be some younger generation from the upper-middle class are facing freedom restrictions from their parents. On the other hand, there might also be individuals from the working class that work many hours every day to feed their family, causing them to have lesser freedom in life. In addition, we can see that class 2 to 5 are having slight negatively skewed plots, meaning that majority of individuals fall at the upper scale level. Although that is the case, there are still some individuals from class 2 to 5 that have a great freedom and control in their life and a very small number of outliers that have no choice at all as being shown by the violin plot’s whiskers.


### Basic Data Analysis
```
corrplot(cor(newdata), method="number")
```

![image](https://github.com/user-attachments/assets/6e1f7e7e-8232-40c7-a5db-cb011299fe10)
> Correlation heat map for the five main variables (Q46-Q50)


It is decided to produce a correlation heat map for the five main variables to see if they are related to each other and to what degree. Firstly, we see that Q50 and Q49 have the highest positive correlation value amongst all the other variables, which is 0.66, indicating that an individual with a high value of Q50 will usually possess a high value for Q49 too, or oppositely. In other words, an individual who has higher satisfaction with the financial situation of their household will usually have higher satisfaction with life. On the other hand, we also can see that Q49 and Q46 have the highest negative correlation value amongst all the other variables, which is -0.46, which means that a higher value of Q49 will usually leads to a lower value of Q46, or inversely. In other words, an individual who has higher satisfaction with life will usually feel happier in their life, or oppositely.

## Further Data analysis
Further data analysis was conducted using the five main variables we have chosen earlier which are Q46, Q47, Q48, Q49 and Q50, together with two demographic factors which are class and age, as we believe these are the variables that will help us to achieve our primary aim of exploring an individual overall well-being.
