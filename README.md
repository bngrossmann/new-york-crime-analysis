# New York Crime Analysis
This analysis will examine the crime distribution in New York City over the course of the year 2021.

The data for this project is available through NYC Open Data.

The source page used was: https://data.cityofnewyork.us/Public-Safety/NYPD-Complaint-Data-Current-Year-To-Date-/5uac-w243

A copy of the "NYPD Complaints Incident Level Data Footnotes" was downloaded on 2022-04-13 and was posted here as "NYPD_Complaint_Incident_Level_Data_Footnotes.pdf"


## Sections within the Notebook
1. Load Data
2. Uniqueness and Duplication
3. Eliminating Unnecessary Columns
4. Data Types and Consistency
5. Univariant Visuals
6. Correlation

## Issues to Resolve
* Time value increments too fine
  * Action: Bin times into intervals of 15 minutes
* Time and Date values don't get passed into correlation matrix
  * Action: Change time to hours since beginning of day (0.00, 0.25, ..., 23.50, 23.75)
  * Action: Change date to day since beginning of year (1, 2, ..., 364, 365)
