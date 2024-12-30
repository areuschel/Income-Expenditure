# Income-Expenditure
Replicated data pipeline for understanding income and expenditures of US households. Adjustments to orginal methods and those outcomes can be found in the "Independent Analysis" section of the project.

<h2>Description</h2>

![bls](/Photos/bls_slide.png?raw=true "PCA")

The study of consumer behaviors is integral to the regulation of domestic and international trade, understanding trends in social welfare, and identifying the groups most vulnerable to market changes.

The Bureau of Labor Statistics (BLS) is the primary government organ that gathers and analyzes data relating to consumer behavior. The dataset used in this project is completely public use provided through the BLS. Though this project will only cover findings from one fiscal quarter of 2016, the availability of this data from many other years opens the door for more long-term analyses of consumer trends.

<h2>Languages and Utilities Used</h2>

- <b>RStudio</b> 
- <b>tidyverse</b>
- <b>ClustOfVar</b>
- <b>factoextra</b>
- <b>Hmisc</b>
- <b>nsprcomp</b>


<h2>Project walk-through:</h2>

![bls](/Photos/glimpse1.JPG?raw=true "PCA")

## Table of Contents

### Part I: Replication, Hu (2022)
   - Replication Paper Information and Description
   - Feature Selection and Description of Variables
   - EDA
   - PCA
      - Racial Stratifications
      - Same-sex Households
   - Sparse PCA
   - Hierarchical Clustering
   - CCA

### Part II: Independent Analysis, Reuschel (2024)
   - Changing Use of Categorical Variables
   - Identifying Multivariate Outliers
   - New PCA Outcomes
   - K-means Clustering

### Conclusion
   - Paper Acknowledgement
   - Impact of Independent Analysis
   - Future Considerations

## Part I: Replication, Hu (2022)

![bls](/Photos/glimpse2.JPG?raw=true "PCA")

## Replication Paper
This project was based off an existing paper written by Mingzhao Hu, a biostatistician and lecturer at the Mayo Clinic who published a research paper in 2022 while at the University of California Santa Barbara using data from the 2016 Consumer Expenditure survey. The motivation behind this paper was to create a data analysis and visualization pipeline for understanding the relationship between demographics, income, and expenditures for US households. 

The Consumer Expenditure (CE) survey is unique in that it is the only nationwide survey of demographics, income, expenditure that measures responses on a quarterly basis. In this paper, Hu uses consumer response data from the first quarter of the 2016 fiscal year. The raw data contains 808 original variables, from which Hu initially selects 35 to perform multivariate statistical methods on. In this paper, Hu employs multivariate methods such as Principal Components Analysis (PCA), hierarchical clustering, and Canonical Correlation Analysis (CCA) to draw insights about the relationships between selected variables.

Hu, Paper: https://link.springer.com/article/10.1007/s00180-022-01251-2

BLS Data: https://www.bls.gov/cex/pumd.htm

### Feature Selection and Description
Mingzhao Hu used financial domain knowledge to select 35 of the 808 original features that represented different dimensions of income, expenditure, and demographic information. Brief descriptions of each variable are given below. For replication purposes, I used the same variables.

![Norm](/Plots/variable_desc.JPEG?raw=true "Test")


## Exploratory Data Analysis
### Dealing with Categorical Variables
Many of the statistical methods used in this project assume continuous variables. There are still ways to incorporate categorical information. One option will be shown in the replication section, while I propose an alternative use in the independent analysis section of this project.


#### Variable breakdown:

🌆 (14) - consumer demographics (includes the 4 categorical variables)

💰 (10) - consumer income

🛍️ (11) - consumer expenditures

🟰 6,426 total observations, 35 total variables

- Hu decides to one-hot encode the rest of the categorical variables, creating more columns of “1”s and “0”s. For replication purposes, I followed this step, though I will discuss other options in the independent analysis section of this paper. 

- The first flag raised in exploratory analysis was that many households reported values of “0” for total expenditure. This is unrealistic and therefore 2,171 observations are removed. Hu examines these missing observations, but no apparent patterns are found; thus, he assumes this is random.


