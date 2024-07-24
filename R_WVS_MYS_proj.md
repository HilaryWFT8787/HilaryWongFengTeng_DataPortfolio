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

### Principal Component Analysis (PCA)
We first analyzed the chosen variables with Principal Component Analysis, an unsupervised approach which allows us to summarize the given data such that it can be easily visualized and analyzed, by simplifying the complex high-dimensional data into low-dimensional representations of the dataset while retaining the trends and patterns.
```
pr.out =prcomp(newdata, scale =TRUE)
summary(pr.out)
```
![image](https://github.com/user-attachments/assets/b1d03670-c510-4cba-8510-1ee768b3d8ea)


```
pr.var =pr.out$sdev^2 
pr.var

#proportion of variance explained
pve=pr.var/sum(pr.var )
pve

plot(pve , xlab=" Principal Component ", 
     ylab=" Proportion of Variance Explained ", ylim=c(0,1) ,type="b")
```
![image](https://github.com/user-attachments/assets/baabe845-c16f-49b6-994b-1346a6118ae7)


After performing PCA, we can see the proportion of variance explained by each principal component using Table 4. In addition, the scree plot (Figure 7) displays a distinct elbow at PC2, indicating that the first two components together capture a substantial 72.75% of the dataset’s variance, and the first two components are enough for us to do our principal component analysis. 

```
pr.out$rotation
```
![image](https://github.com/user-attachments/assets/ad7793bf-e735-4a1a-bfeb-a4b7c4fad1c7)

From Table 5, we can observe that the first principal component (PC1) places a relatively large negative weight on three variables which are Q48, Q49 and Q50. This means that these three variables are having a negative relationship with PC1. In other words, individuals who are highly satisfied with their financial situation (Q50) will also be highly satisfied with their life (Q49) and have a greater deal of choice for their freedom and control in life (Q48).

In simpler terms, PC1 mainly represents an individual’s overall satisfaction with their life. An individual who are more satisfied with their financial situation will usually be more satisfied with their life as they are more likely to be able to meet their financial needs without worrying too much about financial problems. At the same time, an individual who have better financial situation will also means that they will have more freedom in their life with less financial restraint and can purchase the things that they want more frequently, which will then lead to higher satisfaction with life too.
The second principal component (PC2) places a relatively large negative weight on two variables which are Q46 and Q47., In other words, PC2 will mainly represents these two variables as their negative loadings are way larger than the other three variables.

In simpler terms, PC2 mainly captures an individual personal overall well-being. This means that a higher value of Q46 which is less happy will often leads to a higher value of Q47 which is poorer self-rated health, as these two variables are highly correlated for the PC2 component. On the other hand, individuals who are happier will often have better self-rated health. Due to these two variables having positive loadings on the PC1 component, it means that they have a negative relationship with the other 3 variables on PC1. 

```
##########
#Biplot by Age Group and Class
##########

# By Age
by_Agegroup = PCA(newdata, graph=FALSE)
fviz_pca_biplot(by_Agegroup,
                geom.ind = "point",
                pointshape = 21,
                pointsize = 2,
                fill.ind = mutate.data$Age_Group,
                col.ind = "black",
                col = "red",
                xlab = "Principal Component 1",
                ylab = "Principal Component 2",
                title = "PCA - Biplot of Scores by Age Groups",
                legend.title = list(fill = "Age Group", color
                                    = "Clusters"),
                repel = TRUE
)+
  ggpubr::fill_palette("jco")+
  ggpubr::color_palette("npg")


# By Class
by_Agegroup = PCA(newdata, graph=FALSE)
fviz_pca_biplot(by_Agegroup,
                geom.ind = "point",
                pointshape = 21,
                pointsize = 2,
                fill.ind = mutate.data$Class,
                col.ind = "black",
                col = "red",
                xlab = "Principal Component 1",
                ylab = "Principal Component 2",
                title = "PCA - Biplot of Scores by Class",
                legend.title = list(fill = "Class", color
                                    = "Clusters"),
                repel = TRUE
)+
  ggpubr::fill_palette("jco")+
  ggpubr::color_palette("npg")
```

![image](https://github.com/user-attachments/assets/db97680e-db09-4bf7-8263-516a60e583c5)
> Figure 8: Biplot of Scores by Five Age Groups 


![image](https://github.com/user-attachments/assets/f52ac33d-9e9e-4edb-a327-f4cb9ab42614)
> Figure 9: Biplot of Scores by Five Classes

To ease our visualization of the biplot, we have decided to rotate the whole plot up-side down so that the variables loadings are on the positive side on their respective component. In other words, the positive side of PC1 will mean higher scale value of Q48, Q49 and Q50 which are higher satisfaction with overall life, while the positive side of PC2 will means higher scale value of Q46 and Q47 which are less happy. According to Figure 8 and 9, the variables Q48, Q49 and Q50 are having positive loadings on PC1, indicating that these three variables are positively correlated with this principal component. Then, Q46 and Q47 have negative loadings on PC1, implying a negative relationship with Q48, Q49 and Q50 on PC1. Next, all the five variables have positive loadings for PC2. Although that is the case, PC2 will represent Q46 and Q47 as these two variables have loadings that are more parallel to the PC2 axis. Not just that, we can observe that Q46 and Q47 are highly correlated as their loadings are very close to each other. Similarly, Q48, Q49 and Q50 are highly correlated too as their loadings are close to each other.

According to the two biplots above along with their categorical variable groupings, all the 500 respondents are scattered rather evenly. However, we can also observe that there are some respondents being at the far top left of the biplot and some slightly at the far bottom left of the biplot. Hence, the biplots can be explained in terms of quadrants such as below:
•	First quadrant (Top right): Greater deal of choice for freedom and control in life, higher satisfaction with life, higher satisfaction with household financial situation, feeling less happy, poorer self-rated health.
•	Second quadrant (Top left): Lesser deal of choice for freedom and control in life, lower satisfaction with life, lower satisfaction with household financial situation, feeling less happy, poorer self-rated health.
•	Third quadrant (Bottom left): Lesser deal of choice for freedom and control in life, lower satisfaction with life, lower satisfaction with household financial situation, feeling happier, good self-rated health.
•	Fourth quadrant (Bottom right): Greater deal of choice for freedom and control in life, higher satisfaction with life, higher satisfaction with household financial situation, feeling happier, good self-rated health.

### Clustering
Clustering analyzation has been utilized to identify the underlying groupings and patterns that are embedded in the dataset. This allows a better understanding of the factors that may affect the general cultural shifts, economic dynamics, self-satisfaction and evolving societal values. Clustering methods of hierarchical clustering and k-mean are applied into the analyzation to identify any similar attribute present within the dataset.  

```
km = kmeans(pr.out$x[,c(1:2)],centers=6,nstart=20)
plot(pr.out$x[,1:2],type="n"); 
text(pr.out$x[,1:2],rownames(newdata),col=(km$cluster),cex=0.6)
```

![image](https://github.com/user-attachments/assets/8b569a56-d9d1-42b8-bacf-f279f296ed9f)

From the cluster above, it can be identified that the clustering k-mean had produced 6 distinct clustering sets. The clustering above can be explained by the following descriptions in respect of PC1 and PC2:

•	Clustering 1 (Vermilion): People that are not only unwell to average on personal well-being yet dissatisfied on their overall life. 

•	Clustering 2 (Magenta): People that are well to average on personal well-being but dissatisfied on their overall life. 

•	Clustering 3 (Light blue): People that are unwell on personal well-being but averagely satisfied on their overall life. 

•	Clustering 4 (Black): People that are very well on their personal well-being and averagely to quite satisfied on their overall life.

•	Clustering 5 (Baby Blue): People that are having average well on personal well-being yet quite satisfied with their overall life. 

•	Clustering 6 (Lime Green): People that are having average well on personal well-being with very satisfied overall life. 

### Exploratory Factor Analysis (EFA)

This exploratory factor analysis (EFA), an interdependence technique that validates the dependent variables (DV) share common variance with each other, is utilized the aforementioned clustering to discover further on the pattern within each group while producing significant understanding for the project. Therewith, the Kaiser-Meyer-Olkin (KMO) test and Bartlett's test of Sphericity are performed in this factor analysis validity based on the dataset employed in PCA. With that, KMO test, which had analyzed with the Measure of Sampling Adequacy (MSA), and Bartlett’s test is performed to determine the EFA is beneficial for this dataset. Thus, the MSA and Bartlett’s test results are as followed: 
```
efadata=as.matrix(cor(newdata))

KMO(efadata)

cortest.bartlett(efadata, n=500, diag = TRUE)
```

![image](https://github.com/user-attachments/assets/3fc158f4-89a8-46e2-983e-1a9b2af01995)

Based on Table 6, the 5 DVs from the data frame had passed the KMO test and the Bartlett’s test, with MSA of 0.79 which is surpassed 0.5 and having p-value of 3.932362e^(-172) that is lower than 0.05 respectively. Henceforth, this demonstrated that the dataset has sufficient correlations among the variables to proceed with the factor analysis effectively. 

Furthermore, to make the factor structure interpretable, oblique rotation is executed to acquire more theoretically meaningful factors. Therewith, oblique rotation is considered more flexible and realistic as it is essential that underlying dimensions are assumed to be correlated with each other. With that, this characteristic makes the rotation more accurate, aligning well with the clustering of variables mentioned earlier. Hence, factanal() function was implemented to exercise the factor analysis on the dataset with factor number of 2, which was obtained from the aforementioned clustering analysis. 


```
#oblique rotation
rotation1 <- factanal(covmat = efadata, factors = 2, n.obs = 500, rotation = "promax")
#p-value > 0.05, valid model

rotation1

#orthogonal rotation
rotation2 <- factanal(covmat = efadata, factors = 2, n.obs = 500, rotation = "varimax")
#p-value > 0.05

rotation2
```

![image](https://github.com/user-attachments/assets/6b1f2433-0bd4-4dc6-85ce-54e30161acf8)

Further results displayed as followed:
-	Test of the hypothesis that 2 factors are sufficient.
-	The chi square statistic is 1.26 on 1 degree of freedom.
-	The p-value is 0.262.

With that, the eigenvalue can be obtained from Table 8 for Factor 1 and Factor 2 are 1.942 and 1.183 respectively. Moreover, the p-value produced from the calculation is 0.262, which is more than 0.05, and is considered as a valid model for the dataset that does not reject the null hypothesis of 2 factor solution. 

## Discussion (Findings)
After performing data visualization and further data analysis, we have found a few interesting findings. Firstly, we can see that individuals with a lower scale value for Q46 (very happy) will usually have a lower scale for Q47 (very good self-rated health) as shown by the high correlation of these two variables in PCA. This goes the same for Q48, Q49 and Q50 as shown by their high correlation in PCA. These findings can also be proved by using the EFA or the correlation heatmap, as the pair of variables are usually in the same factor in the EFA or are highly correlated with each other in the correlation heatmap.

Surprisingly, we have also found that no matter which age group or classes the individuals came from, all of them respond fairly different from one another. However, we can still see some differences by using the violin plots. Firstly, we can see that the upper class and upper-middle class will usually have higher satisfaction in their financial situation and a greater deal of choice for freedom and control in their life compared to the lower classes. Secondly, we can see that middle-aged adult (40-49 years old) will usually have higher satisfaction in their household financial situation and a greater deal of choice for freedom and control in their life compared to the other age groups. Other than these two variables (Q50, Q48), the remaining three variables (Q46, Q47, Q49) show fairly similar results across all age groups or classes which we have decided not to include their violin plots in this particular report.

Furthermore, it is also evidence that there are some individuals who are having low happiness while also rating their own health low. This awareness in Malaysia may stem from concerns about the health conditions individuals face, including obesity, diabetes, ischemic heart disease, cancer, and stroke (Khaw et al.,2023). Therewith, individuals may experience heightened concern for their own health, leading to unexpected self-doubt and the potential onset of mental disorders such as depression. Other the other hand, there are also numerous individuals having less freedom of choice and control in their lives while also dissatisfied with their financial household and their lives from the findings. Less freedom of choice may cause by restriction by the parents, occupied time on studying and strict house rules; dissatisfaction in financial household and life may be due to family supports, stress on workplace, and restrained by current stable job and future wealth as previously mentioned. 
These disgruntlements may also contribute to the development of mental illnesses.

With that, Malaysian Government should take necessary action to improve individuals’ general wellness from these issues. Not to mention, the increment of interest in extending access to mental health care beyond conventional healthcare settings is evident. Governments can leverage research initiatives to evaluate the effectiveness of these approaches, linking the outcomes to grantmaking (Frank et al., 2023). Moreover, communities that encourage mental health often provide opportunities for social interaction through active transportation choices like walking, biking, and public transportation. BC Healthy Communities (n.d.) stated that these community attributes have protective effects on mental health by fostering trust, social connectedness among residents, and reducing social isolation which may benefit the individuals from disgruntlements faced. To address the shortage of behavioral health providers, it is also crucial to invest in programs that attract providers to the field, especially in underserved communities. Governments also can make substantial investments in youth mental health services, including early childhood mental health programs and funding for community schools that provide mental health services (House, 2022).

## Discussion (Limitations and Recommendations)
When performing multivariate analysis, it is essential to recognize and consider the constraints that might be inherent in each multivariate technique. After performing a few multivariate techniques on this World Values Survey Wave 7 dataset, we have found that there are indeed some limitations regarding these multivariate techniques.

Firstly, the Principal Component Analysis was not able to show us a clear difference between the categories of demographic factors, which are class and age. For example, five classes of individuals are scattered rather evenly on the biplot, which we were not able to differentiate exactly which class belongs to which quadrant more. An example would be, we were not able to know if individuals from the upper class usually have a higher satisfaction in life or feeling happier compared to other classes. In this case, we feel like the violin plots are more suitable to see the differences between each category of class or age for each of the five variables (Q46-Q50).

Next, we also performed K-means Clustering which showed us 6 different clusters based on the scores given by the respondents. Similarly to PCA, there is a limitation which we could not identify which category level would most likely be in which cluster. For example, the senior age group are in all the 6 clusters, which we were not able to know which cluster they are more likely to be into.

Lastly, EFA uses chi-square as a test statistic, which might be sensitive to large numbers of observations such as the dataset’s observations, which in this case we have 500 observations which are quite large. Although EFA has successfully showed us which variables can be grouped together as a group, it does not give us the certainty of whether the factors are correct as a model due to EFA’s nature as an ‘exploratory’ factor analysis.

Other than limitations for multivariate techniques, there might also be some limitations exist for the research and dataset itself. We know that this research was based solely on the respondents’ perception. Due to this reason, there might be some inequality in choice as different individuals tend to have different perceptions on the scale value of the research. For example, some might view the scale of 7 as a high rating, while some might view it as an average rating which is caused by different perception from person-to-person due to individual differences in terms of attitude, beliefs and upbringing (Saylor Academy, n.d.). We can see that the scale value choice range for Q48, Q49 and Q50 is too flexible where it ranges from 1 to 10. Each respondent might have different ways to quantify their perception in terms of scale value from 1 to 10. With that, it is recommended that each variable should have a smaller range of scale values, such as 1 to 5, with indication of lower satisfaction to higher satisfaction, so that the results from the respondents are not impacted by the scale of choice. Not just that, it is also recommended for each variable to have a same range of scale values. In this research, Q46 has 4 scale values, Q47 has 5 scale values, while Q48, Q49 and Q50 have 10 scale values. This might cause the respondents to keep changing their perceptions in terms of scale values. For example, the respondents will firstly view the scale value of 5 as high rating for the Q47 variables as it is the highest scale value, then for the Q50 variable, they will need to change their perception again as 5 is the middle rating for Q50, which is totally different from the previous variables. This process will keep going on for the other variables which have different range of scale values. Due to that, it is recommended that all variables to have a standard range of scale values with a smaller range such as 1 to 5, so that the results will be more accurate in terms of the respondents’ perceptions.

## Conclusion
Analysis of World Values Survey Wave 7 in Malaysia dataset had yielded important discoveries about the state of individuals in Malaysia population. By employing multivariate approaches, it produced significant insight on individual’s well-being and satisfaction in overall life. Although the approaches do exhibit some limitations to an extent, it also be an advantage in testing the dataset reliability and accuracy. Hence, policymakers or interventions may take this insight into consideration for improve the quality of life in Malaysia. 

Additionally, it is also important to note that some Malaysian exhibited mental illness that should take into consideration for future generation and population health. Malaysian government should build reliable and evidence base for preventive mental health care, increase community connectivity, expanding the behavioral health workforce and expanding early childhood and school-based intervention services to booth and improve future Malaysian mental health. 

In conclusion, our research provides beneficial information into the wellness of Malaysian, populations’ satisfaction in general lives, and the conundrum Malaysian had to faced. This insight may inform and benefit the targeted interventions or policies in Malaysia that aimed to enhance the quality of life for the future population. 



# Reference 
BC Healthy Communities. (n.d.). Four ways local governments can support mental health through community design | BC Healthy Communities. https://bchealthycommunities.ca/four-ways-local-governments-can-support-mental-health-through-community-design/

Frank et al. (2023). Four actions the federal government can take to address mental health in the U.S. www.commonwealthfund.org. https://doi.org/10.26099/92wy-ek23

House, W. (2022, July 15). FACT SHEET: President Biden to Announce Strategy to Address Our National Mental Health Crisis, As Part of Unity Agenda in his First State of the Union. The White House. https://www.whitehouse.gov/briefing-room/statements releases/2022/03/01/fact-sheet-president-biden-to-announce-strategy-to-address-our-national-mental-health-crisis-as-part-of-unity-agenda-in-his-first-state-of-the-union/

Khaw, W., Chan, Y. M., Nasaruddin, N. H., Alias, N., Tan, L., & Ganapathy, S. S. (2023). Malaysian burden of disease: years of life lost due to premature deaths. BMC Public Health, 23(1). https://doi.org/10.1186/s12889-023-16309-z

Saylor Academy. (n.d.). Differences in perception. https://saylordotorg.github.io/text_business- communication-for-success/s07-03-differences-in-perception.html 










