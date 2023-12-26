The code was removed from the public domain following its adoption into 
a proprietary ETL toolkit.

The documentation has been left for users to explore and discuss.

# HCRIS-Documentation

Hospitals submit annual worksheets detailing their FY cost center data. 
CMS then tabulates this data into HCRIS. Each worksheet comprises lined 
items, and each line comprises multiple columns. Each column corresponds 
to a response that is alphanumeric or purely numeric. The worksheet 
responses are tabulated and distributed among three raw data sets. It may 
be helpful to think of the data set as coordinate data.

Headers (or column names) can be found in the data dictionary or 
readme.text. The CMS readme.txt states that Worksheet Code, Line Number, 
Column Number contain trailing or leading zeroes and must not be trimmed.

The following imports the raw data into data frames, then merges the data 
frames into a combined table. Additionally, facility details were pulled 
from CMS and merged. From there, we can further prepare the data for 
export.

These scripts will accept raw CMS Cost Report data from the HCRIS 
(Healthcare Cost Report Information System). They produce a dataframe 
combining the data, and hospital identifying information from CMS. 
Functions have been provided to count or sort the dataframe. 

Please download necessary HOSPITAL-2010 data sets from CMS HCRIS 
(https://www.cms.gov/Research-Statistics-Data-and-Systems/Downloadable-Public-Use-Files/Cost-Reports/Cost-Reports-by-Fiscal-Year). 
Extract files into your working directory, then set the working directory path in both scripts.

NOTE: Resulting data will be large (>3GB). Ensure you have adequate 
storage space and memory to accomodate these efforts. Expect 2-3GB 
required for each additional dataframe generated.

---

# Answers to Specific Questions

#### How many hospitals are in the dataset?
    5304 unique Provider Number identifiers (PRVDR_NUM)
    0 invalid entries (NaN)
#### How many reports were filed in 2021?
    5366 unique reports (RPT_REC_NUM)
    0 invalid entries (NaN)
#### Why are there more reports than hospitals?
    An individual hospital must resubmit a report if the FY changes or CHange of OWnership (CHOW).
    Reports may also be submitted in error. CMS tries to eliminate such additional reports (per readme.txt).
#### What are the Top 10 hospitals by number of beds in 2021?
    AdventHealth Orlando (Orlando, FL)              2791
    New York-Presbyterian Hospital (New York, NY)   2334
    Orlando Health (Orlando, FL)                    1600
    Jackson Health System (Miami, FL)               1504
    Norton Hospitals, Inc (Louisville, KY)          1465
    Baptist Medical Center (San Antonio, TX)        1451
    Montefiore Medical Center (Bronx, NY)           1426
    St Josephs Hospital (Tampa, FL)                 1382
    Cleveland Clinic (Cleveland, OH)                1326
    Yale-New Haven Hospital (New Haven, CT)         1306
#### Which hospital employs the most nurses on an FTE basis
    West River Regional Medical Center-CAH (Hettinger, ND)  3601
    
---

# Scaling up this solution

Annually, data will be pulled from HCRIS and imported for cleaning and 
analysis.

Header info will not change until form 2552-10 is discontinued. New header 
info can be extracted with regex from the SQL description text. Further, new 
accompanying documentation can be compared iteratively, line-by-line to 
generate preliminary changelog texts. An initial scan of this rough text 
provides insight into potential accomodations to investigate. Further 
documentation from CMS on major updates should be considered.

I would need to investigate ways to reduce the size of the combined data set. 
I have removed NaN values from the NMRC set in the code; and created a 
dataframe from the combined_data ALPHNMRC_ITM_TXT values to filter out white 
space and empty strings but could find no such values. I would estimate a 
decade-long dataset to require 35GB per decade. Earlier years will have less 
data while future years will have more than that estimate.

This kind of reporting could be applied to other forms reported to CMS. If 
different forms use different data structures, we will need to refer to the 
documentation for the model and header info.

Additionally, these responses are not correlated with questions making the 
data inaccessible to less technical users. One alternative would be to extract form data and create a new table containing the full text of the question 
found at a worksheet/line/column coordinate. However, forms would need to be 
scraped and compared, as question/response wording and coordinates may vary 
year to year.