### Missing Values and Distribution of Race

- The table below shows variables removed due to high % of missing data
- The figure on the right shows the distribution of racial categories

![eda](/Plots/EDA_comb.JPEG?raw=true "Test")



### Correlation Matrix

The correlation matrix for these 43 variables is large, so it is visualized with a heatmap.

It is important to note that the outer top right section looks like there is no relationship between many of the variables, but this is due to the many dummy variables created for race, education, and sex. Otherwise, we can note that the other income and expenditure variables are related either negatively or positively. These relationships are needed to validate assumptions for PCA and other statistical methods employed in this project.

![Norm](/Plots/corr_m.png?raw=true "Test")



## Statistical Methods: Replication
![Norm](/Title_Slides/title_slide_1.png?raw=true "Test")

As always, let's check the assumptions of the first method. The following checklist follows Mingzhao Hu's paper. The assumptions not addressed in the replication paper will be addressed in the independent analysis section.

#### Checking PCA Assumptions:
- <b>Continuous variables</b>
   - ❌✅
   - categorical variables changed to binary 0's and 1's
- <b>Linear relationships between variables</b>
   - ✅
- <b>High sample size</b>
   - ✅
- <b>No significant outliers</b>
   - ❌
   - Hu does not check for outliers before using PCA

### Full Model, Replication

1. Number of optimal principal componenets = 3

   - This number is selected from the scree plot below where the 'elbow' is. In other words, where does the proportion of variance from one PC to the next 'drop-off'?/ Where does each subsequent PC explain around the same % of variance as the previous?

![scree](/Plots/Scree_rep1.png?raw=true "PCA")

2. Proportion of variance explained

🎖 TOTAL: 36% explained with 5 PC's

   - Note: Although 3 was the value selected as the optimal number of PC's based off the scree plot, I'm showing that even by adding an additional 2 principal components the overall variance explained in this model is very low. Typically, I look for 70-80% of total variance explained in a well-performing model.  


### Race stratifications graphed on PC1 and PC2, Replication

Using PC1 and PC2 from the model above, observations are graphed and split according to the available race stratifications.

The most notable difference seen in my plots below is that the White respondents occupy more space above the first principal component that represents important income and expenditure variables. While the overall model did not perform exceedingly well, this raises the question of income disparities between White and non-White racial groups. For a more robust interterpretation of racial difference in consumer behavior, I would prefer to have higher sample sizes for Black, Asian, and Multi-racial family units.


![race](/Plots/pc1_race.png?raw=true "PCA")


### Single-sex Household Model, Replication

One additional group Hu extracted from this data was single-sex households. This is an interesting group to analyze as it contains information about same-sex couples, people living alone, and single parents. These groups are expected to have different consumer behaviors, which is the purpose for running these models separately. The image below shows how this subset was created.

![chunk](/Plots/chunk_1.png?raw=true "PCA")

#### Output: Scree Plot and Biplot

![scree](/Plots/biplot_scree_ss.JPEG?raw=true "PCA")

1. Number of principal componenets = 4

2. Proportion of variance explained

   PC1: 12.2%

   PC2: 9.1%

   PC3: 7.7%

   PC4: 4.9%

🎖 TOTAL: 33.9% 

As you can see from the total proportion of variance explained, this model did not show improvement from the previous full model. Later on, I discuss changes that improve the performance of these models.

3. Biplot, PC1-PC2

Note: I set a contribution threshold of 0.05 for plotting so that only the most important variables could be clearly seen. 

Interpretation: It is clear and reasonable that the male and female variables contribute greatly overall to this model because the data was trimmed to only include single-sex homes.
Unsurprisingly, these variables point in opposite directions on this biplot. What this plot further reveals is that the variables for family size and number of kids in the household are projected in the same positive direction as the female variables on the second principal component. This tells us that many of the single-sex homes are likely to be single mothers and have more children in the household than the male single-sex households.



