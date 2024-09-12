# Income-Expenditure
Replicated data pipeline for understanding income and expenditures of US households. Adjustments to orginal methods and those outcomes can be found in the "Independent Analysis" section of the project.

<h2>Description</h2>

![bls](/bls_slide.png?raw=true "PCA")

The study of consumer behaviors is integral to the regulation of domestic and international trade, understanding trends in social welfare, and identifying the groups most vulnerable to market changes. Income security or insecurity has the power to alter every aspect of an individual’s life; from small everyday tasks such as deciding what to eat for lunch to having the ability and knowledge to participate in democratic processes like voting. 

These far-reaching applications of economic health give us the ability to pinpoint anomalies in how different groups of people earn an income and subsequently spend their money. Thus, the study of these crucial differences can give way to new programs and policies that uplift disadvantaged communities. 

The Bureau of Labor Statistics (BLS) is the primary government organ that gathers and analyzes data relating to consumer behavior. The dataset used in this project is completely public use provided through the BLS. Though this project will only cover findings from one fiscal quarter of 2016, the availability of this data from many other years opens the door for more long-term analyses of consumer trends.

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

![Norm](/variable_desc.JPEG?raw=true "Test")


## Exploratory Data Analysis
### Dealing with Categorical Variables
Many of the statistical methods used in this project assume continuous variables. There are still ways to incorporate categorical information. One option will be shown in the replication section, while I propose an alternative use in the independent analysis section of this project.


### Variable breakdown:
- - - - - - - - - - - - - - - - - - - - - - - - -

🌆 (14) - consumer demographics (includes the 4 categorical variables)

💰 (10) - consumer income

🛍️ (11) - consumer expenditures

🟰 6,426 total observations, 35 total variables

- Hu decides to one-hot encode the rest of the categorical variables, creating more columns of “1”s and “0”s. For replication purposes, I followed this step, though I will discuss other options in the independent analysis section of this paper. 

- The first flag raised in exploratory analysis was that many households reported values of “0” for total expenditure. This is unrealistic and therefore 2,171 observations are removed. Hu examines these missing observations, but no apparent patterns are found; thus, he assumes this is random.


### Missing Values

![eda](/Income_EDA.png?raw=true "Test")

- The 6 income variables in the table above were removed due to high % of missing data

### Race Distribution

![eda](/Race_EDA.png?raw=true "Test")

- This data is heavily skewed towards white households thus challenging the robustness of racial interpretations


### Correlation Matrix
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

#### Full Model, Replication

1. Number of principal componenets = 3

   - This number is selected from the scree plot below where the 'elbow' is. In other words, where does the proportion of variance from one PC to the next 'drop-off'/ each subsequent PC explains around the same % of variance.

![scree](/Scree_rep1.png?raw=true "PCA")

2. Proportion of variance explained

🎖 TOTAL: 36% explained with 5 PC's

   - Although 3 was the value selected as the optimal number of PC's based off the scree plot, I'm showing that even by adding an additional 2 principal components the overall variance explained in this model is very low. Typically, I look for 70-80% of total variance explained from a well-performing model.  


#### Race stratifications graphed on PC1 and PC2, Replication

Using PC1 and PC2 from the model above, observations are graphed and split according to the available race stratifications.

![race](/pc1_race.png?raw=true "PCA")

The most notable difference seen in my plots above is that the White respondents occupy more space above the first principal component that represents important income and expenditure variables. While the overall model did not perform exceedingly well, this raises the question of income disparities between White and non-White racial groups. For a more robust interterpretation of racial difference in consumer behavior, I would prefer to have higher sample sizes for Black, Asian, and Multi-racial family units.


#### Single-sex Household Model, Replication

One additional group Hu extracted from this data was single-sex households. The image below shows how this subset was created.

![chunk](/chunk_1.png?raw=true "PCA")

1. Number of principal componenets = 

![scree](/title_slide_2.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: %

   PC2: %

   PC3: %

   PC4: %

🎖 TOTAL: % 

3. Biplot, PC1-PC2

![biplot](/biplot_pca1.png?raw=true "PCA")



#### Sparse PCA, Replication
![title](/title_slide_3.png?raw=true "PCA")


## PCA, Independent Analysis
![title](/title_slide_4.png?raw=true "PCA")


#### Multivariate Outliers

#### My Full Model

After completing the replication, I had many ideas of what I wanted to do differently. 

1. Number of principal componenets = 

![scree](/scree_pca1.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: %

   PC2: %

   PC3: %

   PC4: %

🎖 TOTAL: % 

3. Biplot, PC1-PC2

![biplot](/biplot_pca1.png?raw=true "PCA")


#### Comparing PCA Models, Replication vs. Independent Analysis
![compare](/pca_comparison.png?raw=true "PCA")



## Clustering, Replication
![title](/title_slide_5.png?raw=true "PCA")


### Two approaches: ClustOfVar and varclus libraries

### Comparing results

## CCA: Canonical Correlation Analysis

#### Checking CCA Assumptions:
- <b>Continuous variables</b>
   - ✅
- <b>Linear relationships between variables</b>
   - ✅
- <b>High sample size</b>
   - ✅
- <b>No significant outliers</b>
   - ❌✅

### Comparing Results

## Further Independent Analysis

### Changing the use of categorical variables

### Checking for MV Outliers

### K-means Clustering

### Principal Components Analysis (PCA)

## Conclusions

### Paper Acknowledgement

### Impact of Independent Analysis

### Future Considerations
