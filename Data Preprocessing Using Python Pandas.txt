Task 1: Loading Hospitalization Details

import pandas as pd
#--- Read in dataset(hospitalisation_details.csv) ----
hosp_details = pd.read_csv("hospitalisation_details.csv")
hosp_details
--------------------------------------------------------------------------------------------------------------------------------------------
Task 2: Identifying Null Values in Hospitalization Details

null_values = hosp_details.isnull().sum()
null_values
---------------------------------------------------------------------------------------------------------------------------------------------
Task 3: Identifying Data Types in Hospitalization Details

datatype = hosp_details.dtypes
datatype
--------------------------------------------------------------------------------------------------------------------------------------------
Task 4: Identifying Duplicate Data in Hospitalization Details

duplicates = hosp_details.duplicated().sum()
duplicates
---------------------------------------------------------------------------------------------------------------------------------------------
Task 5: Data Preprocessing and Cleaning for Hospitalization Details

hosp_details.drop_duplicates(inplace=True)
columns_to_remove = ["Has_Children", "Is_Frequent_Treatment"]
hosp_details.drop(columns=columns_to_remove, axis=1, inplace=True)
new_columns = {
        'c_id': 'customer_id',
        'yr': 'year',
        'mth': 'month',
        'date?': 'date',
        'children?': 'children',
        'charges?':'charges',
        'host_tier':'hospital_tier',
        'Ct_tier':'city_tier',
        'st_id':'state_id'
    }

hosp_details= hosp_details.rename(columns=new_columns)
hosp_details.to_csv('hospitalisation_details_cleaned.csv', index=False)
hosp_details
----------------------------------------------------------------------------------------------------------------------------------------------
Task 6: Loading Medical Examination Data

med_exam = pd.read_csv("./medical_examinations.csv")
med_exam
------------------------------------------------------------------------------------------------------------------------------------------
Task 7: Identifying Null Values in Medical Examination Data

null_values = med_exam.isnull().sum()
null_values
----------------------------------------------------------------------------------------------------------------------------------------------
Task 8: Identifying Data Types in Medical Examination Data

datatype = med_exam.dtypes
datatype
-------------------------------------------------------------------------------------------------------------------------------------------
Task 9: Identifying Duplicate Data in Medical Examination Data

duplicates = med_exam.duplicated().sum()
duplicates
---------------------------------------------------------------------------------------------------------------------------------------------
Task 10: Data Preprocessing and Cleaning for Medical Examination Data

med_exam .drop_duplicates(inplace=True)
columns_to_remove = ["recovery_period"]
med_exam.drop(columns=columns_to_remove, axis=1, inplace=True)
new_columns = {
        'cid': 'customer_id',
        'b_m_i': 'BMI',
        'h_Issues': 'health_issues',
        'cancer_hist': 'cancer_history',
        'noofmajorsurgeries':'numberofmajorsurgeries',
        'smoker??':'smoker'
    }
med_exam  = med_exam .rename(columns=new_columns)
med_exam .to_csv('medical_examinations_cleaned.csv', index=False)
med_exam

#--- Export the df as "medical_examinations_cleaned.csv" ---
med_exam .to_csv('medical_examinations_cleaned.csv', index=False)
med_exam
---------------------------------------------------------------------------------------------------------------------------------------------