### Sparse PCA, Replication
![title](/Title_Slides/title_slide_3.png?raw=true "PCA")

#### 🤷🏼 What is sparse PCA?

Sparse PCA works similarly to regular PCA, but sets a new constraint on the model (k). k represents a predetermined integer value that limits the number of non-zero principal components. In other words, you are only allowing the 'top k' attributes to explain the variance of your data.

#### Pros and Cons of sparse PCA

🚀 Pros - reduces noise, increases interpretability of the individual principal components

⛓️‍💥 Cons - oversimplifies the principal components

#### First, is this data sparse?

What even is sparse data? 

Data is sparse either when (1) the raw data contains many 0's or (2) you choose to one-hot encode categorical variables. Since I followed Hu's recommendation to one-hot encode the demographic variables, my current dataset is sparse.

#### Output: loadings

```{r}
sparse_pca <- nsprcomp(fmli_16, ncomp = 3, center = T, scale. = T, k = c(5,5,5), nneg = FALSE)

sparse_pca$rotation
```

![title](/Plots/sparse_loadings.png?raw=true "PCA")

Looking at the first three PC's, you can see that the rest of the variables are set to 0 because the k value selected was 5 (k=5). 

Recall, the first three PC's from the full model represented (1) income and expenditure, (2) age and retirement, and (3) gender. The green boxes above highlight "new" information about these components. In addition to the information we already knew about these first three components, sparse pca reveals more variables that contribute to each component that would have been difficult to pick out of the noise from the full model.

Interpretations: 

1. Food and housing expenditures are two of the most influential costs to US households. It's also very reasonable that these two costs vary greatly between households based on location, family size, and lifestyle.
   
2. Kids and the number of income-earning members are projected in the opposite direction of the retirement and elderly variables. This makes sense as we knew this principal component to represent aging populations who receive supplemental assistance from the government.

3. Vehicles are projected in the same direction as the male variables. However, the loading for number of vehicles is only ~0.167, which isn’t significant keeping in mind that this is a sparse PCA. Though this variable doesn't play a big role in this sparse PCA analysis, it could still be interesting for a deeper dive into gender differences in income and expenditure.


## Clustering, Replication
![title](/Title_Slides/title_slide_5.png?raw=true "PCA")


### Two approaches: ClustOfVar and varclus libraries

#### 📍Why pick a hierarchical model?

Clustering models that use Euclidean distance calculations require continuous variables. Since this data contains both continuous and categorical binary variables, we must select a clustering technique suited for this scenario. The following clustering techniques use different linkage methods for determining similarity.

#### 📍Two options: hclustvar vs. varclus 

There are a ton of packages in R that perform hierarchical clustering. These are just two that Hu selected and used for comparison. Here are the key differences:

hclustvar()

- Similarity of variables is calculated using correlation ratio (categorical vars) & squared correlation (continuous vars)
- Clusters are formed using homogeneity of merged clusters as criterion

varclus()

- Similarity of variables is calculated using (i) Hoeffding’s D-Statistic, (ii) Spearman’s rho, OR (iii) Pearson’s rho
- Clusters are formed using one of the following linkage methods : 'average', 'complete'

### Comparing results

1. Dendrogram created using hclustvar()

![title](/Plots/dend_1.png?raw=true "PCA")

- From this dendrogram, three main clusters emerge: (1) age and family composition, (2) education and income/expenditures, and (3) gender, race, and geography.
- Family variables are grouped together on the lefthand side, and the race variables are grouped on the righthand side.
- Looking more granularly, we see that this clustering model was able to group fam_size and kids together as well as age, elderly, and retirement together in a separate grouping.

2. Dendrogram created using varclus()

![title](/Plots/dend_2.png?raw=true "PCA")

- All combinations of hyperparameters were tested. (ie. Spearman, Hoeffding, or Pearson WITH average or complete linkage)
- None of the models using this technique worked well on the data- dendrogram above shows poor hierchical structure


## CCA: Canonical Correlation Analysis

