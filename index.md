# Portfolio

---
## Detecting Fraud in Financial Payment Services using Paysim Synthetic Dataset of Mobile Money Transactions 
### Using Holdout Sample, Undersampling for Imbalanced Dataset, Supervised Machine Larning Algorithms (Logistic Regression, Decision Tree, Random Forest, XGBoost), Visualization with R (ggplot).
[![View on GitHub](https://img.shields.io/badge/GitHub-View_on_GitHub-red?logo=GitHub)](https://github.com/jennynguyen-97/financialfrauddetection) 
[![View R code](https://img.shields.io/badge/R-View_R_Code-blue?logo=#75AADB)](https://rpubs.com/htn14/849978)

### Business Understanding
The cost of fraud pre-COVID-19 among U.S. financial services and lending firms rose 12.8% over the previous reporting period, which covered the first halves of 2018 and 2019 respectively. For every dollar of fraud lost in the pre-COVID period, U.S. financial services and lending companies incurred an average of $3.78 in costs, up from $3.35. These losses include the transaction face value for which firms are held liable, plus fees and interest incurred, fines and legal fees, labor and investigation costs and external recovery expenses. The COVID-19 pandemic has had a significantly negative impact on financial services and lending firms. The volume of successful attacks has risen across segments during COVID-19, most dramatically among larger institutions, causing a spike in the cost of fraud.

Due to the increasingly sophisticated and costly frauds, the financial services industry must continue to accelerate and innovate how it prevent, detect, and investigate fraudulent activities. One of the most promising solutions to achieve this goal is through machine learning algorithms.

![frauds-in-audit](https://user-images.githubusercontent.com/93355594/147002073-cf91f2f9-8f92-4ca1-9525-276e329314b3.jpeg)

### Data Understanding
#### Data Overview
The dataset used in this project is from [Kaggle - Paysim synthetic financial dataset for fraud detection](https://www.kaggle.com/arjunjoshua/predicting-fraud-in-financial-payment-services/data). Paysim provided synthetic data for mobile money transactions in a one month period. This dataset contains 11 attributes and roughly 6.3 million observations. There is no null value in the dataset so it does not require data cleaning.

The main technical challenge in any financial fraud dataset is the highly imbalanced distribution between 0 and 1 (or legitimate and fraudulent transactions). This project will show how imbalanced dataset is resolved and choose a suitable machine learning algorithm to deal with the skew.
  
#### Exploratory Data Analysis
First, I visualize the frequency of fraud, meaning which percentage of total transactions is fraudulent. The dataset is highly imbalanced with fraudulent transactions only account for 0.13%.

<img width="729" alt="Screen Shot 2021-12-21 at 7 13 34 PM" src="https://user-images.githubusercontent.com/93355594/147013778-247b1675-bc1e-4f7c-8947-c94a0b3d80d7.png">
  
Second, I want to know which types make up the majority of fraudulent transactions. Interestingly, only two transaction types out of five - CASH-OUT and TRANSFER have fraudulent activities. TRANSFER means money is sent to a customer/fraudster and CASH-OUT means money is sent to a merchant who pays the customer/fraudster in cash. Remarkably, the number of fraudulent TRANSFERs almost equals the number of fraudulent CASH-OUTs. These observations make sense in which frauds are committed by first transferring out funds to another account which subsequently cashes it out.

<img width="723" alt="Screen Shot 2021-12-21 at 7 24 11 PM" src="https://user-images.githubusercontent.com/93355594/147014501-e65d28b4-ac4c-40a3-8636-569e4875d652.png">

Third, it would be helpful to know the distribution of amount of fraudulent transactions. According to this histogram, fraudulent amount has positive skew with most transactions between 0 - 625,000 in local currency. There is also a low peak around 10,000,000. In total, these fraudulent transactions account for a loss of 12,056,415,428 in local currency. 

<img width="706" alt="Screen Shot 2021-12-21 at 7 36 22 PM" src="https://user-images.githubusercontent.com/93355594/147015349-b783359c-075a-4183-96ca-bcbe44481691.png">
  
Fourth, I incorporate a time element associated with fradulent activities in the analysis by taking the modulo (or the remainder) of a division between the step variable and 24. Since each step represents 1 hour of real world and there is a total of 743 steps for 30 days of data, I convert them into 24 hours where each day has 0 to 23 hours. As the graph depicts, most fraudulent transactions occur between 3 to 6 a.m. when there is a lack of real-time human monitoring. Representing time as hours instead of steps, therefore, would add predictive power to the model and represent a pattern that machine learning algorithms can detect.

<img width="710" alt="Screen Shot 2021-12-21 at 7 37 30 PM" src="https://user-images.githubusercontent.com/93355594/147016244-04e44ce2-01d7-4eab-b1f8-bd4e69fa8718.png">

### Data Preparation
I decide to drop four columns: step, nameOrig, nameDest, and isFlaggedFraud. step is used in creating the hour variable so I remove it to avoid the multicollinearity issue. nameOrig and nameDest have too many unique levels to create dummy variables. isFlaggedFraud, according to the data description, will be set when an attempt is made to TRANSFER an amount greater than 200,000; however, only 16 entries (out of more than 6 million) are set to 1 and instances where conditions are met but isFlaggedFraud is not set. Consequently, isFlaggedFraud is not consistent with the data description and will be dropped.
  
In addition, I have to determine an effective way to split the train and test sets so that the train dataset is balanced and appropriate for the modeling step. First, I split the dataset into 70:30 with 70% for train dataset and 30% for test dataset. Next, I apply under-sampling majority class method due to the highly skewed dataset. This method balances classes in the train dataset so that there are 5725 observations in each class or 50:50 split among classes 0 and 1. Balancing dataset is important in machine learning as feeding imbalanced data to classifier model can make it biased in favor of the majority class, simply because it did not have enough data to learn about the minority.
  
### Modeling & Evaluation
Since the train dataset is balanced, I do not need to consider metrics that take into account the imbalance of the dataset. I will use four metrics for model evaluation:
  1. Accuracy: measures the number of classifications a model correctly predicts
  2. Area under the curve (AUC): measures the ability of a classifier to distinguish between classes  
  3. False Positive Rate (FPR): measures the percentage of false positives in each model. I do not want a high FPR as it will reduce customer satisfaction and increase customer fraction in using our mobile money app.
  4. False Negative Rate (FNR): measures the percentage of false negatives in each model. Since the cost of undetected fraud is high, I want my model to correctly detect fraudulent transactions in order to implement preventive methods.

The performance of four above-mentioned models according to four performance criterias is listed below:

<img width="581" alt="Screen Shot 2021-12-23 at 11 59 52 AM" src="https://user-images.githubusercontent.com/93355594/147271213-7f9089d1-13e3-486a-aa41-affc8c32dde6.png">

XGBoost outperforms other models on all four metrics, suggesting its high precision and generalization on future unseen data. Consequently, I choose XGBoost as the final model to perform predictions on the train dataset. I also rank feature importance based on XGBoost. As the graph indicates,  oldbalanceOrg, newbalanceOrig, amount, and type provide high informational gains.
  
<img width="603" alt="Screen Shot 2021-12-23 at 12 04 27 PM" src="https://user-images.githubusercontent.com/93355594/147271629-4819779d-2449-4551-a3d0-dc5c408daf54.png">
  
### Deployment  
When used successfully, machine learning removes heavy burden of data analysis from the fraud detection team. The results help the team with investigation, insights, and reporting. Machine learning doesn’t replace the fraud analyst team, but gives them the ability to reduce the time spent on manual reviews and data analysis. This means analysts can focus on the most urgent cases and assess alerts faster with more accuracy, and also reduce the number of genuine customers declined.

There are, however, points in which the machine learning algorithm can be improved. First, while undersampling helps reduce the risk of machine learning algorithms skewing toward the majority class and offers less storage requirements and better run times for analyses, this method may drop potentially important information or cause the sample of the majority class chosen to be biased and not representative of real world data. In order to improve the sampling method, a combination of both undersampling and oversampling can be implemented to obtain the most lifelike dataset and accurate results. Second, unsupervised machine learning methods such as K-means or Support Vector Machine (SVM) can be implemented to capture normal data distribution in unlabeled data sets when they’re being trained. Unsupervised methods can prove particularly helpful in cases when labeling data is expensive and time-consuming. Third, machine learning methods will differ based on companies' processes and particular setting. Companies will need to conduct research and experimentation to assess what data and features they have readily available to figure out which model can help detect fraud efficiently. This XGB can serve as a base model and can be upgraded based on companies' features, characteristics, and needs.
  
### Note
Full version of the project including reproducible code can be found in github depository. The above write-up is meant to serve as an abstract of the project.

---
## Predicting Bike Share Demand for Capital Bikeshare in Washington, D.C.
### Using Supervised Machine Learning Algorithms (Linear Regression, Lasso Regression, CART, Random Forest), Holdout Sample, k-Fold Cross Validation, Visualization with Tableau and R (ggplot).

[![View on GitHub](https://img.shields.io/badge/GitHub-View_on_GitHub-red?logo=GitHub)](https://github.com/jennynguyen-97/bikesharedemand) 
[![View R code](https://img.shields.io/badge/R-View_R_Code-blue?logo=#75AADB)](http://jennynguyen-97.github.io/bikesharedemand/bikesharedemand.html)

[Capital Bikeshare](https://www.capitalbikeshare.com) is metro DC's bikeshare service, with 4,500 bikes and 500+ stations across 7 jurisdictions: Washington, DC.; Arlington, VA; Alexandria, VA; Montgomery, MD; Prince George's County, MD; Fairfax County, VA; and the City of Falls Church, VA. Designed for quick trips with convenience in mind, it’s a fun and affordable way to get around.

![CaBi-how-it-works-hero2](https://user-images.githubusercontent.com/93355594/139561008-607a884b-785c-4fa6-a6c9-ace57caca446.jpg)

The [dataset](https://www.kaggle.com/c/bike-sharing-demand/overview) includes hourly rental data spanning two years of Capital Bikeshare. The training set is comprised of the first 19 days of each month, while the test set is the 20th to the end of the month. I recognize that the dataset may be incomplete due to censored demand. That is, Capital Bikeshare only observed the realized rentals at stations at which bikes were available, not those that were not realized due to the unavailability of bikes.

### Business Problem
Every day, many people use bike sharing to get around, whether for work or leisure. Acknowledging the importance of convenience, availability, and affordability in maintaining a successful bike-share program, I aim to build a predictive model that can closely and accurately predict the actual hourly demand of bikes for each day. If bike-share programs don't have enough bikes available, riders will lower their expectations, abandon bike sharing, and switch to other transportation methods. When sufficient numbers of functioning bikes are on the ground in trafficked areas, riders will trust the bike share as a reliable means of transportation.

This project focuses on predicting the bike share demand based on available data such as date, time, holiday, working day, season, weather, temperature, humidity, and windspeed. 

### Modeling 
My recommended random forest model achieved an out-of-sample <img src="https://render.githubusercontent.com/render/math?math=R^2"> of 83.5% and RMSE of 72.98, outperformed three other machine learning algorithms: multiple linear regression, Lasso regression, and classification and regression tree (CART). 

<img width="646" alt="Screen Shot 2021-11-11 at 8 05 26 PM" src="https://user-images.githubusercontent.com/93355594/141391859-926e162d-6bdc-4d10-9504-9ac3bb63df8e.png">

The random forest model is versatile, meaning that there is very little pre-processing required and the data does not need to be rescaled or transformed. Additionally, the model has moderate bias, low variance, and is robust to outliers and non-linear data. Needless to say, besides these advantages in utilizing random forest, there are several drawbacks that firms should be aware of regarding deployment. Firstly, the random forest model is not easily interpretable. It is a predictive modeling tool and not a descriptive tool, meaning if firms are looking for a description of the relationships in their bike demand data, other approaches would be better. Secondly, random forest can't extrapolate, it can only make a prediction that is an average of previously observed labels. Consequently, if there are operational disruptions caused by natural disasters such as winter storms, tornadoes, or pandemics, the random forest model will not make accurate predictions. Firms can mitigate these limitations by using the random forest model as a part of ensemble learning.

### Conclusion
The final result shows that demand for bikes is higher during weekdays compared to weekends and during timeblocks for morning commutes (7 a.m. - 9 a.m.) and afternoon commutes (4 p.m. - 7 p.m.). Consequently, firms should develop strategies to not only provide sufficient bikes to stations during busy hours, but also reallocate bikes to appropriate stations that match the commute patterns.

![Hours](https://user-images.githubusercontent.com/93355594/140855632-73d2edc6-9669-4c7d-948d-8155d4e09be2.png)

Season and weather patterns also play a role in bike demand. Specifically, demand is higher during summer and fall with total rentals for each season approximate 916,000 and 1,050,000 rentals, respectively. Additionally, higher demand often happens mostly when the weather is clear and partly cloudy.

![Sheet 1](https://user-images.githubusercontent.com/93355594/140856152-a2bfb026-d612-4c04-b778-c96f3c46cc4f.png)

![Weather](https://user-images.githubusercontent.com/93355594/140856156-c665f67a-364d-4289-be78-95096db915ae.png)

By utilizing the information that is available ahead of time, bike-share firms can forecast the bike demand within the next week and thus reduce the uncertainty and ambiguity in their daily operations. Specifically, firms can supply and refill sufficient bikes at each dock, increase customer representative staff to assist customers on their cycling journeys, and implement an appropriate repair and maintenance schedule to minimize service disruption. Being able to predict demand ahead of time will enable bike-sharing programs to improve their operational efficiency, customer satisfaction, and business profits.

### Note
Full version of the project including reproducible code can be found in github depository. The above write-up is meant to serve as an abstract of the project.

---
## Exploring a Timeless Problem Through a Novel Lens: Happiness v.s. GDP
### Using Supervised Machine Learning Algorithms (Linear Regression, Multiple Linear Regression), Causal Modeling (Panel Regression), Cross-Sectional Data, Hypothesis Testing, Visualization with R (ggplot).

[![View on GitHub](https://img.shields.io/badge/GitHub-View_on_GitHub-red?logo=GitHub)](https://github.com/jennynguyen-97/gdpandhappiness) 
[![View R code](https://img.shields.io/badge/R-View_R_Code-blue?logo=#75AADB)](http://jennynguyen-97.github.io/gdpandhappiness/Team-Project.html)

### Motivation
Happiness levels are correlated with societal metrics such as crime rate, birth rate, economic growth, and education levels. Policy makers, consequently, often use happiness as a basis for making policy changes. I believe this topic is of particular importance in studying governmental policies and their related social implications, as the ultimate goal of a nation is to increase not only economic well-being, but also mental and physical welfare of its citizens.

**Understanding the causal relationship between changes in the happiness of a nation's citizens and changes in that nation's GDP per capita can allow governing bodies to make data-driven decisions regarding post-pandemic recovery policies towards both the economy and the well-being of their citizens.**

### Literature
There have been numerous studies over the years on happiness vs GDP, with students and experts alike analyzing data in search of an answer to the causal relationship between these two variables. I draw inspiration from some of these previous studies.

The genesis of happiness and GDP stems from [Richard Easterlin](https://halshs.archives-ouvertes.fr/halshs-00590436/) who in 1974 attempted to explain why despite the growth of the U.S. economy, the levels of national hapiness did not rise correspondingly. Easterlin analyzed the happiness levels of many Western countries over time and saw virtually no trends for changes in happiness levels despite incomes nearly doubling over the same period of time. 

<img width="667" alt="image" src="https://user-images.githubusercontent.com/93355594/140999697-00902f49-bfa1-4bfa-b8ef-5cdb3eeea63a.png">
 
Other literature results, on the other hand, show diminishing impact of GDP on Happiness. A popular theory conducted by [Proto and Rustichini](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0079358) stated that the positive relation in happiness vanishes beyond some value of increasing income. 

<img width="582" alt="image" src="https://user-images.githubusercontent.com/93355594/141002991-d651f88c-5c86-496d-acbe-6c212e6fc54e.png">
 
In agreement with Proto and Rustichini, Robert E. Lane in his 2001 book _The Loss of Hapiness in Market Democracies_ also argued that once an individual rised above a poverty line or "subsistence level", the main source of increased well-being is not income but rather friends and a good family life.
  
### Data and Exploratory Data Analyses
I utilize the Gallup World Poll's [World Happiness Report](https://www.kaggle.com/ajaypalsinghlo/world-happiness-report-2021) covering years from 2005 to 2021. The happiness scores and rankings use data from the Gallup World Poll. The columns following the happiness score estimate the extent to which each of six factors – economic production (GDP), social support, life expectancy, freedom, absence of corruption, and generosity – contribute to making life evaluations higher in each country.
 
From the correlation matrix, there are several pairs of variables that are highly correlated, specifically _social_ and _loggdp_, _healthylife_ and _loggdp_, _healthylife_ and _social_. Consequently, I will exclude _social_ and _healthylife_ variables out of the multiple linear regression model to reduce multicollinearity.

<img width="813" alt="image" src="https://user-images.githubusercontent.com/93355594/141028297-68b48892-f448-416d-8403-d64d9ac1a440.png">
 
As I am interested in the relationship between _happiness_ and _loggdp_, I visualize the relationship between these two variables.
  
<img width="706" alt="image" src="https://user-images.githubusercontent.com/93355594/141028828-dded89ff-7038-4fec-aaaa-33c005cae77e.png">
 
Indeed, there is a positive trend between _loggdp_ and _happiness_, indicating that in general as _loggdp_ increases, _happiness_ will increase as well. However, I will have to consider omitted variable bias (OVB) in my modeling.
  
### Methodology
#### A. Linear Regression (OLS)
Model: Happiness = ɑ + βLogGDP + e
 
<img width="574" alt="image" src="https://user-images.githubusercontent.com/93355594/141030004-7641b446-bdd0-47a9-ac85-eb11f303cbdf.png">
 
I can see that for 1% change in GDP, happiness index increases on average by 0.76, ceteris paribus. The relationship between happiness and GDP is also statistically significant. There seems to be a positive relationship between happiness and GDP. However, my model may suffer from OVB because there are many factors that affect happiness such as employment, equality, freedom, etc. that have not been taken into consideration. The coefficient might actually refect some other "unobserved" factors that are not included in the analysis.
 
Model: Happiness = ɑ + β<sub>1</sub>LogGDP + β<sub>2</sub>Freedom + β<sub>3</sub>Generosity + β<sub>4</sub>Corruption + e
 
<img width="572" alt="image" src="https://user-images.githubusercontent.com/93355594/141030636-75c571e7-7163-4386-8086-8a7d1cb55ea2.png">
 
The relationship between GDP and happiness is still positive and statistically significant. However, the magnitude of GDP on happiness is smaller: for 1% change in GDP, happiness index increases on average by 0.67, ceteris paribus.

#### B. Panel Regression
Panel regressions eliminate potential sources of country-specific measurement error and OVB. Panel regressions are of crucial importance for analysis based on survey data, because the questionnaires are generally different across countries, and there are pervasive effects due to culture and language on the statement made in the surveys.

I create a panel data of my current dataset with Country ID as the entity ID and Year as the time ID. I have 166 countries (entities), observed over 17 periods (2005 - 2021), for a total of 2,098 observations. Note that the panel is unbalanced in that not all countries are observed throughout 17 periods. Consider the panel nature of the dataset, perhaps happiness is different (on average) for each country (e.g., varying with perception and evaluation) or perhaps happiness is different through time (e.g., government policies). These variables are in effect omitted, and if relevant, then my causal estimate may be biased (OVB); consequently, I run a panel regression for happiness on Log GDP per capita while controlling for countries and time effects.
 
Model: index_plm <- plm(happiness~loggdp, data = index.p, model = "within", effect = "twoways")

<img width="740" alt="image" src="https://user-images.githubusercontent.com/93355594/141031366-8e1f6dd5-8d3d-4b40-87f4-d94d5706c159.png">
 
The coefficient on _loggdp_ is 1.229 and is statistically significant. This implies that the treatment effect remains positive, but the OLS specification seems to have been biased downward the effect of Log GDP on happiness by omitting entity and time effects.
 
I take a deeper look at the panel regression model by examining the effect of Log GDP per capita on Hapiness with balanced and unbalanced panel data for a period of eight years from 2014 - 2021.
 
Balanced panel data (85 countries for a total of 680 observations)

<img width="630" alt="image" src="https://user-images.githubusercontent.com/93355594/141031770-33db5a51-fc24-403b-b887-5a4ff9de3b35.png">
 
Unbalanced panel data (160 countries for a total of 1107 observations)
 
<img width="674" alt="image" src="https://user-images.githubusercontent.com/93355594/141031842-e8fdbf7d-59fa-46e9-99f2-4d4e0debfbc4.png">
 
The effect of _loggdp_ on happiness is positive and significant for both panels. However, the effect of _loggdp_ is higher in balanced panel data than it is in unbalanced panel data. The unbalanced data contains influential observations that pulls the betas toward zero.
 
Upon further investigation, I recognize that most missing data in the period 2014 - 2021 was in the year of 2020. Specifically, most under-developed or developing countries such as Afghanistan, Costa Rica, Haiti, Indonesia, and Vietnam are missing happiness records in 2020. The reason for that is because the surveys cannot be conducted in those countries due to the pandemic. Additionally, removing countries that were not observed during the entire time period 2014 - 2021 leads to significant loss of data. The unbalanced panel contains 160 countries, but balanced panel contains only 85 countries. As a result, I suspect that there are some unobserved characteristics caused by the lack of random attrition in panel data.
  
To determine if the data is random, I conduct a t-test for randomization on balanced and unbalanced panel data. t-value is 5.512 and p-value= 4.163e-08 which is significant. Consequently, the data is not randomly missing.

<img width="581" alt="Screen Shot 2021-11-11 at 9 01 26 PM" src="https://user-images.githubusercontent.com/93355594/141396067-f530fe73-2b33-44fd-aa8f-8eec75b3ead3.png">

### Findings
From both linear regression and panel regression methodologies, I can conclude that there is a positive relationship between happiness and Log of GDP per capita. The relationship indicates that people in richer countries are on average happier than people in poorer countries. My finding is consistent with my initial hypothesis.
 
The conclusion of this project is different from the Easterlin Paradox which states that there is no correlation between GDP and happiness and other academic research which indicates that average life satisfaction actually peaks with countries that have an average annual income of about $33,000; after that, life satisfaction tends to drop as wealth rises.
 
The conclusion further strengthens the importance of GDP per capita (or a sense of financial stability) in influencing and determining a nation's happiness level. Based on a statement made by [Clark, Frijters, & Shields](https://halshs.archives-ouvertes.fr/halshs-00590436/document) that "Every social scientist should be attempting to understand the determinants of happiness, and it should be happiness which is the explicit aim of government intervention," my findings conclude that policy makers and economists should put a greater emphasis on fiscal policies that restructure and expand economies post-pandemic to enhance the nation's overall well-being and life satisfaction.
  
### Conclusion
Based on my modeling of linear regression (OLS) and panel regression, I conclude that there is a positive causal relationship of GDP per capita on Happiness. Specifically, people in developed countries are on average happier and more satisfied with their lives than people in developing and under-developed countries. The coefficient of Log GDP varies based upon methodologies and control variables; however, ther is always a positive and statistically significant relationship between Log GDP and Happiness.
 
I acknowledge that there are limitations to my analysis that I can improve upon or can be a source of improvement for future studies. Specifically:
 
* One critical consideration that is relevant in my analysis is the potential existence of a two-way relationship between a nation's GDP and the happiness level of that nation's citizens. It could very well be the case where higher GDP raises the happiness of citizens as disposable income increases, but it could also be the case where happier citizens are more productive and therefore contribute to a higher national GDP. Implementation of simultaneous equations could address this issue.
* The data source can be improved to eliminate selectivity problems and to create a randomly missing panel data. If data cannot be collected in 2020 due to the global pandemic, survey conductors can try to identify and remove those influential observations by using the Cook's distance.
* GDP has a positive impact on Happiness, but the level of impact may be different across stages of economies or geographical locations. Consequently, researchers can divide countries according to the level of economy and/or the geographic location to determine if the causal relationship is constant.
  
### Note
Full version of the project including reproducible code can be found in github depository. The above write-up is meant to serve as an abstract of the project.
  








