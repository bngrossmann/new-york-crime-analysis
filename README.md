# New York Crime Analysis
## Summary
This project will attempt to build a predictive model for determining the level of a crime for a given type of location.

It will examine the crime distribution in New York City over the course of the year 2021 as reported by location types and the 77 precincts of the NYPD.  The results of this type of analysis can most directly be used to inform decisions about crime prevention strategies, whether that be long term solutions such as community and social programs or more immediate requirements such as additional patrols.


## Data Set
### Source
The data for this project is publicly available information for crimes reported to the New York City Police Department, available through NYC Open Data.

The source page used was:
https://data.cityofnewyork.us/Public-Safety/NYPD-Complaint-Data-Current-Year-To-Date-/5uac-w243

The source is updated quarterly, and the data set used for this project was downloaded on April 8, 2022.

A copy of the "NYPD Complaints Incident Level Data Footnotes" was downloaded on 2022-04-13 and was posted in this repository as "NYPD_Complaint_Incident_Level_Data_Footnotes.pdf".

### Description
The data set contains 36 columns with 449,506 rows. Each row is a unique report made to the NYPD.

The columns can be catagorized into the following groups
* Report Identifier (1 column)
* Location Information (17 columns)
* Times and Dates (5 columns)
* Special Codes (7 columns)
* Suspect and Victim Demographics (6 columns)
#### Keeping Columns
Since there are 36 columns, I will indentify here the columns that will be used in this project.
* Complaint Number (CMPLNT_NUM): The unique report identifier
* Address Precinct Code (ADDR_PCT_CD): The precinct where the crime occured
* Complaint From Date (CMPLNT_FR_DT): The date when the crime began
* Complaint From Time (CMPLNT_FR_TM): The time of day when the crime began
* Law Category Code (LAW_CAT_CD): The severity level of the offense
* Premise Type Description (PREM_TYP_DESC): The type of premise where the offense occured
#### Dropping Columns
Reasons for dropping the remaining columns are as follows:
* Location Information
  * Too general or too specific
  * Redundant information using different coordinate systems
* Times and Dates
  * Times and dates after crimes end or are reported inherently happen after the crime 
* Special Codes
  * Greater details about individual reports
  * Redundant inforamtion by assigning numerical codes to each description
* All suspect and victim demographics
  * Over 40% of the suspect data is unknown
  * Likely entangled with population demographics of precincts
### Creating Columns
There is information that is not explictly presented anywhere, but does exist within the data that is already present. Some of it can be made into their own columns so they can be examined seperately from the data it was hidden within.
* Day of the week
  * This can be extracted from the date
  * This 7 day cycle affects social patterns
## Issues with Data and Resolution Action
* Time value increments too fine
  * Action: Bin times into intervals of 15 minutes
* Time and Date values don't get passed into correlation matrix
  * Action: Change time to hours since beginning of day (0.00, 0.25, ..., 23.50, 23.75)
  * Action: Change date to day since beginning of year (1, 2, ..., 364, 365)

## Exploratory Data Analysis
Examing some of the general relationships of the data, some general characteristics can be seen:

* All severity levels of crime rose throughout the year. But there is a noticable decrease during the final two weeks, which might be attributatable to the winter holiday season.
  ![NY Crime by Day of Year 2021](https://user-images.githubusercontent.com/99386257/165869423-b750a350-7a4e-4cf0-93ea-01824b298d8b.png)

This indicates that the time of year is potentially important for determining when crime prevention efforts should be increased. There is a big caveat, however. The year 2021 was a pandemic year, so the yearly trend in crime may not be typical. A follow-up project should examine other years to make a determination.

* All precincts have similar ratios of crime severities.
  ![NY Crime Ratios in Precincts](https://user-images.githubusercontent.com/99386257/165869907-342d0a33-aef6-4278-aa58-7ff32fef23c9.png)

This would indicate that the division of resources and crime prevention measures might be distributed similarly among precincts. The specifics of what these might be will more likely be dependent on the individual community and zoning characteristcs of the precincts (this is why premise type descriptions are included in this project).

* Majority of crimes happen either in streets or in residences.
  ![NY Crime by Premise Type](https://user-images.githubusercontent.com/99386257/165870003-4be8d00b-5313-49a7-ab29-6bc0a3d90f97.png)

These very different types of settings would require very different approaches, as one type is a very public location and the other type is a very private location. How these are distributed throughout the city may entail different solutions within different precincts.

## Models 
This is a classification analysis with 3 target values:violation, misdemeanor, felony)

The models I will run will be be a Random Forest, a K-Nearest Neighbors, and a Logistic Regression. I will also run a baseline model to serve as a minium standard.

## Model Comparison

The results of the models are seen in the following confusion matrices. The results are normalized so that each row in one matrix has a sum of 1.

![NY Crime Confusion Matrics](https://user-images.githubusercontent.com/99386257/166010110-0a7b61be-d268-4c40-a5e4-45f7e00412db.png)

None of the models perform very well.

Obviously, the Baseline model is not expected to perform well. It predicts everthing should be a misdemeanor. The Linear Regression is barely better. Regardless of what the actual severity level is, the model will misdemeanor over 90% of the time. This is an abject failure for the violation and felony cases.

The Random Forest and K-Nearest Neighbors preformed significantly better, but still predict the severity level as misdemeanor most of the time. Their true positive rates for the violation case is two orders of magnitude better than the Logistic Regression. Their true positive rates for the felony case also improved over the Logistic Regression, with 4.3 times better for the Random Forest and 3.3 times better for the K-Nearest Neighbors.

However, the Random Forest and K-Nearest Neighbors still predict misdeameanor most of the time.

I am choosing best model is the Random Forest as it has the best true positive rates for violations and felonies. Additionally, since felonies are the most dangerous severity level, it may be more import to predict true positives for this case than the other two.

Due to the not stellar performance of any of these models, I am concluding that even the best of them is not particularly useful by itself. Two strong reasons may explain why.
1. The models have insufficient data. There may be other factors that are not included in police reports that may affect criminal behavior. Such factors may include variables such as weather, state of the economy, and distribution of socio-economic classes of the various neighborhoods within a precinct. These types of information would need to be collected from another source.
2. The COVID-19 pandemic years are uniquely different than most years. As social distancing rules, mask mandates, and anxieties about COVID-19 changed throughout the year, there may have been an disruptive effect that lowers the predictability of any models applies to this problem.

There does exist a third explanation, in which there is an inherent limit to how well a model has solved the crime prediction problem. I don't believe the models used here have exhausted enough avenues of improvement to have reached that limit.
