# Clinical Priority Project

### Clinical_Data_Clean.R
This script reads in the raw data files, cleans the data, and writes cleaned 
dataframe to new csv file.

Currently, the _labs.csv_ raw data has been read and cleaned in this script.
  - labs data was cleaned and filtered by specific lab values
        -"Sodium Level", "Potassium Level", "Chloride Lvl", "CO2 Lvl",
        "Magnesium Serum", "Phosphorus Serum", "Albumin Level", "Glucose Level",
        "BUN", "Creatinine", "Calcium Level, Total", "Calcium Ionized",
        "Lactic Acid Blood, Venous", "Lactic Acid Blood, Arterial",
        "White Blood Cell Count", "Hemoglobin","Platelet Count","Erythrocyte SedRate"
  - _demographics.csv_ data was added to the labs data frame
  - cleaned data was saved as _labs_clean.csv_
  
Also contains function to select random subset of patients by specified size.

Currently, the _labs_clean.csv_ data is used to calculate the acuity score as
well as change-over-time score.

### Clinical_Priority.R
This script calculates _acuity scores_ from patient data and writes a dataframe
with the patients id and acuity score to a new csv file.

##### Acuity score calculated through the algorithm below: 

_ACUITY = abs((RESULT_VALUE - NORMAL_MEAN) / ((NORMAL_HIGH - NORMAL_MEAN) / 2)_

  - RESULT VALUE was value measured
  - NORMAL HIGH was provided in the dataset
  - NORMAL MEAN was calculated by averaging the NORMAL_HIGH and NORMAL_LOW lab values
  
##### Acuity score code followed the procedure below:
  1. Randomly select a point in time per patient
  2. Find closest lab value per lab within a 12 hr window
  3. Calculate NORMAL MEAN by averaging NORMAL HIGH and NORMAL LOW
  4. Calculate acuity score using algorithm above
  5. Acuity scores were summed per patient to obtain one acuity score per patient
  6. Separate column created where acuity score was normalized on a scale from 0-100All acuity scores divided by absolute highest acuity score to obtain normalized acuity scores

### Clinical_Priority_Time.R
This script calculates _change-over-time scores_ from patient data and writes a dataframe with patient ids and change-over-time scores to a new csv file.

###### Change-over-time score calculated through algorithm below:
_CHANGE_OVER_TIME = abs(CHANGE_LAB_NORM / as.numeric(TIME_DIFF))_

  - CHANGE_LAB_NORM = diff(RESULT_VALUE_NORM)
    - RESULT_VALUE_NORM = (RESULT_VALUE - NORMAL_MEAN) / (NORMAL_HIGH - NORMAL_LOW)
        - RESULT_Value is the lab value
  - TIME_DIFF is the differnce in time between the most recent and second most recent lab values

##### Change-over-time score code followed the procedure below:
  1. Randomly select a point in time per patient
  2. Find closest lab value per lab within a 12 hr window
  3. Find the closest previous lab value by date to the randomly selected lab value.
  4. Filter out lab values that only have one measurement
  5. Normalize lab measurements through RESULT_VALUE_NORM calculation above.
  6. Calculate CHANGE_OVER_TIME score by finding the ratio of the difference between normalized lab values and the difference in time measured between the two lab values.

### Clinical_Priority_Visual.R
This script visualizes the relationships between acuity score and chane-over-time score
in various settings.

### Clinical_Priority_PRISM.R
This script visualizes the relationship between clinical acuity scores and the PRISM scoring
system.

### Clinical_Priority_MPEWS.R
This script visualizes the relationship between clinical acuity scores and the MPEWS scoring
system.

