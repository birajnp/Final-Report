Dataset Used: DHS program Nepal Dataset 2022
it can be accessed by dhsprogram.com (to access the dataset submit the team names, affiliation and defined proposal)
Please read DHS Program Recode manual and Reports as well as watch videos to understand data proporly before proceding.
the dataset can only be shared among the team members
Main File Used: All respondent's files and women's Files.
Contact Person: Biraj Neupane, birajn2@illinois.edu.

all_data_labels.csv file contains data labels for All respondent's Files.
womens_data_labels.csv file contains data labels for All respondents.

After accessing the data Please use the R code and you will be able to get the data and variables as below.
Please Use NDHS2022 Nepal Version 8 data files.

Here is a table with the extracted variables and their descriptions:

| Variable | Description |
|----------|-------------------|
| Caseid | Case identifier |
| v024 | Province of residence |
| v025 | Type of place of residence |
| v040 | Cluster altitude in meters |
| hv270 | Wealth index combined |
| hv271 | Wealth index factor score combined (5 decimals) |
| hv270a | Wealth index for urban/rural |
| hv271a | Wealth index factor score for urban/rural (5 decimals) |
| shecoreg | Ecological region |
| shdist | District |
| hv104 | Sex of household member |
| hv107 | Highest year of education completed |
| hv108 | Education completed in single years |
| hv115 | Current marital status |
| ha2 | Woman's weight in kilograms (1 decimal) |
| ha3 | Woman's height in centimeters (1 decimal) |
| ha35 | Smoking (cigarettes in last 24 hours) |
| wbp9 | Women's blood pressure first systolic reading |
| wbp10 | Women's blood pressure first diastolic reading |
| wbp24 | Women's blood pressure final systolic reading |
| wbp25 | Women's blood pressure final diastolic reading |
| wbp26 | Women's blood pressure category |
| wbp18 | Has a doctor/health worker prescribed medication to control blood pressure |
| wbp19 | Is respondent taking medication to control blood pressure |
| ha40 | Body mass index/100 (decoded value present in cleaned dataset) |
| v005 | Women's individual sample weight (6 decimals) |
| v012 | Respondent's current age |
| v501 | Current marital status |
| v106 | Highest educational level |
| v130 | Religion |
| v131 | Ethnicity |
| v437 | Respondent's weight in kilograms (1 decimal) |
| v438 | Respondent's height in centimeters (1 decimal) |
| v445 | Body mass index |
| v463a | Smokes cigarettes |
| v463b | Smokes pipe full of tobacco/sulpha/chilim |
| v463aa | Frequency smokes cigarettes |
| v463ab | Frequency currently uses other type of tobacco |
| v464 | Number of cigarettes in last 24 hours |
| v481 | Covered by health insurance |
| v512 | Years since first cohabitation |
| secoreg | Ecological region |
| sdist | District |


These values were repeated so only one values are kept in final csv file
1.	"v024" (Province of residence) - Repeated twice
2.	"v025" (Type of place of residence) - Repeated twice
3.	"v040" (Cluster altitude in meters) - Repeated twice
4.	"hv107" (Highest year of education) - Repeated twice
5.	"secoreg" (Ecological region) - Repeated twice
6.	"shdist" (District) - Repeated twice

  

1661 rows dropped as v012 “Age” were less than 18 
There were 9,670 rows with missing data for the following blood pressure variables:

- `wbp9`: Women's blood pressure first systolic reading
- `wbp10`: Women's blood pressure first diastolic reading
- `wbp24`: Women's blood pressure final systolic reading
- `wbp25`: Women's blood pressure final diastolic reading
- `wbp26`: Women's blood pressure category
These rows have been dropped due to the missing data in these variables.


For v512,#years since first cohabitation NA data replaced with 0
For v464, # the number of cigarettes in the last 24 hours NA data replaced with 0
v107, #highest year of education NA replaced with 0
wbp18, #has a doctor/health worker prescribed medication to control blood pressure NA replaced with 0
wbp19, is the respondent taken medication to control blood pressure NA replaced with 0
v107, #highest year of education 

Final dataset = merged_womens.csv It has 3516 rows with   42 variables mentioned above
few of the rows have missing values so when you omit the data in R or any other statistical program it will be 3513 rows with 42 variables


R codes and Packages Used