The final multivariate method I replicated from Hu's original paper is Canonical Correlation Analysis (CCA). This method is used to explore how groups of variables influence one another opposed to investigating the relationship between individual variables. 

#### 📍 Group Selection

The two selected groups for examining correlation are income and expenditure. Hu selects four of the most important income and four of the most important expenditure variables to run CCA with. This yielded the square root of eigenvalues to be (0.59, 0.19, 0.04, and 0.00). 

```{r}
# income vars
income <- cca_df %>%
   select(fam_income, fam_supp, frretirm, fsalarym)

# expenditure vars
exp <- cca_df %>%
   select(-c(fam_income, fam_supp, frretirm, fsalarym))

exp4 <- exp %>%
   select(foodcq, totexpcq, houscq, healthcq)

# run CCA
cca_result <- cancor(income, exp4)

cca_result$cor
```

The first value, 0.59, represents the correlation between linear combinations of the chosen income variables, U1, and linear combinations of the chosen expenditure variables, V1. This value is high enough to support preconceived beliefs that income and expenditure are dependent on one another. 

![title](/Plots/eigen.png?raw=true "PCA")


- The eigenvectors above show that family income, the first row of E1, contributes highly for the first canonical variate and strongly influences the next three variates as well.
  
- In the same table, we can see that the expenditure variables’ influences on each canonical variate differ more between variates, with total expenditure being the most influential on the first variate; yet housing, healthcare, and food costs contribute more to the other variates.

- Overall, this tells us that income can better be generalized through total family income, whereas outcomes can greatly differ between the various expenditure dimensions.


## Part II: Independent Analysis, A. Reuschel (2024)

![title](/Photos/break.JPG?raw=true "PCA")

### Changing the use of categorical variables

Although I was pleasantly surprised at my ability to replicate each of Hu’s methods, I found that the results obtained were not super informative and many of the models had poor performace. My biggest qualm with the replication process was the sheer number of dummy variables added through one-hot encoding to include categorical variables. 

In addition to diluting the dataset with “0”s, this choice tip-toes around key assumptions of multivariate methods used. To rectify the issue of categorical features without losing all key demographic information, I decided to use these categories to subset the original dataset.

#### Part 1: Educational Attainment

The original data has 8 different categories of educational attainment. This was hurting the analysis because these categories were so close together and disproportionate (One category only had 22 observations total!!). I combined all 8 and split them into 2 categories: (1) 'Some College' and (2) 'No College'.

Proportion of sample:

🧑🏽‍🏫 Some College, n = 2,728

🧑🏻‍🚀 No College, n = 1,526

#### Part 2: Racial/Ethnic Groups

I did a similar tactic with the race categories as I did with educational attainment, creating “White” and “POC” groups. The data was too heavily skewed towards the White respondents that the previous 5 categories for non-White identities were difficult to draw any interpretations from. 

While I typically advise against grouping all non-White racial categories together— since it can overlook the unique challenges and strengths of each racial identity— in this case, it was the most practical approach to increase sample size and balance the proportions between groups. 

Proportion of sample:

- Race_White, n = 3,490

- Race_POC, n = 764


### Checking for MV Outliers

As mentioned in the replication section, Hu did not mention checking for multivariate outliers before performing PCA. 

#### 1. Selecting a Distance Threshold

With my newly made subsets (‘no college’, ‘some college’, ‘white respondents’, and ‘POC respondents’), I calculated the mahalanobis distances for each observation. With a chi-squared distribution and alpha of 0.99, the distance threshold was 42.98. For the ‘some college’ group, this identified over 400 observations as outliers, but a plot using total income and total expenditure as axes revealed that many of these fell directly on top of the general spread of data. 

I plotted these outliers on multiple different dimensions but was dissatisfied with the number of observations this cutoff would remove. Thus, I chose to increase the distance threshold from 42.98 to 60. This now only labels 280 observations as outliers, which are graphed on both income and expenditure as well as income and retirement.

![title](/Plots/outliers.JPEG?raw=true "MVN-out")

