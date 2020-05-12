# Analysis of CRC data

#### Importing libraries


```R
library(survival)
library(tidyverse)
```

## 1. Description of data

Samples were taken from colorectal cancers in surgically resected specimens in 290 colorectal cancer patients. The data for which the disease free survival time and censoring information was not available were removed. The final number of patients for which the data was available was 226. Survival analysis was done with this final data.
The variables that were available in the data are:
<div class="something" color="yellow" markdown="1">
  ## Heading 2
  Some **bold** text.
</div>

<table style="border: 1px solid purple" align="left" markdown="1">
<tr style="background-color: purple;color:white">
<th>Slno</th>
<th>Variable Name</th>
<th>Description</th>
</tr>
    
<tr>
<td>1</td>
<td>sampleID</td>
<td>ID</td>
</tr>
    
<tr>
<td>2</td>
<td>location</td>
<td>Tumor location</td>
</tr>

<tr>
<td>3</td>
<td>dukes_stage</td>
<td>cancer stage (Duke's classification)</td>
</tr>

<tr>
<td>4</td>
<td>age_diag</td>
<td>age at diagnosis</td>
</tr>
    
<tr>
<td>5</td>
<td>gender</td>
<td>gender</td>
</tr>
    
<tr>
<td>6</td>
<td>dfs_time</td>
<td>Disease Free Survival (DFS) time, in months</td>
</tr>
    
<tr>
<td>7</td>
<td>dfs_event</td>
<td>DFS event: 1=event time, 0=censoring time</td>
</tr>
    
<tr>
<td>8</td>
<td>adjXRT</td>
<td>adjuvant radiation therapy</td>
</tr>
    
<tr>
<td>9</td>
<td>adjCTX</td>
<td>adjuvant chemotherapy</td>
</tr>
</table>

### Data Modification
The below columns were added which are modified from the existing ones:
<table style="border: 1px solid purple" align="left">
	<tr style="background-color: purple;color:white">
		<th>Variable Name</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>dfs_timeYears</td>
		<td>Disease Free Survival (DFS) time, in years</td>
	</tr>
	<tr>
		<td>age_group2</td>
		<td>identify the age group in (25-50) and (above 50+)</td>
	</tr>
	<tr>
		<td>age_group3</td>
		<td>identify the age group in (25-39), (40-59), (60-79), (80+)</td>
	</tr>
	<tr>
		<td>adjT2</td>
		<td>shows whether the patient has undergone an adjuvant therapy or not</td>
	</tr>
	<tr>
		<td>adjT3</td>
		<td>shows whether the patient has undergone: <br>no adjuvant therapy(None), <br>both adjuvant therapy(Both), <br>one of the adjuvant therapy(Single)</td>
	</tr>
</table>


```R
cdat <- mutate(cdat, dfs_timeYears = dfs_time * 30.5 / 365.25, 
               age_group2 = as.factor(ifelse(age_diag <= 50, '25-50', '50+')), 
               age_group3 = as.factor(ifelse(age_diag <=48, '25-48',
                                             ifelse(age_diag > 48 & age_diag <= 69, '49-69', '70+'))),
               adjT2 = as.factor(ifelse(adjXRT =='N' & adjCTX == 'N', 'N', 'Y')),
               adjT3 = as.factor(ifelse(adjXRT =='N' & adjCTX == 'N', 'None',
                                       ifelse(adjXRT =='Y' & adjCTX == 'Y', 'Both','Single'))))
```

<table style="border: 1px solid purple" align="left">
    <tr style="background-color:purple;color:white"><th> sampleID </th> <th> location </th> <th> dukes_stage </th> <th> age_diag </th> <th> gender </th> <th> dfs_time </th> <th> dfs_event </th> <th> adjXRT </th> <th> adjCTX </th> <th> dfs_timeYears </th> <th> age_group2 </th> <th> age_group3 </th> <th> adjT2 </th> <th> adjT3 </th>  </tr>
    <tr><td> GSM358341 </td> <td> Right </td> <td> A </td> <td align="right"> 78.00 </td> <td> M </td> <td align="right"> 3.64 </td> <td align="right"> 1.00 </td> <td> N </td> <td> N </td> <td align="right"> 0.30 </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
    <tr><td> GSM358342 </td> <td> Rectum </td> <td> A </td> <td align="right"> 53.00 </td> <td> F </td> <td align="right"> 14.53 </td> <td align="right"> 0.00 </td> <td> N </td> <td> N </td> <td align="right"> 1.21 </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
    <tr><td> GSM358343 </td> <td> Left </td> <td> A </td> <td align="right"> 80.00 </td> <td> F </td> <td align="right"> 16.47 </td> <td align="right"> 1.00 </td> <td> N </td> <td> N </td> <td align="right"> 1.38 </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
    <tr><td> GSM358344 </td> <td> Left </td> <td> A </td> <td align="right"> 58.00 </td> <td> M </td> <td align="right"> 19.75 </td> <td align="right"> 1.00 </td> <td> N </td> <td> N </td> <td align="right"> 1.65 </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
    <tr><td> GSM358345 </td> <td> Left </td> <td> A </td> <td align="right"> 81.00 </td> <td> M </td> <td align="right"> 20.02 </td> <td align="right"> 1.00 </td> <td> N </td> <td> N </td> <td align="right"> 1.67 </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
    <tr><td> GSM358346 </td> <td> Right </td> <td> A </td> <td align="right"> 57.00 </td> <td> M </td> <td align="right"> 23.96 </td> <td align="right"> 1.00 </td> <td> N </td> <td> N </td> <td align="right"> 2.00 </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
</table>

## 2. Basic descriptive statistics

<table style="border: 1px solid purple" align="left">
    <tr style="background-color:purple;color:white">
        <th>sampleID</th>
        <th>location</th>
        <th>dukes_stage</th>
        <th>age_diag</th>
        <th> gender</th>
        <th>dfs_time</th>
        <th>dfs_event</th>
        <th>adjXRT</th>
        <th>adjCTX</th>
        <th>dfs_timeYears</th>
    </tr>
    <tr>
        <td> Length:226         </td>
        <td> Rectum: 30   </td>
        <td> A:41   </td>
        <td> Min.   :26.00   </td>
        <td> F:106   </td>
        <td> Min.   :  0.92   </td>
        <td> Min.   :0.0000   </td>
        <td> N:204   </td>
        <td> N:139   </td>
        <td> Min.   : 0.07682   </td>
    </tr>
    <tr>
        <td> Class :character   </td>
        <td> Colon :  2   </td>
        <td> B:94   </td>
        <td> 1st Qu.:58.00   </td>
        <td> M:120   </td>
        <td> 1st Qu.: 22.28   </td>
        <td> 1st Qu.:1.0000   </td>
        <td> Y: 22   </td>
        <td> Y: 87   </td>
        <td> 1st Qu.: 1.86069   </td>
    </tr>
    <tr>
        <td> Mode  :character   </td>
        <td> Left  : 93   </td>
        <td> C:91   </td>
        <td> Median :67.00   </td>
        <td>  </td>
        <td> Median : 38.46   </td>
        <td> Median :1.0000   </td>
        <td>  </td>
        <td>  </td>
        <td> Median : 3.21158   </td>
    </tr>
    <tr>
        <td>  </td>
        <td> Right :101   </td>
        <td>  </td>
        <td> Mean   :66.03   </td>
        <td>  </td>
        <td> Mean   : 43.52   </td>
        <td> Mean   :0.7788   </td>
        <td>  </td>
        <td>  </td>
        <td> Mean   : 3.63396   </td>
    </tr>
    <tr>
        <td>  </td>
        <td>  </td>
        <td>  </td>
        <td> 3rd Qu.:75.00   </td>
        <td>  </td>
        <td> 3rd Qu.: 59.50   </td>
        <td> 3rd Qu.:1.0000   </td>
        <td>  </td>
        <td>  </td>
        <td> 3rd Qu.: 4.96852   </td>
    </tr>
    <tr>
        <td>  </td>
        <td>  </td>
        <td>  </td>
        <td> Max.   :92.00   </td>
        <td>  </td>
        <td> Max.   :142.55   </td>
        <td> Max.   :1.0000   </td>
        <td>  </td>
        <td>  </td>
        <td> Max.   :11.90356   </td>
    </tr>
</table>
<table style="border: 1px solid purple" align="left">
    <tr style="background-color:purple;color:white">
        <th> age_group2 </th>
        <th> age_group3 </th>
        <th> adjT2 </th>
        <th> adjT3 </th>
    </tr>
    <tr>
        <td> 25-50: 27   </td>
        <td> 25-48: 22   </td>
        <td> N:138   </td>
        <td> Both  : 21   </td>
    </tr>
    <tr>
        <td> 50+  :199   </td>
        <td> 49-69:103   </td>
        <td> Y: 88   </td>
        <td> None  :138   </td>
    </tr>
    <tr>
        <td>  </td>
        <td> 70+  :101   </td>
        <td>  </td>
        <td> Single: 67   </td>
    </tr>
</table>


From the summary of the data we find,
- The sample size is 226(sampleID).
- Tumor have 4 locations in this sample and has data of the patients at cancer stage A, B and C (location and dukes_stage).
- Youngest age at which cancer diagnosed in the sample is 26 and oldest is 92(age_diag).
- There are 106 females and 120 males in our data(gender).
- The trial period is 142.55 months or 11.9 years(dfs_time and dfs_timeYears).
- Out of 226 patients, 22 have undergone adjuvant radiation therapy and 87 have undergone adjuvant chemo therapy(adjXRT and adjCTX).
- There are 27 patients in the age group 25 to 50 and 199 above 50(age_group2).
- 22 patients between age 25 and 48. Almost equal number of patients in 49 – 69 and above 70(age_group3).
- 88 patients have undergone atleast one of the adjuvant therapies(adjT2).
- 21 has undergone both adjuvant therapies, 67 has undergone exactly one of the adjuvant therapies and 138 have not undergone any of the adjuvant therapies(adjT3).

### Event Distribution

<table style="border: 1px solid purple" align="left">
<tr style="background-color: purple;color:white">
<th>No. of Censorings (0)</th>
<th>No. ofEvents (1)</th>
</tr>
<tr>
<td style="border: 1px solid #ddd">50</td>
<td style="border: 1px solid #ddd">176</td>
</tr>
</table>

## 3. Questions asked, methods used, results
Based on the data given, I will :
- Estimate the survival of the patients
- Analyse if the progression of cancer for these patients depend on their age, their cancer stage, location of tumor, the adjuvant therapies they have done and their gender.
- Create a model and do a risk stratification to identify the subjects at high risk.
- Note : time in years is considered for doing the analysis.

## Non Parametric Analysis for single sample
### Survival function with Kaplan-Meier Estimator
- According to Kaplan-Meier test, there is a 50% chance that cancer might make a progress within 4.16 years (49.8 months) (median) with a confidence interval of 3.74 - 4.65 (44.8 - 55.7).


```R
fitKM <- survfit(Surv(dfs_timeYears, dfs_event) ~ 1, data = cdat)
fitKM
summary(fitKM)
plot(fitKM, main = "Kaplan-Meier estimator", conf.int = T, ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
```

<img src="../Plots/fitKM.png" align="left"/>
<img src="../Plots/Fig1.1.png" align="left"/>

### Nelson-Aalen Estimator
- With Nelson-Aalen estimator, there is a 50% chance of progression of cancer within 4.21 years (slightly different from Kaplan-Meier), but confidence interval exactly same as Kaplan-Meier 3.74 - 4.65.


```R
fitNA <- survfit(Surv(dfs_timeYears, dfs_event) ~ 1, data = cdat, type = "fh")
fitNA
plot(fitNA, main = "Nelson-Aalen estimator", ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
```

<img src="../Plots/fitNA.png" align="left"/>
<img src="../Plots/fig1.2.png" align="left"/>

## Non Parametric Analysis (group based)
Here the LogRank test is used to do the statistical test based on : adjuvant therapies (adjXRT, adjCTX), age_group2 and gender. LogRank works for more than 2 groups also, but since it is hard to read we do this only for covariates with 2 groups.

### LogRank based on gender
The null hypothesis for the test is 
$H0∶ S_f(t)=S_m(t)$	where
$S_f(t)$ is the survival function of female,
$S_m(t)$ is the survival function of male.

From the LogRank test we can see:
- p-value = 0.4 which is very large. So we do not reject H0 ie. there is no significant difference between female and male survival. Since there is no difference between both groups, it is better to consider this as a single sample to get a single estimate rather than stratifying them as female and male.

This can be seen from the Kaplan-Meier plot too:
- Males have a slightly higher survival but with an overlapping confidence interval. This survival difference becomes insignificant as p-value is large.



```R
genderLogRank <- survdiff(Surv(dfs_timeYears, dfs_event) ~ gender, data = cdat)
genderLogRank
```

<img src="../Plots/genderLR.png" align="left"/>


```R
fitKMGen <- survfit(Surv(dfs_timeYears, dfs_event) ~ gender, data = cdat)
fitKMGen
```

<img src="../Plots/genderKM.png" align="left"/>


```R
plot(fitKMGen, main = "Kaplan-Meier estimator", col = 1:2, conf.int = T, ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
legend(9, 1, legend=c("Female", "Male"), col=1:2, lty=1:1)
```

<img src="../Plots/fig1.3.png" align="left"/>

### LogRank based on age_group2
The null hypothesis for the test is 
$H0∶S_a(t)=S_b(t)$	where
			$S_a(t)$ is the survival function of patients in age group 25 – 50,
			$S_b(t)$ is the survival function of patients of age greater than 50.

From the test we see:
- p-value = 0.05 which is small and so we do not reject H1. ie the survival function of these 2 groups are different. So we do the survival function (Kaplan-Meier) for this samples.

From Kaplan-Meier we can infer that:
- Patients below and equal to 50 years has higher survival compared to those above 50.
- There is a 50% chance of cancer progression within 5.39 years with a confidence interval of 4.21 to 8.31 for patients in age group 26-50.
- There is a 50% chance of cancer progression within 4 years with a confidence interval of 3.66 to 4.54 for patients above 50 years.



```R
ageLogRank <- survdiff(Surv(dfs_timeYears, dfs_event) ~ age_group2, data = cdat)
ageLogRank
```

<img src="../Plots/age2LR.png" align="left"/>


```R
fitKMage2 <- survfit(Surv(dfs_timeYears, dfs_event) ~ age_group2, data = cdat)
fitKMage2
```

<img src="../Plots/age2KM.png" align="left"/>


```R
plot(fitKMage2, main = "Kaplan-Meier estimator", col = 1:2, ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
legend(8.5, 1, legend=c("Age 26-50", "Age 50+"), col=1:2, lty=1:1)
```

<img src="../Plots/fig1.4.png" align="left"/>

But each age groups have different stages of cancer which might confound our results. So to check this, a stratified LogRank test is done based on dukes_stage.

**Stratified LogRank for age_group2 based on dukes_stage**
- The p-value = 0.02 which is still very less. So there is significant difference in survival between the patients for age groups 25-50 and 50+ irrespective of the cancer stage.


```R
ageStrat <- survdiff(Surv(dfs_timeYears, dfs_event) ~ age_group2 + strata(dukes_stage), data = cdat)
ageStrat
```

<img src="../Plots/ageStrat.png" align="left"/>

### LogRank based on adjuvant chemotherapy (adjCTX)
The null hypothesis for the test is 
$H0∶S_y(t)=S_n(t)$	where
			$S_y(t)$ is the survival function of patients undergone adjuvant chemotherapy,
			$S_n(t)$ is the survival function of patients not undergone adjuvant chemotherapy.
- From LogRank we see p-value = 0.3 which is very large. So we do not reject H0 ie. there is no significant difference in survival function between whether undergone adjCTX or not. Since there is no significant difference between both groups, it is better to consider this as a single sample to get a single estimate rather than stratifying them based on adjuvant chemo therapy.
- From Kaplan-Meier, the median survival for those undergone chemo therapy and not are 4.41 and 3.78 repectively but with an overlapping confidence interval of (3.99-5.42) and (3.51-4.60). So it is not significant.


```R
adjCTXLogRank <- survdiff(Surv(dfs_timeYears, dfs_event) ~ adjCTX, data = cdat)
adjCTXLogRank
```

<img src="../Plots/CTXLR.png" align="left"/>


```R
fitKMadjCTX <- survfit(Surv(dfs_timeYears, dfs_event) ~ adjCTX, data = cdat)
fitKMadjCTX
```

<img src="../Plots/CTXKM.png" align="left"/>


```R
plot(fitKMadjCTX, main = "Kaplan-Meier estimator", col = 1:2, conf.int=T, ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
legend(8, 0.85, legend=c("No CT", "Undergone CT"), col=1:2, lty=1:1)
```

<img src="../Plots/fig1.5.png" align="left"/>

### LogRank based on adjuvant radiation therapy (adjXRT)
The null hypothesis for our test is 
$H0∶S_y(t)=S_n(t)$	where
			$S_y(t)$ is the survival function of patients undergone adjuvant Radiation therapy,
			$S_n(t)$ is the survival function of patients not undergone adjuvant Radiation therapy.
            
From LogRank test we see:
- p-value = 0.02 which is small and so we do not reject H1. ie the survival function of these 2 groups are different. So we will check the Kaplan-Meier estimates(survival function) for both.

From the Kaplan-Meier test we can infer:
- There is a 50% chance of cancer progression within 4.1 years for patients who have not undergone adjuvant radiation therapy.
- There is a 50% chance of cancer progression within 6.21 years for patients who have undergone adjuvant radiation therapy.
- So undergoing adjuvant radiation therapy is statistically better than not undergoing one.
- In the survival curve we can see that patients undergone radiation therapy has a higher survival.


```R
adjXRTLogRank <- survdiff(Surv(dfs_timeYears, dfs_event) ~ adjXRT, data = cdat)
adjXRTLogRank
```

<img src="../Plots/XRTLR.png" align="left"/>


```R
fitKMadjXRT <- survfit(Surv(dfs_timeYears, dfs_event) ~ adjXRT, data = cdat)
fitKMadjXRT
```

<img src="../Plots/XRTKM.png" align="left"/>


```R
plot(fitKMadjXRT, main = "Kaplan-Meier estimator", col = 1:2, conf.int=T, ylab = "Survival probability", xlab = "time (years)")
abline(h=0.5, col = "blue")
legend(8, 0.85, legend=c("No RT", "Undergone RT"), col=1:2, lty=1:1)
```

<img src="../Plots/fig1.6.png" align="left"/>

### Conclusion from LogRank:
From logrank tests above we clearly see that:
- There is no signficant difference in survival based on gender, adjuvant chemotherapy.
- There is difference in survival of patients based on age_group2 and adjuvant radiation therapy. But we will confirm this with Cloglog plot of survival.

From the survival curves in cloglog scale:
- for age_group2, the curves are not parallel, so the risks are not proportional and the model does not hold.
- for adjXRT, the curves are parallel, so this is good.


```R
clogXRT <- survfit(Surv(dfs_timeYears, dfs_event) ~ adjXRT, data = cdat)
plot(clogXRT, main = "Cloglog for adjXRT", fun= "cloglog", col = 1:2)
legend(0.1, 1, legend=c("No RT", "Undergone RT"), col=1:2, lty=1:1)
```

<img src="../Plots/clogxrt.png" align="left"/>


```R
clogAge2 <- survfit(Surv(dfs_timeYears, dfs_event) ~ age_group2, data = cdat)
plot(clogAge2, main = "Cloglog for age_group2", fun= "cloglog", col = 1:2)
legend(0.1, 1, legend=c("25-50", "50+"), col=1:2, lty=1:1)
```

<img src="../Plots/clogage2.png" align="left"/>

## Regression : Cox Proportional Hazards Model (coxph)
LogRank works for more than 2 groups also, but hard to read. So Coxph is used to analyse continuous variates and those with more than 2 groups. Here I check for survival dependency on age at diagnosis, location of tumor, stage of cancer, age_group3, adjT3.

### Cox Regression based on age_diag
- Age at which cancer is diagnosed is significant as p-value is less than 5%. The positive coefficient means higher the age, higher the risk. The hazards ratio 1.015 indicates an increase in risk of 1.5% as the age increase by one year.


```R
fit_AgeCPH <- coxph(Surv(dfs_timeYears, dfs_event) ~ age_diag, data = cdat)
summary(fit_AgeCPH)
```

<img src="../Plots/fig1.7.png" align="left"/>

### Cox Regression based on age_group3
- Beta coefficient is positive for the age groups '49-69' and '70+', they have a higher risk compared to the age group '25-48'. The hazards ratio is 1.60 and 1.68 for the age groups '49-69' and '70+' respectively, indicating 60 and 68 percent higher risk. But since the p-value greater than 5%, this is not very significant.


```R
fit_Age3CPH <- coxph(Surv(dfs_timeYears, dfs_event) ~ age_group3, data = cdat)
summary(fit_Age3CPH)
```

<img src="../Plots/fig1.8.png" align="left"/>

### Cox Regression based on adjT3
- Risk is twice higher for patients who have not done any of the adjuvant therapies(adjT3None) compared to those who have done both adjuvant therapies. Hazards ratio is 2.3 with confidence interval of 1.12-4.74 and the p-value = 0.02 makes this very significant.
- Risk is twice higher for patients who have done only one of the adjuvant therapies(adjT3Single) compared to those who have done both adjuvant therapies. Hazards ratio is 2.3 with confidence interval of 1.08-4.85 and the p-value = 0.03 makes this very significant.


```R
fit_adjT3CPH <- coxph(Surv(dfs_timeYears, dfs_event) ~ adjT3, data = cdat)
summary(fit_adjT3CPH)
```

<img src="../Plots/fig1.9.png" align="left"/>

### Cox Regression based on dukes_stage
- Stage A has significantly lower risk compared to stage B with p-value=0.02.
- Stage C is not significantly different from stage B as p value=0.099 which is greater than 5%.


```R
cdat <- mutate(cdat, 
               dukes_stage = relevel(dukes_stage, ref = "B"))
fit_stageCPH <- coxph(Surv(dfs_timeYears, dfs_event) ~ dukes_stage, data = cdat)
summary(fit_stageCPH)
```

<img src="../Plots/coxduke.png" align="left"/>

### Cox Regression with multiple covariates
Now we will check the cox model based on : age_diag, dukes_stage, location, adjT3
- Negative coefficient for dukes_stageA with a hazards ratio of 0.59 indicates a 41% lower risk for stage A cancer compared to stage B cancer with confidence interval 0.39-0.89. The p-value=0.01 makes this highly significant.
- Negative coefficient for dukes_stageC means a lower risk in stage C than B. But since the p-value=0.35 is very high this is not significant. ie Stage C cancer is not significantly different from stage B.
- The positive coefficient for the age means higher the age, higher the risk. The hazards ratio 1.011 indicates an increase in risk of 1.1% as the age increase by one year. But this is not very significant as p-value=0.09 is greater than 5%. 
- Positive coefficients for adjT3None and adjT3Single means lower risk for patients who did both adjuvant radiation and chemo therapies compared to others. Hazards ratio is 2 in both cases which means risk is twice higher compared to a patient undergone both therapies with confidence intervals of 1.12-5.27 and 1.08-5.29. This is significant as p-value is 0.03 which is less than 5%.
- Beta coefficient for all tumor locations compared to 'Colon' is negative means lower risk in all locations compared to 'Colon', but since p-value for all 3 is very large (0.68, 0.63, 0.52) which makes this insignificant.


```R
cdat <- mutate(cdat, 
               dukes_stage = relevel(dukes_stage, ref = "B"))
fit_multiCPH1 <- coxph(Surv(dfs_timeYears, dfs_event) ~ dukes_stage + age_diag + adjT3 + location, data = cdat)
summary(fit_multiCPH1)
```

<img src="../Plots/fig2.png" align="left"/>

Since location is not very significant, we will check for the model without location.
- Negative coefficient for dukes_stageA with a hazards ratio of 0.59 indicates a 41% lower risk for stage A cancer compared to stage B cancer with confidence interval 0.40-0.89. The p-value=0.01 makes this highly significant.
- Negative coefficient for dukes_stageC means a lower risk in stage C than B. But since the p-value=0.29 is very high this is not significant. ie Stage C cancer is not significantly different from stage B.
- Positive coefficient for the age means higher the age, higher the risk. The hazards ratio 1.014 with a confidence interval of 1.00-1.03 indicates an increase in risk of 1.4% as the age increase by one year. This is significant as p-value=0.039 is less than 5%.


```R
cdat <- mutate(cdat, dukes_stage = relevel(dukes_stage, ref = "B"))
fit_multiCPH2 <- coxph(Surv(dfs_timeYears, dfs_event) ~ dukes_stage + age_diag + adjT3, data = cdat)
summary(fit_multiCPH2)
```

<img src="../Plots/fit_multiCPH2.png" align="left"/>

Also adjT3 is a combination variable of adjXRT and adjCTX. Since adjCTX is not significant, we will create the model with adjXRT instead of adjT3.
- Positive coefficient for adjXRTN means higher risk for patients who have not done adjuvant radiation therapy compared to others. Hazards ratio is 2.4 with confidence interval of 1.16-4.93 which means risk is twice higher compared to the patients undergone adjuvant radiation therapy. This is very significant as p-value is 0.018 which is less than 5%.


```R
cdat <- mutate(cdat, dukes_stage = relevel(dukes_stage, ref = "B"))
fit_multiCPH3 <- coxph(Surv(dfs_timeYears, dfs_event) ~ dukes_stage + age_diag + adjXRT, data = cdat)
summary(fit_multiCPH3)
```

<img src="../Plots/fig2.1.png" align="left"/>

### Comparing with concordance scores
Comparing the above 2 models (dukes_stage + age_diag + adjT3 & dukes_stage + age_diag + adjXRT) with concordance scores, the second one seems to be the better one with higher concordance score.

### Comparing the models with AIC scores:
On comparing the above models, we get dukes_stage + age_diag + adjXRT as the better model with lower AIC score of 1496.490.


```R
fits <- list(fit_multiCPH1 = fit_multiCPH1, fit_multiCPH2 = fit_multiCPH2, fit_multiCPH3 = fit_multiCPH3)
sapply(fits, AIC)
```

<img src="../Plots/fig2.2.png" align="left"/>

Automatic model selection based on AIC (stepwise AIC) resulted in the same previous model as dukes_stage + age_diag + adjXRT.


```R
fitCPHFull <- coxph(Surv(dfs_timeYears, dfs_event) ~ location + dukes_stage + age_diag + gender + adjXRT + adjCTX, data = cdat)
fitCPHInitial <- coxph(Surv(dfs_timeYears, dfs_event) ~ 1, data = cdat)
MAICmodel <- step(fitCPHInitial, direction = "forward", steps = 15, scope = list(lower = fitCPHInitial, upper = fitCPHFull))
```

<img src="../Plots/fig2.3.png" align="left"/>

### Model Diagnostics
To see if the model is good enough, Shoenfeld residuals is also checked. The p-values are large, so the model is good. So the final model chosen is with covariates : dukes_stage, age_diag and adjXRT.


```R
residual.scho <- cox.zph(fit_multiCPH3)
residual.scho
```

<img src="../Plots/fig2.4.png" align="left"/>

## Prediction :
Based on the model, we will identify subjects at high risk:


```R
d_new <- select(cdat, -dfs_time, -dfs_event, -dfs_timeYears)
head(d_new)
```

<table style="border: 1px solid purple" align="left">
<tr style="background-color: purple;color:white"> <th>  </th> <th> sampleID </th> <th> location </th> <th> dukes_stage </th> <th> age_diag </th> <th> gender </th> <th> adjXRT </th> <th> adjCTX </th> <th> age_group2 </th> <th> age_group3 </th> <th> adjT2 </th> <th> adjT3 </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> GSM358341 </td> <td> Right </td> <td> A </td> <td align="right"> 78.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
  <tr> <td align="right"> 2 </td> <td> GSM358342 </td> <td> Rectum </td> <td> A </td> <td align="right"> 53.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
  <tr> <td align="right"> 3 </td> <td> GSM358343 </td> <td> Left </td> <td> A </td> <td align="right"> 80.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
  <tr> <td align="right"> 4 </td> <td> GSM358344 </td> <td> Left </td> <td> A </td> <td align="right"> 58.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
  <tr> <td align="right"> 5 </td> <td> GSM358345 </td> <td> Left </td> <td> A </td> <td align="right"> 81.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> </tr>
  <tr> <td align="right"> 6 </td> <td> GSM358346 </td> <td> Right </td> <td> A </td> <td align="right"> 57.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> </tr>
   </table>



```R
d_segmented <-
  d_new %>%
  mutate(risk_score = predict(fit_multiCPH3, newdata = d_new, type = "lp"))
head(d_segmented)
```

<table style="border: 1px solid purple" align="left">
<tr style="background-color: purple;color:white"> <th>  </th> <th> sampleID </th> <th> location </th> <th> dukes_stage </th> <th> age_diag </th> <th> gender </th> <th> adjXRT </th> <th> adjCTX </th> <th> age_group2 </th> <th> age_group3 </th> <th> adjT2 </th> <th> adjT3 </th> <th> risk_score </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> GSM358341 </td> <td> Right </td> <td> A </td> <td align="right"> 78.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> -0.10 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> GSM358342 </td> <td> Rectum </td> <td> A </td> <td align="right"> 53.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> <td align="right"> -0.44 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> GSM358343 </td> <td> Left </td> <td> A </td> <td align="right"> 80.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> -0.08 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> GSM358344 </td> <td> Left </td> <td> A </td> <td align="right"> 58.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> <td align="right"> -0.38 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> GSM358345 </td> <td> Left </td> <td> A </td> <td align="right"> 81.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> -0.06 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> GSM358346 </td> <td> Right </td> <td> A </td> <td align="right"> 57.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 49-69 </td> <td> N </td> <td> None </td> <td align="right"> -0.39 </td> </tr>
   </table>


```R
d_segmented %>%
arrange(desc(risk_score))%>%
head(10)
```

<table style="border: 1px solid purple" align="left">
<tr style="background-color: purple;color:white"> <th>  </th> <th> sampleID </th> <th> location </th> <th> dukes_stage </th> <th> age_diag </th> <th> gender </th> <th> adjXRT </th> <th> adjCTX </th> <th> age_group2 </th> <th> age_group3 </th> <th> adjT2 </th> <th> adjT3 </th> <th> risk_score </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> GSM358442 </td> <td> Right </td> <td> B </td> <td align="right"> 92.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.61 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> GSM358448 </td> <td> Right </td> <td> B </td> <td align="right"> 92.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.61 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> GSM358408 </td> <td> Left </td> <td> B </td> <td align="right"> 89.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.57 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> GSM358410 </td> <td> Right </td> <td> B </td> <td align="right"> 86.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.53 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> GSM358434 </td> <td> Right </td> <td> B </td> <td align="right"> 86.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.53 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> GSM358407 </td> <td> Right </td> <td> B </td> <td align="right"> 84.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.50 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> GSM358389 </td> <td> Rectum </td> <td> B </td> <td align="right"> 83.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.49 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> GSM358431 </td> <td> Left </td> <td> B </td> <td align="right"> 83.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.49 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> GSM358444 </td> <td> Rectum </td> <td> B </td> <td align="right"> 82.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.47 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> GSM358446 </td> <td> Right </td> <td> B </td> <td align="right"> 82.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.47 </td> </tr>
   </table>

<table style="border: 1px solid purple" align="left">
<tr style="background-color: purple;color:white"> <th>  </th> <th> sampleID </th> <th> location </th> <th> dukes_stage </th> <th> age_diag </th> <th> gender </th> <th> adjXRT </th> <th> adjCTX </th> <th> age_group2 </th> <th> age_group3 </th> <th> adjT2 </th> <th> adjT3 </th> <th> risk_score </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> GSM358442 </td> <td> Right </td> <td> B </td> <td align="right"> 92.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.61 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> GSM358448 </td> <td> Right </td> <td> B </td> <td align="right"> 92.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.61 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> GSM358408 </td> <td> Left </td> <td> B </td> <td align="right"> 89.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.57 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> GSM358410 </td> <td> Right </td> <td> B </td> <td align="right"> 86.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.53 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> GSM358434 </td> <td> Right </td> <td> B </td> <td align="right"> 86.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.53 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> GSM358407 </td> <td> Right </td> <td> B </td> <td align="right"> 84.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.50 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> GSM358389 </td> <td> Rectum </td> <td> B </td> <td align="right"> 83.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.49 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> GSM358431 </td> <td> Left </td> <td> B </td> <td align="right"> 83.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.49 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> GSM358444 </td> <td> Rectum </td> <td> B </td> <td align="right"> 82.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.47 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> GSM358446 </td> <td> Right </td> <td> B </td> <td align="right"> 82.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.47 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> GSM358473 </td> <td> Right </td> <td> B </td> <td align="right"> 81.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.46 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> GSM358394 </td> <td> Left </td> <td> B </td> <td align="right"> 80.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.44 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> GSM358455 </td> <td> Right </td> <td> B </td> <td align="right"> 80.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.44 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> GSM358402 </td> <td> Left </td> <td> B </td> <td align="right"> 79.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.43 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> GSM358457 </td> <td> Right </td> <td> B </td> <td align="right"> 79.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.43 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> GSM358395 </td> <td> Right </td> <td> B </td> <td align="right"> 78.00 </td> <td> F </td> <td> N </td> <td> Y </td> <td> 50+ </td> <td> 70+ </td> <td> Y </td> <td> Single </td> <td align="right"> 0.42 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> GSM358454 </td> <td> Right </td> <td> B </td> <td align="right"> 78.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.42 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> GSM358466 </td> <td> Rectum </td> <td> B </td> <td align="right"> 78.00 </td> <td> M </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.42 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> GSM358470 </td> <td> Right </td> <td> B </td> <td align="right"> 78.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.42 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> GSM358471 </td> <td> Colon </td> <td> B </td> <td align="right"> 78.00 </td> <td> F </td> <td> N </td> <td> N </td> <td> 50+ </td> <td> 70+ </td> <td> N </td> <td> None </td> <td align="right"> 0.42 </td> </tr>
   </table>



```R

```
