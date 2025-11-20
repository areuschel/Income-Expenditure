# Income-Expenditure
Replicated data pipeline for understanding income and expenditures of US households. Adjustments to orginal methods and those outcomes can be found in the "Independent Analysis" section of the project.

<h2>Description</h2>

<img src="/Photos/bls_slide.png" width="400">

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

<img src="/Photos/glimpse1.JPG" width="850">

## Table of Contents

### [Part I](#replication-section): Replication, Hu (2022)
   - Replication Paper Information and Description

### [Part II](#ind-section): Independent Analysis, Reuschel (2024)
   - Changing Use of Categorical Variables
   - Identifying Multivariate Outliers
   - New PCA Outcomes
   - K-means Clustering
   - New CCA Outcomes

### Conclusion
   - Paper Acknowledgement
   - Impact of Independent Analysis

<a name="replication-section" />

## Part I: Replication, Hu (2022)

<img src="/Photos/glimpse2.JPG" width="850">

## Replication Paper
This project was based off an existing paper written by Mingzhao Hu who published a [research paper](https://link.springer.com/article/10.1007/s00180-022-01251-2) in 2022 while at the University of California Santa Barbara using data from the 2016 Consumer Expenditure survey. The motivation behind this paper was to create a data analysis and visualization pipeline for understanding the relationship between demographics, income, and expenditures for US households. 

The Consumer Expenditure [(CE)](https://www.bls.gov/cex/pumd.htm) survey is unique in that it is the only nationwide survey of demographics, income, expenditure that measures responses on a quarterly basis. In this paper, Hu uses consumer response data from the first quarter of the 2016 fiscal year. The raw data contains 808 original variables, from which Hu initially selects 35 to perform multivariate statistical methods on. In this paper, Hu employs multivariate methods such as Principal Components Analysis (PCA), hierarchical clustering, and Canonical Correlation Analysis (CCA) to draw insights about the relationships between selected variables.


### Feature Selection and Description

Hu used financial domain knowledge to select 35 of the 808 original features that represented different dimensions of income, expenditure, and demographic information. For replication purposes, I used the same variables.

## Exploratory Data Analysis

### Dealing with Categorical Variables

Many of the statistical methods used in this project assume continuous variables. There are still ways to incorporate categorical information. One option will be shown in the replication section, while I propose an alternative use in the independent analysis section of this project.


#### Variable breakdown:

| Variable Type | Data Type: Numeric or Categorical | Number of variables |
|---|---|---|
| Demographics | 10 numeric, 4 categorical | 14 |
| Income Variables | numeric | 10 |
| Expenditure Variables | numeric | 11 |

🟰 6,426 total observations, 35 total variables

   - Hu decides to one-hot encode the rest of the categorical variables, creating more columns of “1”s and “0”s.
   - For replication purposes, I followed this step, though I will discuss other options in the independent analysis section of this paper. 


### Missing Values and Distribution of Race

- The table below shows variables removed due to high % of missing data
- The figure on the right shows the distribution of racial categories

<img src="/Plots/EDA_comb.JPEG" width="800">


### Correlation Matrix

The correlation matrix for these 43 variables is large, so it is visualized with a heatmap.

It is important to note that the outer top right section looks like there is no relationship between many of the variables, but this is due to the many dummy variables created for race, education, and sex. Otherwise, we can note that the other income and expenditure variables are related either negatively or positively. These relationships are needed to validate assumptions for PCA and other statistical methods employed in this project.

<img src="/Plots/corr_m.png" width="800">


## Statistical Methods: Replication

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

<img src="/Plots/Scree_rep1.png" width="700">

2. Proportion of variance explained

🎖 TOTAL: 36% explained with 5 PC's

   - Note: Although 3 was the value selected as the optimal number of PC's based off the scree plot, I'm showing that even by adding an additional 2 principal components the overall variance explained in this model is very low. Typically, I look for 70-80% of total variance explained in a well-performing model.  


### Race stratifications graphed on PC1 and PC2, Replication

Using PC1 and PC2 from the model above, observations are graphed and split according to the available race stratifications.

The most notable difference seen in my plots below is that the White respondents occupy more space above the first principal component that represents important income and expenditure variables. While the overall model did not perform exceedingly well, this raises the question of income disparities between White and non-White racial groups. For a more robust interterpretation of racial difference in consumer behavior, I would prefer to have higher sample sizes for Black, Asian, and Multi-racial family units.

<img src="/Plots/pc1_race.png" width="700">


### Single-sex Household Model, Replication

One additional group Hu extracted from this data was single-sex households. This is an interesting group to analyze as it contains information about same-sex couples, people living alone, and single parents. These groups are expected to have different consumer behaviors, which is the purpose for running these models separately. The image below shows how this subset was created.

<img src="/Plots/chunk_1.png" width="700">

#### Output: Scree Plot and Biplot

<img src="/Plots/biplot_scree_ss.JPEG" width="700">

1. Number of principal componenets = 4

2. Proportion of variance explained

   PC1: 12.2%

   PC2: 9.1%

   PC3: 7.7%

   PC4: 4.9%

🎖 TOTAL: 33.9% 

As you can see from the total proportion of variance explained, this model did not show improvement from the previous full model. Later on, I discuss changes that improve the performance of these models.

**Interpretation:**
It is clear and reasonable that the male and female variables contribute greatly overall to this model because the data was trimmed to only include single-sex homes.
Unsurprisingly, these variables point in opposite directions on this biplot. What this plot further reveals is that the variables for family size and number of kids in the household are projected in the same positive direction as the female variables on the second principal component. This tells us that many of the single-sex homes are likely to be single mothers and have more children in the household than the male single-sex households.

---

### Sparse PCA, Replication

#### 🤷🏼 What is sparse PCA?

Sparse PCA works similarly to regular PCA, but sets a new constraint on the model (k). k represents a predetermined integer value that limits the number of non-zero principal components. In other words, you are only allowing the 'top k' attributes to explain the variance of your data.

#### Pros and Cons of sparse PCA

**Pros** - reduces noise, increases interpretability of the individual principal components

**Cons** - oversimplifies the principal components

#### First, is this data sparse?

What even is sparse data? 

Data is sparse either when (1) the raw data contains many 0's or (2) you choose to one-hot encode categorical variables. Since I followed Hu's recommendation to one-hot encode the demographic variables, my current dataset is sparse.

#### Output: loadings

```{r}
sparse_pca <- nsprcomp(fmli_16, ncomp = 3, center = T, scale. = T, k = c(5,5,5), nneg = FALSE)

sparse_pca$rotation
```
<img src="/Plots/sparse_loadings.png" width = 400 />

Looking at the first three PC's, you can see that the rest of the variables are set to 0 because the k value selected was 5 (k=5). 

Recall, the first three PC's from the full model represented (1) income and expenditure, (2) age and retirement, and (3) gender. The green boxes above highlight "new" information about these components. In addition to the information we already knew about these first three components, sparse pca reveals more variables that contribute to each component that would have been difficult to pick out of the noise from the full model.

**Sparse PCA Interpretations:**

| Variables | Finding |
|---|---|
|Food, Housing Expenditures| Food and housing expenditures are two of the most influential costs to US households. It's also very reasonable that these two costs vary greatly between households based on location, family size, and lifestyle.|
|Kids, Income-earners, Elderly | Kids and the number of income-earning members are projected in the opposite direction of the retirement and elderly variables. This makes sense as we knew this principal component to represent aging populations who receive supplemental assistance from the government. |
|Sex, Vehicles| Vehicles are projected in the same direction as the male variables. However, the loading for number of vehicles is only ~0.167, which isn’t significant keeping in mind that this is a sparse PCA. Though this variable doesn't play a big role in this sparse PCA analysis, it could still be interesting for a deeper dive into gender differences in income and expenditure. |


## Clustering, Replication

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

<img src="/Plots/dend_1.png" width = 600 />

- From this dendrogram, three main clusters emerge: (1) age and family composition, (2) education and income/expenditures, and (3) gender, race, and geography.
- Family variables are grouped together on the lefthand side, and the race variables are grouped on the righthand side.
- Looking more granularly, we see that this clustering model was able to group fam_size and kids together as well as age, elderly, and retirement together in a separate grouping.

2. Dendrogram created using varclus()

<img src="/Plots/dend_2.png" width = 600 />

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

<img src="/Plots/eigen.png" width = 500 />


- The eigenvectors above show that family income, the first row of E1, contributes highly for the first canonical variate and strongly influences the next three variates as well.
  
- In the same table, we can see that the expenditure variables’ influences on each canonical variate differ more between variates, with total expenditure being the most influential on the first variate; yet housing, healthcare, and food costs contribute more to the other variates.

<a name="ind-section" />

- Overall, this tells us that income can better be generalized through total family income, whereas outcomes can greatly differ between the various expenditure dimensions.


## Part II: Independent Analysis, A. Reuschel (2024)

![title](/Photos/break.JPG?raw=true "PCA")

### 🕹️ Changing the use of categorical variables

Although I was pleasantly surprised at my ability to replicate each of Hu’s methods, I found that the results obtained were not super informative and many of the models had poor performace. My biggest qualm with the replication process was the sheer number of dummy variables added through one-hot encoding to include categorical variables. 

In addition to diluting the dataset with “0”s, this choice tip-toes around key assumptions of multivariate methods used. To rectify the issue of categorical features without losing all key demographic information, I decided to use these categories to subset the original dataset.

#### Part 1: Educational Attainment

The original data has 8 different categories of educational attainment. This was hurting the analysis because these categories were so close together and disproportionate (One category only had 22 observations total!!). I combined all 8 and split them into 2 categories: (1) 'Some College' and (2) 'No College'.

Proportion of sample:

🧑🏽‍🏫 Some College, n = 2,728

🧑🏻‍🚀 No College, n = 1,526

#### Part 2: Racial/Ethnic Groups

I did a similar tactic with the race categories as I did with educational attainment, creating “White” and “POC” groups. The data was too heavily skewed towards the White respondents that the previous 5 categories for non-White identities were difficult to draw any interpretations from. 

While I typically would discourage the grouping of all non-White racial categories together—thus eliminating the opportunity to find key differences between racial/ethnic categories— in this case, it was the most practical approach to increase sample size and balance the proportions between groups. Due to this, no interpretations are made regarding the individual non-White subgroups.

Proportion of sample:

- Race_White, n = 3,490

- Race_POC, n = 764


### 🕹️ Checking for MV Outliers

As mentioned in the replication section, Hu did not mention checking for multivariate outliers before performing PCA. 

#### 1. Selecting a Distance Threshold

With my newly made subsets (‘no college’, ‘some college’, ‘white respondents’, and ‘POC respondents’), I calculated the mahalanobis distances for each observation. With a chi-squared distribution and alpha of 0.99, the distance threshold was 42.98. For the ‘some college’ group, this identified over 400 observations as outliers, but a plot using total income and total expenditure as axes revealed that many of these fell directly on top of the general spread of data. 

I plotted these outliers on multiple different dimensions but was dissatisfied with the number of observations this cutoff would remove. Thus, I chose to increase the distance threshold from 42.98 to 60. This now only labels 280 observations as outliers, which are graphed on both income and expenditure as well as income and retirement.


![title](/Plots/outliers.JPEG?raw=true "MVN-out")

As you can see from the plots above, most of the outliers occur among respondents who are <b>not</b> receiving retirement income. This observation was consistent throughout all subsets and shows us that individuals on retirement are spending their money more responsibly and are not accounting for large discrepancies in the data. These 280 observations are removed, making the new sample size n = 3,974.

### 🔎 Principal Components Analysis (PCA)

#### My Full Model, without MV outliers

After removing multivariate outliers from each subset, I re-ran my PCAs.

The table below shows the dimensionality reduction outcomes of 3 PCA models. The first two models are replicated from Hu's paper, and the last one is the result of the full model replication with multivariate outliers removed.

Highlighted in green are two takeaways from comparing these models.

<img src="/Plots/pca_comparison.png" width = 750 />

- First, the model that explained most of the total variance was the model separating out single-sex households.
   - This could be due to the smaller sample size of homes that fit in this category.
     
- The second circled piece of information provides evidence that removing multivariate outliers improved the performance of the technique.
   - More variation is explained in less variables.
      - Hu's full model used 15 variables to explain 36.8% of total variation in the data.
      - My full model used only 12 variables to explain 37.2% of total variation in the data.
         - This enhances the performance of PCA as a dimensionality reduction technique, further narrowing down the variables we are interested in.


#### Code for setting a contribution threshold
```{r}
# setting a contribution threshold for better plots
cont_edu <- nc_pca$rotation^2
cont_sc <- sc_pca$rotation^2

# must contribute at least 5%
threshold <- 0.05

vars_keep2 <- apply(cont_edu[,1:2], 1, function(x) any(x >= threshold2))
vars_keep3 <- apply(cont_sc[,1:2], 1, function(x) any(x >= threshold2))

filt_edu <- cont_edu[vars_keep2, 1:2]
filt_sc <- cont_sc[vars_keep3, 1:2]
```
#### PCA stratifications: Educational attainment & Race

<img src="/Plots/new_biplots1.JPEG" width = 700 />

For the two educational subsets, the overall variance explained increased drastically from the replicated full model. 

- For “no college” respondents, 5 PCs explain 54.4% of the total variance and for “some college” respondents, 5 PCs explain 54.1% of the total variance.
   - Comparing this with the original replicated PCA model where only 38% of the total variance is explained in 5PCs, this shows great improvement in interpretability and model performance.
     

#### PCA Loadings, 'Some College' vs. 'No College'
(Note: I consider a loading to be 'significant' if it exceeds +/- 0.30)

```
# print loadings of first 5 PC's
sc_pca$rotation[ ,1:5]
nc_pca$rotation[ ,1:5]
```

A few interesting differences come about when viewing the loadings of these 5 PCs for the different education levels. 

   - First, healthcare costs contribute significantly for the second component for those without college educations where this variable is not considered significant in any of the first 5 PCs for the group with college educations.
      - This is relevant to note that the level of education has tangible cost consequences when it comes to healthcare. Intuitively, this makes sense that the more knowledge you have of personal health and preventative care, the less you might need to spend on expensive procedures.
      - It could also relate to higher incomes of college-educated individuals and their ability to pay for preventative checkups.

- Not a single variable in component five for the ‘no college’ group was significant, but in the ‘some college’ group, educational costs (-0.59) and the number of cars (-0.33) were both important to the fifth component.
   - This suggests that the fifth component could be explaining variation in ‘extraneous’ expenditure categories that are not necessarily important to survival like food and housing.

#### PCA Loadings, 'Race-White' vs. 'Race-POC'

I completed these same steps for the two new race groups. Again, more of the total variance was explained through 5 PCs: 56.2% for White respondents and 56.8% for the POC group. 

<img src="/Plots/pca_race2.JPEG" width = 700 />

The main difference in the biplots for these two groups is that for the White respondents, both PC1 and PC2 have all-negative coefficients where the POC group has all positive coefficients for PC2. This is incredibly revealing as it suggests the impacts of income are the same for these groups, but as they age and receive retirement benefits there is an underlying difference in structure for how POC individuals experience aging as it relates to consumer behaviors and costs.

Other findings from the loadings of these principal components communicate differences in consumer habits by these racial groups. 

- First, this PCA model was the very first out of all (recall: full model, single-sex, sparse, no college, and some college) to have government supplemental assistance programs as significant to any of the first 5 components.
   -  In the fifth component for the POC group, the variable ‘welfarem’ has a loading of -0.48.
      -  The appearance of the welfare variable highlights a unique situation for POC households that may relate to systemic disadvantages that lend POC individuals more likely to rely on government assistance.

   -  The other variables significant to this component are ‘men_o16’ and ‘educacq’.
      -  These variables, representing education and men over 16 (able to work), can be interpreted as additional struggles in gaining access to stable jobs and educational opportunities.


### 🔎 K-means Clustering

Due to the fact that I re-invented the way in which categorical variables would be used for this dataset, my new subsets only included continuous variables. This made me curious about the application of k-means clustering, which was only omitted by Hu because of his inclusion of categorical variables. 

This method relies on minimizing the within sum of squares of each cluster to delineate groups from the overall spread of data. I had already performed PCA on these new subsets, so I decided to further investigate this method by clustering observations on the first two principal components for both the educational and racial groups.

#### Education: 'No college' vs. 'Some college'
I first took the “no college” group and set the number of clusters to 3. The result obtained from this clustering almost resembled a pie chart, with the center of the circle nearly missing the origin. This makes sense conceptually as observations should be more similar based on their proximity to each principal component. The choice of 3 groups was validated through an elbow plot that showed a gradual decrease in within-cluster variation after 3.

<img src="/Plots/KMeans_2.JPEG" width = 650 />


'No college'
- Cluster 1 as denoted in the legend seems to represent the high-earners and spenders of this group as its previous biplot shows the projections for income and expenditure variables in this direction.
- Clusters 2 and 3 then subsequently represent low- earners and spenders with the difference between these groups being age primarily.

‘Some college’

- The first cluster as denoted in the legend shows many more observations in this category of high-earners and spenders as well as a few identifiable outliers belonging to this cluster.
- This validates preconceived assumptions that people with more education tend to earn and spend more money.
- The other two clusters are dense, showing a potent middle class between both young and old respondents.

#### Race: 'Race-White' vs. 'Race-POC'

Next, I performed k-means clustering on the White and POC datasets using their respective principal components. Once more, 3 clusters are observed to be the best number through elbow plots.

<img src="/Plots/KMeans_3.JPEG" width = 650 />

When analyzing the groups for the White respondents, the two densest clusters, clusters 1 and 3 according to the legend in figure 25, fall towards higher earners and spenders. 
- This is a change from the educational plots, that showed one big cluster for this group. 

When compared with the POC plots, the high-income group is once again one large cluster. Due to the spread being greater for low-income, low-spending households in both educational subsets and the POC subset, I will speculate that there is a mismatch in this dataset that skews the White respondents towards higher incomes. 
- While this could be an accurate representation of income distribution in the US, I am reluctant to make such conclusions without more proportional data for the POC group.

### 🔎 CCA

The final method tested with my changes to the dataset was CCA. My hope was that in removing outliers and subsetting the data by the educational and racial variables, I would be able to see a clearer relationship between the selected income and expenditure variables.

#### Differences in Educational Attainment

CCA Code
```{r}
# split into income & exp vars
income_no <- cca_no %>%
   select(fam_income, fam_supp, frretirm, fsalarym) # select only income vars
exp_no <- cca_no %>%
   select(-c(fam_income, fam_supp, frretirm, fsalarym)) # select NOT income vars

income_some <- cca_some %>%
   select(fam_income, fam_supp, frretirm, fsalarym)
exp_some <- cca_some %>%
   select(-c(fam_income, fam_supp, frretirm, fsalarym))

# run cca for both educational groups
cca_res_no <- cancor(income_no, exp_no)
cca_res_no

cca_res_sc <- cancor(income_some, exp_some)
cca_res_sc
```

Variable Importance Code
```
# variable importance, no college
importance_nc <- sqrt(rowSums(cca_res_no$xcoefˆ2)) # Importance of income variables
importance_nc
importance_nc2 <- sqrt(rowSums(cca_res_no$ycoefˆ2)) # Importance of expenditure variables
importance_nc2

# variable importance, some college
importance_sc <- sqrt(rowSums(cca_res_sc$xcoefˆ2)) # Importance of income variables
importance_sc
importance_sc2 <- sqrt(rowSums(cca_res_sc$ycoefˆ2)) # Importance of expenditure variables
importance_sc2
```

<img src="/Plots/CCA_table1.png" width = 650 />

The summarized table of importance above shows the overall contribution of each expenditure variable and contrasts it with the opposing group. The expenditure variables for each category are more important to the canonical variate for those without college educations compared to those with some level of higher education.

#### Differences in Racial Identity Groups

```{r}
# selecting vars
income_wht <- cca_wht %>%
   select(fam_income, fam_supp, frretirm, fsalarym)
exp_wht <- cca_wht %>%
   select(-c(fam_income, fam_supp, frretirm, fsalarym))

income_poc <- cca_poc %>%
   select(fam_income, fam_supp, frretirm, fsalarym)
exp_poc <- cca_poc %>%
   select(-c(fam_income, fam_supp, frretirm, fsalarym))

# perform cca
cca_res_wht <- cancor(income_wht, exp_wht)
cca_res_wht

cca_res_poc <- cancor(income_poc, exp_poc)
cca_res_poc
```

<img src="/Plots/CCA_table2.png" width = 650 />

This table shows a similar pattern as the educational attainment stratifications shown before. In this case, we can see that the expenditure variables more heavily influence each canonical variate in households identifying as either Black, Asian, Indigenous, or 2+ races as compared to the White households.

These results support previous conclusions on this dataset that show a level of socioeconomic privilege in the US economic system given to White households and those with college degrees.

## Conclusions

![title](/Photos/glimpse4.JPG?raw=true "PCA")

### Paper Acknowledgement

Hu’s paper provides a detailed and comprehensive view of data from the Bureau of Labor Statistics’ Consumer Expenditure survey by employing multivariate statistical methods for interpretation and visualization. The combination of PCA, clustering, and CCA is a strong foundational basis for understanding the relationships between chosen variables. 

The contribution of this paper is undeniable, given the great attention to detail in documentation and the public availability of the data. Hu’s work is greatly important to the use of the CE survey as his domain knowledge allowed him to reduce the dimensions of the dataset from 808 to 35 prior to analysis. Without this contribution, navigating through such a large dataset would have been daunting and time-consuming.

### Impact of Independent Analysis

I enjoyed the process of this project very much. I have attempted replication once before but used a different dataset than the ones the authors had. Being able to work with the same data is interesting because I can start to see where my knowledge of a subject has a role to play in making decisions about the data. 

In this case, I wanted to check and validate additional assumptions that the author missed, and this turned out to improve the performance of the models. I also thought that my use of the categorical variables enhanced the interpretation abilities of all of the methods, despite the drawbacks of simplifying the educational and racial categories.
