
## PROJECT: SURVIVAL ANALYSIS OF EXPRESSION DATA FROM PRIMARY COLORECTAL CANCERS IN R

### 1. Importing libraries
The below 3 libraries are used for this analysis.

![1a.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/1a.png)

### 2. Description of data

Samples were taken from colorectal cancers in surgically resected specimens in 290 colorectal cancer patients. The data for which the disease free survival time and censoring information was not available were removed. The final number of patients for which the data was available was 226. Survival analysis was done with this final data.

The variables that were available in the data are:

![2.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/2.png)

#### Data Modification
The below columns were added which are modified from the existing ones:
![3.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/3.png)

First few datas:
![4b.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/4b.png)

### 3.	Basic descriptive statistics

#### Summary of data
![5.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/5.png)

![6a.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/6a.png)

#### Event Distribution
![7a.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/7a.png)

From the summary of the data we find,
- The sample size is 226(sampleID).
- Tumour have 4 locations in this sample and has data of the patients at cancer stage A, B and C (location and dukes_stage).
- Youngest age at which cancer diagnosed in the sample is 26 and oldest is 92(age_diag).
- There are 106 females and 120 males in the data(gender).
- The trial period is 142.55 months or 11.9 years(dfs_time and dfs_timeYears).
- Out of 226 patients, 22 have undergone adjuvant radiation therapy and 87 have undergone adjuvant chemo therapy(adjXRT and adjCTX).
- There are 27 patients in the age group 25 to 50 and 199 above 50(age_group2).
- 22 patients between age 25 and 48. Almost equal number of patients in 49 – 69 and above 70(age_group3).
- 88 patients have undergone atleast one of the adjuvant therapies(adjT2).
- 21 has undergone both adjuvant therapies, 67 has undergone exactly one of the adjuvant therapies and 138 have not undergone any of the adjuvant therapies(adjT3).

### 4. Questions asked, methods used, results
The data has the detail whether the patients were undergoing adjuvant therapies. So I am analysing if the adjuvant therapies makes any difference in progression of cancer based on cancer stage, age, gender and tumour location of the subject.
Note: For the analysis, I am considering time in year.
Finally, the created model is used to find the high risk subjects and for prediction. 

#### Method 1 : Non Parametric Analysis for single sample

#### *Survival function with Kaplan-Meier Estimator*
-	According to Kaplan-Meier test, there is a 50% chance that cancer might make a progress within 4.16 years (49.8 months) with a confidence interval of 3.74 - 4.65 (44.8 - 55.7).

![8.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/8.png)

#### *Nelson-Aalen Estimator*
-	With Nelson-Aalen estimator, there is a 50% chance of progression of cancer within 4.21 years (slightly different from Kaplan-Meier), but confidence interval exactly same as Kaplan-Meier 3.74 - 4.65.

![9.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/9.png)

#### Method 2 : Non Parametric Analysis (group based)
Here the LogRank test is used to do the statistical test based on : adjuvant therapies (adjXRT, adjCTX), age_group2 and gender. LogRank works for more than 2 groups also, but since it is hard to read we do this only for 2 group covariates.

#### *LogRank based on gender*
From the LogRank test we can see:
-	p-value = 0.4 which is very large. So we do not reject H<sub>0</sub> i.e. there is no significant difference between female and male survival. Since there is no difference between both groups, it is better to consider this as a single sample to get a single estimate rather than stratifying them as female and male.

![10.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/10.png)

#### *LogRank based on age_group2*
The null hypothesis for the test is <br>H<sub>0</sub>∶S<sub>a</sub>(t)=S<sub>b</sub>(t)	<br>where <br>S<sub>a</sub>(t) is the survival function of patients in age group 25 – 50, <br>S<sub>b</sub>(t) is the survival function of patients of age greater than 50

-	From LogRank, p-value = 0.05 which is small and so we do not reject H<sub>1</sub>. i.e. the survival function of these 2 groups are different. So we do the survival function (Kaplan-Meier) for this samples.

From Kaplan-Meier we can infer that:
-	Patients below and equal to 50 years has higher survival compared to those above 50.
-	There is a 50% chance of cancer progression within 5.39 years with a confidence interval of 4.21 to 8.31 for patients in age group 25-50.
-	There is a 50% chance of cancer progression within 4 years with a confidence interval of 3.66 to 4.54 for patients above 50 years.

![11.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/11.png)

But each age groups have different stages of cancer which might confound our results. So to check this, a stratified LogRank test is done based on dukes_stage.

***Stratified LogRank for age_group2 based on dukes_stage***
-	The p-value = 0.02 which is still very less. So there is significant difference in survival between the patients for age groups 25-50 and 50+ irrespective of the cancer stage.