setwd("C:/Users/Biraj Neupane -IARTP/Desktop/Clean Data")
library(tidyverse)
library(knitr)
library(mlr3)
library(readr)
library(dplyr)
library(haven)
library(gtsummary)
library(foreign)
library(survey)
library(labelled) 
library(forcats)



# # All dataset (for whole survey)
all <- read_dta("NPPR82DT/NPPR82FL.DTA")
write.csv(t(data.frame(var_label(all))), "all_data_labels.csv", na="")


### womens dataset from survey
women<- read_dta("NPIR82DT/NPIR82FL.DTA")
write.csv(t(data.frame(var_label(women))), "women_data_labels.csv", na="")


## selecting variables from main womens file

Selected_women <-women|> 
  select(
    caseid, #case identification
    v001,	#cluster number
    v002, #	household number
    v003, #	respondent's line number
    v004, #	ultimate area unit
    v005, #	women's individual sample weight (6 decimals)
    v012, #	respondent's current age
    v501,	#current marital status
    
    v024,	# province of residence
    v025,	#type of place of residence
    v040,	#cluster altitude in meters
    v106, #	highest educational level
    v107, #	highest year of education
    v130,	#religion
    v131,	#ethnicity
    v437,	#respondent's weight in kilograms (1 decimal)
    v438,	#respondent's height in centimeters (1 decimal)
    v445,	#body mass index
    v463a,	#smokes cigarettes
    v463b,	#smokes pipe full of tobacco/sulpha/chilim
    v463aa, #frequency smokes cigarettes
    v463ab, #	frequency currently uses other type of tobacco
    v464,	#number of cigarettes in last 24 hours
    v481,	#covered by health insurance
    v512,	#years since first cohabitation
    secoreg,	#ecological region
    sdist)	#district)
    


# selecting variables from main all file   
Selected_All <-all|> 
  select(
    hhid,#	case identification
    hvidx,#	line number
    hv001,#	cluster number
    hv002,#	household number
    hv003,#	respondent's line number (answering household questionnaire)
    hv004,#	ultimate area unit
    hv005,#	household sample weight (6 decimals)
    hv024, #	province
    hv025, #	type of place of residence
    hv040, #	cluster altitude in meters
    hv270,	#wealth index combined
    hv271,	#wealth index factor score combined (5 decimals)
    hv270a, #	wealth index for urban/rural
    hv271a, #	wealth index factor score for urban/rural (5 decimals)
    shecoreg, #	ecological region
    shdist, #	district
    hv104, #	sex of household member
    hv107,	#highest year of education completed
    hv108,	#education completed in single years
    hv115,	#current marital status
    ha2, #	woman's weight in kilograms (1 decimal)
    ha3, #	woman's height in centimeters (1 decimal)
    ha35, #	smoking (cigarettes in last 24 hours)
    
    
    # Dependent variables (BP Women)
    # women blood pressure first systolic reading
    wbp9,
    #women blood pressure first diastolic reading
    wbp10,
    
    #women blood pressure Final systolic reading
    wbp24,
    #women blood pressure Final diastolic reading
    wbp25,
    # women blood pressure category
    wbp26,
   
    wbp18, #	has a doctor/health worker prescribed medication to control blood pressure
    wbp19, #	is respondent taken medication to control blood pressure
    ha40) #	body mass index


# Renaming columns in the selected variable
#in household(all) data to match the women's data
# guideline by DHS program was used to merge it
Selected_All <- Selected_All %>%
  rename(v001 = hv001, v002 = hv002, v003 = hvidx)

# Convert identifiers to the same data type and handle case sensitivity
Selected_women$v001 <- as.character(Selected_women$v001,
                                    Selected_women$v002,
                                    Selected_women$v003)

Selected_All$hv001 <- as.character(Selected_All$v001,
                                   Selected_All$v002,
                                   Selected_All$v003)


# matching columns to merge data #sorting both dataset

Selected_All <- Selected_All %>% arrange(v001, v002, v003)
Selected_women <- Selected_women %>% arrange(v001, v002, v003)


#merging dataset

merged_data_Final <- merge(Selected_All, Selected_women, by = c("v001", "v002", "v003"), all.y = TRUE)


#write in csv
write.csv(merged_data_Final, "merged_women.csv")