As you can see from the plots above, most of the outliers occur among respondents who are <b>not</b> receiving retirement income. This observation was consistent throughout all subsets and shows us that individuals on retirement are spending their money more responsibly and are not accounting for large discrepancies in the data. These 280 observations are removed, making the new sample size n = 3,974.

#### My Full Model, without MV outliers

After removing multivariate outliers from each subset, I re-ran my PCAs.

![title](/Plots/new_biplots1.JPEG?raw=true "PCA-out")

For the two educational subsets, the overall variance explained increased drastically from the replicated full model. 

- For “no college” respondents, 5 PCs explain 54.4% of the total variance and for “some college” respondents, 5 PCs explain 54.1% of the total variance.
   - Comparing this with the original replicated PCA model where only 38% of the total variance is explained in 5PCs, this shows great improvement in interpretability and model performance.
   - 
- The biplots made for each educational group do not appear so different because PC1 for both represents income variables and PC2 represents age and retirement variables. This is the same as the replicated full model, which tells us that these two dimensions truly hold the most variance even when split between groups of different educational levels.

#### PCA Loadings, 'Some College' vs. 'No College'
(Note: I consider a loading to be 'significant' if it exceeds +/- 0.30)

A few interesting differences come about when viewing the loadings of these 5 PCs for the different education levels. 

   - First, healthcare costs contribute significantly for the second component for those without college educations where this variable is not considered significant in any of the first 5 PCs for the group with college educations.
      - This is relevant to note that the level of education has tangible cost consequences when it comes to healthcare. Intuitively, this makes sense that the more knowledge you have of personal health and preventative care, the less you might need to spend on expensive procedures.
      - It could also relate to higher incomes of college-educated individuals and their ability to pay for preventative checkups.

- Not a single variable in component five for the ‘no college’ group was significant, but in the ‘some college’ group, educational costs (-0.59) and the number of cars (-0.33) were both important to the fifth component.
   - This suggests that the fifth component could be explaining variation in ‘extraneous’ expenditure categories that are not necessarily important to survival like food and housing.

I completed these same steps for the two new race groups. Again, more of the total variance was explained through 5 PCs: 56.2% for White respondents and 56.8% for the POC group. The main difference in the biplots for these two groups is that for the White respondents, both PC1 and PC2 have all-negative coefficients where the POC group has all positive coefficients for PC2 (Appendix, Figures 19-20). This is incredibly revealing as it suggests the impacts of income are the same for these groups, but as they age and receive retirement benefits there is an underlying difference in structure for how POC individuals experience aging as it relates to consumer behaviors and costs.

Other findings from the loadings of these principal components communicate differences in consumer habits by these two racial groups. First, this PCA model was the very first out of all (recall: full model, single-sex, sparse, no college, and some college) to have government supplemental assistance programs as significant to any of the first 5 components. In the fifth component for the POC group, the variable ‘welfarem’ has a loading of -0.48. The other variables significant to this component are ‘men_o16’ and ‘educacq’. The appearance of the welfare variable highlights a unique situation for POC households that may relate to systemic disadvantages that lend POC individuals more likely to rely on government assistance. This in coordination with education and men over 16 (able to work, so could symbolize employment) can be interpreted as additional struggles in gaining access to stable jobs and educational opportunities.

![title](/Title_Slides/title_slide_4.png?raw=true "PCA")

1. Number of principal componenets = 

![scree](/Plots/scree_pca1.png?raw=true "PCA")

2. Proportion of variance explained

   PC1: %

   PC2: %

   PC3: %

   PC4: %

🎖 TOTAL: % 

3. Biplot, PC1-PC2

![biplot](/Plots/biplot_pca1.png?raw=true "PCA")

#### Comparing PCA Models, Replication vs. Independent Analysis
![compare](/Plots/pca_comparison.png?raw=true "PCA")


### K-means Clustering

### Principal Components Analysis (PCA)

## Conclusions

![title](/Photos/glimpse4.JPG?raw=true "PCA")

### Paper Acknowledgement

### Impact of Independent Analysis

### Future Considerations
