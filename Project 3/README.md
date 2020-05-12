
## PROJECT: SURVIVAL ANALYSIS OF EXPRESSION DATA FROM PRIMARY COLORECTAL CANCERS IN R

### 1. Importing libraries
![1a.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/1a.png)

### 2. Description of data

Samples were taken from colorectal cancers in surgically resected specimens in 290 colorectal cancer patients. The data for which the disease free survival time and censoring information was not available were removed. The final number of patients for which the data was available was 226. Survival analysis was done with this final data.

The variables that were available in the data are:
![2.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/2.png)

#### Data Modification
The below columns were added which are modified from the existing ones:
![3.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/3.png)

First few datas:
![4.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/4.png)

### 3.	Basic descriptive statistics

#### Summary of data
![5.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/5.png)

![6.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/6.png)

#### Event Distribution
![7.png](https://github.com/Jasmy118/MyProjects/blob/master/Project%203/Images%26Files/7.png)

From the summary of the data we find,
- The sample size is 226(sampleID).
• Tumour have 4 locations in this sample and has data of the patients at cancer stage A, B and C (location and dukes_stage).
• Youngest age at which cancer diagnosed in the sample is 26 and oldest is 92(age_diag).
• There are 106 females and 120 males in the data(gender).
• The trial period is 142.55 months or 11.9 years(dfs_time and dfs_timeYears).
• Out of 226 patients, 22 have undergone adjuvant radiation therapy and 87 have undergone adjuvant chemo therapy(adjXRT and adjCTX).
• There are 27 patients in the age group 25 to 50 and 199 above 50(age_group2).
• 22 patients between age 25 and 48. Almost equal number of patients in 49 – 69 and above 70(age_group3).
• 88 patients have undergone atleast one of the adjuvant therapies(adjT2).
• 21 has undergone both adjuvant therapies, 67 has undergone exactly one of the adjuvant therapies and 138 have not undergone any of the adjuvant therapies(adjT3).





