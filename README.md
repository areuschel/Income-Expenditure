# Income_Expenditure
Data pipeline for understanding income and expenditures of US households.

<h2>Description</h2>
The study of consumer behaviors is integral to the regulation of domestic and international trade, understanding trends in social welfare, and identifying the groups most vulnerable to market changes. Income security or insecurity has the power to alter every aspect of an individual’s life; from small everyday tasks such as deciding what to eat for lunch to having the ability and knowledge to participate in democratic processes like voting. These far-reaching applications of economic health give us the ability to pinpoint anomalies in how different groups of people earn an income and subsequently spend their money. Thus, the study of these crucial differences can give way to new programs and policies that uplift disadvantaged communities. The Bureau of Labor Statistics (BLS) is the primary government organ that gathers and analyzes data relating to consumer behavior. The dataset used in this project is completely public use provided through the BLS. Though this project will only cover findings from one fiscal quarter of 2016, the availability of this data from many other years opens the door for more long-term analyses of consumer trends.

<h2>Languages and Utilities Used</h2>

- <b>RStudio</b> 
- <b>tidyverse</b>
- <b>ClustOfVar</b>
- <b>factoextra</b>
- <b>Hmisc</b>


<h2>Project walk-through:</h2>

## Replication Paper
This project was based off an existing paper written by Mingzhao Hu, a biostatistician and lecturer at the Mayo Clinic who published a research paper in 2022 while at the University of California Santa Barbara using data from the 2016 Consumer Expenditure survey. The motivation behind this paper was to create a data analysis and visualization pipeline for understanding the relationship between demographics, income, and expenditures for US households. 

The Consumer Expenditure (CE) survey is unique in that it is the only nationwide survey of demographics, income, expenditure that measures responses on a quarterly basis. In this paper, Hu uses consumer response data from the first quarter of the 2016 fiscal year. The raw data contains 808 original variables, from which Hu initially selects 35 to perform multivariate statistical methods on. In this paper, Hu employs multivariate methods such as Principal Components Analysis (PCA), hierarchical clustering, and Canonical Correlation Analysis (CCA) to draw insights about the relationships between selected variables.

Hu, Paper: https://link.springer.com/article/10.1007/s00180-022-01251-2

BLS Data: https://www.bls.gov/cex/pumd.htm

### Feature Selection and Description
Mingzhao Hu used financial domain knowledge to select 35 of the 808 original features that represented different dimensions of income, expenditure, and demographic information. Brief descriptions of each variable are given below. For replication purposes, I used the same variables.

![Norm](/var_desc_1.png?raw=true "Test")
![Norm](/var_desc_2.png?raw=true "Test")
![Norm](/var_desc_3.png?raw=true "Test")
![Norm](/var_desc_4.png?raw=true "Test")


## Exploratory Data Analysis
### Dealing with Categorical Variables
Many of the statistical methods used in this project assume continuous variables. There are still ways to incorporate categorical information. One option will be shown in the replication section, while I propose an alternative use in the independent analysis section of this project.


Variable breakdown:
- - - - - - - - - - - - - - - - - - - - - - - - -

🌆 (14) - consumer demographics... includes 4 categorical variables

💰 (10) - consumer income

🛍️ (11) - consumer expenditures

🟰 6,426 total observations, 35 total variables

- Hu decides to one-hot encode the rest of the categorical variables, creating more columns of “1”s and “0”s. For replication purposes, I followed this step, though I will discuss other options in the independent analysis section of this paper. 

- The first flag raised in exploratory analysis was that many households reported values of “0” for total expenditure. This is unrealistic and therefore 2,171 observations are removed. Hu examines these missing observations, but no apparent patterns are found; thus, he assumes this is random.


### Missing Values

Percentage of "NA" Values and Race Demographic Breakdown
![Norm](/income_EDA.png?raw=true "Test")

- the 6 income variables in the table above were removed due to high % of missing data
- this data is heavily skewed towards white households thus challenging the robustness of racial interpretations
 
The correlation matrix for these 43 variables is large, so it is visualized with a heatmap.
![Norm](/corr_m.png?raw=true "Test")

It is important to note that the outer top right section looks like there is no relationship between many of the variables, but this is due to the many dummy variables created for race, education, and sex. Otherwise, we can note that the other income and expenditure variables are related either negatively or positively. These relationships are needed to validate assumptions for PCA and other statistical methods employed in this project.


## Statistical Methods: Replication
![Norm](/title_slide_1.png?raw=true "Test")

As always, let's check the assumptions of the first method. The following checklist follows Mingzhao Hu's paper. The assumptions not addressed in the replication paper will be addressed in the independent analysis section.

#### Checking PCA Assumptions:
- <b>Continuous variables</b>
   - ❌✅
- <b>Linear relationships between variables</b>
   - ✅
- <b>High sample size</b>
   - ✅
- <b>No significant outliers</b>
   - ❌

   


### Dataset Dilemma: Sacrifice Sample Size or Demographic Info?



### Description of Variables: Cleaned Dataset



### MANOVA: Multivariate Analysis Of Variance


#### Checking MANOVA Assumptions:
- <b>Independence between observations</b>
  - ✅
- <b>Multivariate Normality</b>
   - ❌

Royston's Test for Multivariate Normality
     
![Norm](/mvn_normality_roy.png?raw=true "Test")


- <b>Absence of multicollinearity</b>
   - ❌✅

![Cor](/cor_heatmap_1.png?raw=true "Heatmap")




### Principal Components Analysis (PCA)

#### Checking PCA Assumptions:
- <b>Continuous variables</b>
   - ✅
- <b>Linear relationships between variables</b>
   - ✅
- <b>High sample size</b>
   - ✅
- <b>No significant outliers</b>
   - ❌✅

### Full Model

1. Number of principal componenets = 4

![scree](/scree_pca1.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: 38.6%

   PC2: 17.9%

   PC3: 12.4%

   PC4: 6.3%

🎖 TOTAL: 75.2% 

4. Biplot, PC1-PC2

![biplot](/biplot_pca1.png?raw=true "PCA")

   

### Model without outliers

This model produced results nearly indentical to the full model, thus they are not described here. Documentation in the R code shows full comparison of these two models.

### Subsetting by % Free and Reduced Lunch

One of my original questions about student performance among Missouri high schools was on the effect of wealth/nourishment through the given Free and Reduced Lunch (FRL) metric. Due to the assumptions of MANOVA being violated, I could not run that multivariate test. I chose to subset my cleaned dataset by schools with 50% or more qualifying for FRL and one for less than 50%.

### PCA, <50% qualifying for FRL (💰 higher income)

1. Number of principal components = 4

![scree](/scree_low_FRL.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: 35.7%

   PC2: 23.0%

   PC3: 12.6%

   PC4: 7.7%

🎖 TOTAL: 79.0% 

3. Biplot, PC1-PC2

![scree](/biplot_low_FRL.png?raw=true "PCA")


### PCA, >50% qualifying for FRL (💰 lower income)

1. Number of principal components = 4

![scree](/scree_high_FRL.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: 39.3%

   PC2: 17.4%

   PC3: 8.9%

   PC4: 6.6%

🎖 TOTAL: 72.2% 

3. Biplot, PC1-PC2

![scree](/biplot_high_FRL.png?raw=true "PCA")



### Interpretations

![scree](/biplot_comparison.png?raw=true "PCA")

![scree](/pca_interpretations.png?raw=true "PCA")

### Future work and considerations

- Canonical Correlation Analysis
   - This method could potentially identify relationships between performance metrics and demographic makeup
- K-means clustering
   - The PCA's provided are a good basis for identifying schools similar to each other based on size and performance
