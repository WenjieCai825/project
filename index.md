# Project
In this project, we are going to import dataset from NHANES website, manage and analyze the data. I will update the project.do as the project goes on. My colleagues could contribute to the project by adjusting the project.do code and see every change we made on it from the github commit statements record. 

### 1. Download and edit the ```Stata_ReadInProgramALlSurveys.do``` provided by NHANES. Upload the edited do file to the github. This do file imports and prepare the mortality data which further analysis would be based on. 
  ```stata
cat https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/Stata_ReadInProgramAllSurveys.do   
```
Refer to the followup.do to see edits. 

### 2. Link the do file in step 1 
  ```stata
global repo "https://github.com/WenjieCai825/project/blob/main/"
do ${repo}followup.do
save followup, replace
```
### 3. merge with DEMO data
   ```stata
import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT", clear
merge 1:1 seqn using followup
lookfor follow
```

### 4. Prepare the key Parameters for Week 7s Analysis
   ```stata
lookfor mortstat permth_int eligstat 
keep if eligstat==1
capture g years=permth_int/12
codebook mortstat
stset years, fail(mortstat)
sts graph, fail
save demo_mortality, replace 
import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.XPT", clear 
merge 1:1 seqn using demo_mortality, nogen
sts graph, by(huq010) fail
stcox i.huq010
```
Documentation for HUQ dataset: https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.htm

<span style="color:lightblue;">Stay tuned!</span>
