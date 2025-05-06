# INTERNATIONAL STUDENTS IN THE U.S.

## Problem Statement and Questions Addressed
The goal of this project is to analyze international student trends in the United States from 2000 to 2022. As international students play a crucial role in shaping university demographics and labor market dynamics, in this study I will answer the following questions:

1.	How does international student enrollment compare to U.S. student enrollment?
2.	From which continents do most international students come from?
3.	Which fields of study are the most common among international students?
4.	Which academic areas are driving OPT (Optional Practical Training) participation?
5.	What hidden trends exist in terms of field growth, volatility, or shifts in student preferences?
6.	What fields of study follow a similar trend pattern?
7.	Which are the msot unstable or volatile fields of study among international students?



## Data Source 🗂️
The dataset used in this analysis was obtained from Kaggle (https://www.kaggle.com/datasets/webdevbadger/international-student-demographics?select=origin.csv), which includes information about international student demographics in the United States. In this Kaggle dataset you can find multiple CSV files covering student academic data, origins, funding sources, and visa status. The original data source is Open Doors (https://opendoorsdata.org/data/international-students/), a project by the Institute of International Education (IIE), which tracks international student trends in the U.S. over time.

The six datasets are:

### - Academic.csv

year: The year. The format is YYYY/YY.

students: The number of students.

us_students: The number of non-international students.

undergraduate: The number of undergraduate students.

graduate: The number of graduate students.

non_degree: The number of non-degree students.

opt: The number of OPT students. OPT stands for Optional Practical Training.


### - Academic_detail.csv

year: The year. The format is YYYY/YY.

academic_type: The academic type. One of ["Undergraduate", "Graduate", "Non-Degree", "OPT"],

academic_level: The academic level. One of ["Associate's", "Bachelor's", "Master's", 'Doctoral',"Professional", "Graduate, Unspecified","Non-Degree, Intensive English", "Non-Degree, Other", "OPT"].

students: The number of students.


### - Field_of_study.csv

year: The year. The format is YYYY/YY.

field_of_study: The field of the study.

major: The major of the study.

students: The number of students.


### - Origin.csv

year: The year. The format is YYYY/YY.

origin_region: The region of origin, such as Asia, Europe, and North America.

origin: The origin, such as Canada, China, and India.

academic_type: The academic type. One of ["Undergraduate", "Graduate", "Non-Degree", "OPT"].

students: The number of students.


### - Source_of_fund.csv

year: The year. The format is YYYY/YY.

academic_type: The academic type. One of ["Undergraduate", "Graduate", "Non-Degree", "OPT"].

source_type: The fund source type. One of ["International", "U.S.", "Other"].

source_of_fund: The source of fund. One of [ "Personal and Family", "Foreign Government or University", "Foreign Private Sponsor", "International Organization", "Current Employment", "U.S. College or University", "U.S. Government", "U.S. Private Sponsor", "Other Sources"].

students: The number of students.


### - Status.csv

year: The year. The format is YYYY/YY.

female: The number of female students.

male: The number of male students.

single: The number of non-married students.

married: The number of married students.

full_time: The number of full-time students.

part_time: The number of part-time students.

visa_f: The number of students with F Visa.

visa_j: The number of students with J Visa.

visa_other: The number of students with other types of Visas.



## Data Cleaning 🧹

For this project, I have used R studio to clean, analyze and visualize the data. These are the steps I followed to clean the data:

- Narrowed the data to only the last century as there were too many missing values in the last century
- Changed the year column form the academic year to only the year
- Removed duplicates
- Get rid of NA values
- After that the data was cleaned, and I didnt have to spend too much time to clean it as the datasets were almost cleaned from 2000 to 2022
```
##Cleaning academic data (changing the academic year to the year)
academic_data_clean <- academic_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())  #Extract start year as integer

any(duplicated(academic_data_clean)) #no duplicates


##Cleaning academic detail data (changing the academic year to the year)
academic_detail_data_clean <- academic_detail_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())

any(duplicated(academic_detail_data_clean)) #no duplicates


##Cleaning field of study data (changing the academic year to the year)
field_of_study_data_clean <- field_of_study_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())

any(duplicated(field_of_study_data_clean)) #no duplicates

field_of_study_data_clean <- field_of_study_data_clean %>%
  filter(!is.na(students)) #removing NA



##Cleaning origin data (changing the academic year to the year)
origin_data_clean <- origin_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())

any(duplicated(field_of_study_data_clean)) #no duplicates


##Cleaning source of fund data (changing the academic year to the year)
source_of_fund_data_clean <- source_of_fund_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())

any(duplicated(source_of_fund_data_clean)) #no duplicates


##Cleaning status data (changing the academic year to the year)
status_data_clean <- status_data %>%
  rename_all(tolower) %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric())

any(duplicated(source_of_fund_data_clean)) #no duplicates
```



## Data Merging 🔗

- Combined academic_data_clean and status_data_clean by year to bring together enrollment data with visa type, gender, and student status (full-time/part-time).

- Grouped and summarized origin_data_clean by year and continent, transforming regional origin data into a higher-level continent view for trend analysis.

- Extracted OPT participation trends from academic_detail_data_clean by filtering for academic_type == "OPT" and grouping by academic level (e.g., Bachelor's, Master's).

- Identified top fields of study across all students using field_of_study_data_clean, grouped by year to observe longitudinal popularity trends.

- Summarized OPT funding sources from source_of_fund_data_clean, highlighting how OPT students support their stay in the U.S. (e.g., through employment, personal funds).
```
#DATA MERGING

##Merge status_data_clean into academic_data_clean by year 
combined_data <- academic_data_clean %>%
  left_join(status_data_clean, by = "year") #This will bring in gender, visa type, and full-time/part-time status.


##Merge in summarized origin_data_clean by year
origin_summary <- origin_data_clean %>%
  mutate(year = str_sub(year, 1, 4) %>% as.numeric()) %>%
  mutate(continent = case_when(
    origin_region %in% c("East Asia", "South and Central Asia", "Southeast Asia", "Central Asia", "Asia") ~ "Asia",
    origin_region %in% c("North Africa", "West Africa", "East Africa", "Central Africa", "Southern Africa", "Africa, Subsaharan", "Africa") ~ "Africa",
    origin_region %in% c("North America") ~ "North America",
    origin_region %in% c("South America") ~ "South America",
    origin_region %in% c("Oceania") ~ "Oceania",
    origin_region %in% c("Europe") ~ "Europe",
    origin_region == "Stateless" ~ "Stateless",
    origin_region %in% c("Middle East", "Mexico and Central America", "Caribbean", "Latin America and Caribbean") ~ "Other",
    TRUE ~ "Other"
  )) %>%
  group_by(year, continent) %>%
  summarise(origin_students = sum(students, na.rm = TRUE)) %>%
  ungroup()  #Continent-level info over time. Let's summarize by origin_region.


##Get OPT data by level from academic_detail_data_clean
opt_by_level <- academic_detail_data_clean %>%
  filter(academic_type == "OPT") %>%
  group_by(year, academic_level) %>%
  summarise(opt_students = sum(students, na.rm = TRUE)) %>%
  ungroup()  #I want to find how many students use their OPT


##Get top OPT fields from field_of_study_data_clean
top_fields_by_year <- field_of_study_data_clean %>%
  group_by(year, field_of_study) %>%
  summarise(total_students = sum(students, na.rm = TRUE)) %>%
  arrange(desc(total_students)) %>%
  ungroup()  #relevant major for ALL students


##Get OPT funding from source_of_fund_data_clean
opt_funding_summary <- source_of_fund_data_clean %>%
  filter(academic_type == "OPT") %>%
  group_by(year, source_of_fund) %>%
  summarise(opt_fund_students = sum(students, na.rm = TRUE)) %>%
  ungroup()
```