![12.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/12.png)

#### *LogRank based on adjuvant chemotherapy (adjCTX)*
-	From LogRank, p-value = 0.3 which is very large. So we do not reject H<sub>0</sub> i.e.. there is no significant difference in survival function between whether undergone adjCTX or not. Since there is no significant difference between both groups, it is better to consider this as a single sample to get a single estimate rather than stratifying them based on adjuvant chemo therapy.
-	From the Kaplan-Meier, the median survival for those undergone chemo therapy and not are 4.41 and 3.78 respectively but with an overlapping confidence interval of (3.99-5.42) and (3.51-4.60). So it is not significant.

![13.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/13.png)

#### *LogRank based on adjuvant radiation therapy (adjXRT)*
-	From LogRank test, p-value = 0.02 which is small and so we do not reject H<sub>1</sub>. i.e. the survival function of these 2 groups are different. So we will check the Kaplan-Meier estimates(survival function) for both.

From the Kaplan-Meier test we can infer:
-	There is a 50% chance of cancer progression within 4.1 years for patients who have not undergone adjuvant radiation therapy.
-	There is a 50% chance of cancer progression within 6.21 years for patients who have undergone adjuvant radiation therapy.
-	So undergoing adjuvant radiation therapy is statistically better than not undergoing one.
-	In the survival curve we can see that patients undergone radiation therapy has a higher survival.

![14.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/14.png)

#### *Conclusion from LogRank:*
From logrank tests above we clearly see that:
-	There is no significant difference in survival based on gender and adjuvant chemotherapy.
-	There is difference in survival of patients based on age_group2 and adjuvant radiation therapy. But we will confirm this with Cloglog plot of survival.

From the survival curves in cloglog scale:
-	For age_group2, the curves are not parallel, so the risks are not proportional and the model does not hold.

![15.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/15.png)

From the survival curves in cloglog scale:
-	For adjXRT, the curves are parallel, so this is good. But the data points are less.

![16.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/16.png)

#### Method 3 : Regression : Cox Proportional Hazards Model (coxph)
LogRank works for more than 2 groups also, but hard to read. So Coxph is used to analyse continuous variates and those with more than 2 groups. Here I check for survival dependency on age at diagnosis, location of tumour, stage of cancer, age_group3, adjT3.

#### *Cox Regression based on age_diag*
-	Age at which cancer is diagnosed is significant as p-value is 0.02 (less than 5%). The positive coefficient means higher the age, higher the risk. The hazards ratio 1.015 indicates an increase in risk of 1.5% as the age increase by one year.

![17.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/17.png)

#### *Cox Regression based on age_group3*
-	Beta coefficient is positive for the age groups '49-69' and '70+', they have a higher risk compared to the age group '25-48'. The hazards ratio is 1.60 and 1.68 for the age groups '49-69' and '70+' respectively, indicating 60 and 68 percent higher risk. But since the p-value greater than 5%, this is not very significant.

![18.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/18.png)

#### *Cox Regression based on adjT3*
-	Risk is twice higher for patients who have not done any of the adjuvant therapies(adjT3None) compared to those who have done both adjuvant therapies. Hazards ratio is 2.3 with confidence interval of 1.12-4.74 and the p-value = 0.02 makes this very significant.
-	Risk is twice higher for patients who have done only one of the adjuvant therapies(adjT3Single) compared to those who have done both adjuvant therapies. Hazards ratio is 2.3 with confidence interval of 1.08-4.85 and the p-value = 0.03 makes this very significant.

![19.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/19.png)

#### *Cox Regression based on dukes_stage*
-	Stage A has significantly lower risk compared to stage B with p-value=0.02.
-	Stage C is not significantly different from stage B as p value=0.099 which is greater than 5%.

![20.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/20.png)

From above coxph models, we find age_diag, adjT3 and dukes_stage are significant. Now coxph with multiple covariates are done.

#### Cox Regression with multiple covariates
#### *Coxph model with age_diag, dukes_stage, location, adjT3*
-	Negative coefficient for dukes_stageA with a hazards ratio of 0.59 indicates a 41% lower risk for stage A cancer compared to stage B cancer with confidence interval 0.39-0.89. The p-value=0.01 makes this highly significant.
-	Negative coefficient for dukes_stageC means a lower risk in stage C than B. But since the p-value=0.35 is very high this is not significant. ie Stage C cancer is not significantly different from stage B.
-	The positive coefficient for the age means higher the age, higher the risk. The hazards ratio 1.012 indicates an increase in risk of 1.2% as the age increase by one year. But this is not very significant as p-value=0.09 is greater than 5%. 
-	Positive coefficients for adjT3None and adjT3Single means lower risk for patients who did both adjuvant radiation and chemo therapies compared to others. Hazards ratio is 2 in both cases which means risk is twice higher compared to a patient undergone both therapies with confidence intervals of 1.12-5.27 and 1.08-5.29. This is significant as p-value is 0.03 which is less than 5%.
-	Beta coefficient for all tumour locations compared to 'Colon' is negative means lower risk in all locations compared to 'Colon', but p-value for all 3 is very large (0.63, 0.52, 0.68) which makes this insignificant.

