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









