# EDA_Boston-Crime-Kaggle-Dataset


# PROJECT 2: Crimes in Boston Dataset (from Kaggle)

Datasets 
Below there are some inspirations for each step, but you are free (and advised
to) include new ideas in your work:  
Crimes in Boston  
Link: [Crimes in Boston](https://www.kaggle.com/datasets/AnalyzeBoston/crimes-in-boston)
 
Overview:  
Crime incident reports are provided by Boston Police Department (BPD) to document
the initial details surrounding an incident to which BPD officers respond. This is a dataset
containing records from the new crime incident report system, which includes a reduced
set of fields focused on capturing the type of incident as well as when and where it
occurred.
Additionally, you are  demographic information on the district’s ethnicity, taken from
https://statisticalatlas.com/place/Massachusetts/Boston/Race-and-Ethnicity, where
other demographics can be found as well. The district code names are taken from
https://bpdnews.com/districts .
  
---
## **Pre-processing**

The Crimes in Boston is a database which records incidents reported by the Boston Police Department between June 2015 and September 2018. The record contains incidents of crimes as well as other events such as the recovery of a lost vehicle, medical assistance to a person and missing person reports. We loaded the data into Google Colab because this is an accessible tool and it allowed us to work together. The dataset consists of 319.073 rows and 17 columns. We decided to delete the columns Lat (latitude), Long (Longitude) and Location from the dataset, as location data has already been provided by the DISTRICT, REPORTING_AREA and STREET columns. Moreover, these columns had more missing values than other columns (about 20.000 instances). We also deleted Occured_on_date, because we have all the time data (year, month and day of the week, hour of the incident, etc) in separate columns already.

Further, we processed the data for missing values. There were no instances where the incident number or offense code were missing. However, there were some instances where location data was missing. We decided to delete rows that had missing values for District, Reporting Area and Street, because we would have no location data at all for those instances. In total, we dropped 1.764 rows. For the column Shooting, there were many missing values, or at least empty cells. However, the cells that were not empty all had the value ‘Y’. We therefore made the assumption that non-empty cells, containing ‘Y’, indicate the use of firearms in the particular incidents, while empty cells meant no firearms were used during the incident. We modified this column so ‘Y’ cells were denoted by a ‘1’ and previously empty cells were denoted by a ‘0’. In other columns with missing values, we replaced them with the most common one (in case of UCR_part), or the most common one in that district (for Street). 

Furthermore, we noticed that sometimes, the same incident number occurs more than once in the dataset. However, it seems that in those cases, the offense codes or offense descriptions were different, meaning that the person committed multiple crimes during the same incident. 


Target Variable = OFFENSE_CODE_GROUP

Drop Variables with the same information
- OCCURED_ON_DATE
- Lat
- Long
- Location
- OFFENSE_CODE
- OFFENSE_DESCRIPTION

Encode High-cardinality variables 
- REPORTING_AREA
- STREET

Encode discrete variables into numbers

## Exploratory Analysis
# Statistical Insights
Before we started with the Machine Learning part, we wanted to gather some insights into the data. We plotted the number of incidents per day of the week, which shows that the number of incidents is roughly divided equally across the week, although there seem to be slightly more incidents on Fridays. We also looked at the spread of incidents across a day. As can be seen on Figure 1, the number varies considerably throughout the day, with a peak at 17:00. For this reason, we decided it might be insightful to group the hours of the day into four parts: Night (00:00-05:59), Morning (06:00-11:59), Afternoon (12:00-17:59) and Evening (18:00-23:59). We chose this distribution to obtain bins of equal width. In order to do this, we created a new variable part_of_day. We can conclude that most incidents happen in the afternoon (see Figure 2), and the least incidents happen at night. This might be counterintuitive at first, however we have to remember that the dataset does not only consist of crimes, but also of incidents like Motor Vehicle Accidents, which are probably more likely to happen during commute hours. 

*Figure1*
![Figure1](https://user-images.githubusercontent.com/117380503/225674752-9d258e18-c519-4ac9-b573-5d38300981ab.png)
*Figure2*
![Figure2](https://user-images.githubusercontent.com/117380503/225675117-1f4d75b3-b0ff-49e5-bd9f-504cf1ea7a3d.png)

Furthermore, we looked into the amount of incidents per district. When we do a simple count of the amount of incidents per district, the districts Roxbury, Dorchester and South End make up the top three, as is visible in Figure 3. Of course, it would make sense that more incidents happen in districts with a higher population. Therefore, we compared the amount of incidents in a district with the population of that district. We extracted the population numbers from the Ethnicity_per_Neighborhood.csv file. Figure 4 shows the results; the top three now consists of Downtown & Charlestown, South End and Mattapan. Downtown & Charlestown is the remarkable outlier with almost 2.3 incidents per inhabitant. However, since this is the center of the city, it is not unlikely that this is in fact correct. This could be important information for the Police Department; it shows which district has the highest crime rate per capita and, therefore, might need more police forces. Additionally, we do not immediately see a link to ethnicity here. In the top three districts for total amount of incidents per capita, there are enormous differences in the composition of the communities, so a direct link seems far-fetched. We did not investigate this further as the reasons and mechanisms behind crime occurrence are very broad and there are a myriad of factors that have influence on it. Moreover, most of the events in the dataset are not crimes but other incidents and so linking ethnicity to this is probably not ideal.

*Figure3*
![Figure3](https://user-images.githubusercontent.com/117380503/225675676-79fff794-3c35-4d89-849a-2b10d4d916c5.png)
*Figure4*
![Figure4](https://user-images.githubusercontent.com/117380503/225675690-57b44df7-e268-4921-ad6e-3d75b76a4501.png)

Lastly, we looked at which incidents happened most often in Boston and also what UCR category occurred most often. The most common incident in Boston in this time period was Motor Vehicle Accident Response (so, not a crime), followed by Larceny and Medical Assistance. We also see this reflected in the UCR categories: the respective distributions for Part One, Part Two and Part Three were 61.411, 97.055 and 157.531. There were another 1.221 incidents under the category Other. This is actually a good sign: the most common incidents were in category three, which consists of minor crimes but mostly incidents such as motor vehicle accidents and verbal disputes. Category one, which has major crimes such as homicide, rape, aggravated assault and burglary, is the least common category aside from Other, which mostly consists of the recovery of stolen vehicles and license plates. The conclusion is that most incidents that the Police Department responds to aren’t crimes, which is a good thing, of course. 