![21.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/21.png)

Since location is not very significant, we will check for the model without location : **age_diag, dukes_stage, adjT3**

#### *Coxph model with age_diag, dukes_stage, adjT3*
-	Positive coefficient for the age means higher the age, higher the risk. The hazards ratio 1.014 with a confidence interval of 1.00-1.03 indicates an increase in risk of 1.4% as the age increase by one year. This is significant as p-value=0.039 is less than 5%.

![22.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/22.png)

Also adjT3 is a combination variable of adjXRT and adjCTX. Since adjCTX is not significant, we will create the model with adjXRT instead of adjT3.

#### *Coxph model with age_diag, dukes_stage, adjXRT*
-	Positive coefficient for adjXRTN means higher risk for patients who have not done adjuvant radiation therapy compared to others. Hazards ratio is 2.4 with confidence interval of 1.16-4.93 which means risk is twice higher compared to the patients undergone adjuvant radiation therapy. This is very significant as p-value is 0.019 which is less than 5%.

![23.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/23.png)

#### Comparing the models with concordance scores
Comparing the above 2 models : **dukes_stage + age_diag + adjT3** & **dukes_stage + age_diag + adjXRT** with concordance scores(Figure 3.14 & Figure 3.15), the second one seems to be the better one with higher concordance score (0.59).

#### Comparing the models with AIC scores
-	On comparing the above models, we get **dukes_stage + age_diag + adjXRT** as the better model with lower AIC score of 1496.490.
Note:
fit_multiCPH1 => dukes_stage + age_diag + adjT3 + location
fit_multiCPH2 => dukes_stage + age_diag + adjT3
fit_multiCPH2 => dukes_stage + age_diag + adjXRT

![24.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/24.png)

#### Automatic model selection based on AIC (stepwise AIC) 
This resulted in the same previous model as **dukes_stage + age_diag + adjXRT**.

![25.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/25.png)

#### Model Diagnostics
To see if the model is good enough, Shoenfeld residuals is also checked. 

The p-values are large, so the model is good. So the final model chosen is with covariates : **dukes_stage, age_diag and adjXRT**.

![26.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/26.png)

### 4.	Results and Conclusions

From the model chosen (**age_diag+adjXRT+dukes_stage** (Figure 3.15)) and other tests above, we get the following results:

-	Undergoing adjuvant radiation therapy is statistically better than not undergoing one.
-	There is a 50% chance of cancer progression within 4.1 years for patients who have not undergone adjuvant radiation therapy.
-	There is a 50% chance of cancer progression within 6.21 years for patients who have undergone adjuvant radiation therapy.
-	The patients who have not undergone adjuvant radiation therapy has twice higher risk compared to those who have done adjuvant radiation therapy.  Hazards ratio is 2.4 with confidence interval of 1.16-4.93. This is very significant as p-value is 0.019 which is less than 5%.
-	Risk increases with age of the subject.
-	Higher the age, higher the risk. The hazards ratio 1.014 with a confidence interval of 1.00-1.03 indicates an increase in risk of 1.4% as the age increase by one year. The p-value=0.04 makes this significant.
-	Stage B and C subjects have higher risk and they are not significantly different from each other.
-	There is 41% lower risk for stage A cancer compared to stage B cancer with confidence interval 0.39-0.89. The p-value=0.01 makes this highly significant.
-	Progression of cancer is not related to location of the tumour.
-	There is no significant difference between female and male survival.
-	There is no significant difference in survival function based on whether the patient has undergone adjuvant chemotherapy or not.

### Top High Risk subjects
Based on the model chosen, we will now identify the subjects at high risk:

![27.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/27.png)

![28a.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/28a.png)

### Prediction
To do prediction with the model, the data is split into train and test data. 80% of the data is selected randomly as the train data and the model is trained with this data. We use this model to predict the scores for the test data, find the AUC and plot the ROC. AUC with this model is 0.84.

![29.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/29.png)

![30.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/30.png)

![31.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/31.png)

### Prediction of  a particular subject
We can also predict the survival of a particular subject using the survfit function as follows:

![32.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/32.png)

![33.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/33.png)
