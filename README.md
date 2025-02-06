
# 🏥 Medical Data History

This project revolves around analyzing a healthcare database that includes patients, admissions, doctors, and related information. Through SQL queries, insights into patient demographics, medical records, and hospital operations are extracted. 

The project emphasizes efficient data extraction, manipulation, and insights generation to enhance healthcare administration.


## 🎯Objective of the Project

The primary aim of this project is to demonstrate proficiency in SQL by:

a) Data Retrieval: Extracting specific data from various tables using different SQL clauses (SELECT, WHERE, FROM, JOIN, etc.).

b) Data Manipulation: Transforming data through operations like concatenation, aggregation (SUM, COUNT), and data type conversions.

c) Data Filtering: Applying conditions to filter data based on specific criteria (e.g., gender, weight range, diagnosis).
Data Sorting: Ordering results based on specific columns (e.g., birth date, patient ID, city).

d) Data Aggregation: Grouping data and performing calculations on groups (e.g., counting patients in each city, finding the total number of admissions).


## 📝 Tasks and thier Queries with outputs

1. Show first name, last name, and gender of patients who's gender is 'M'

``` sql
      select first_name,last_name,gender
      from patients
      where gender = "M" ;
```


2. Show first name and last name of patients who does not have allergies.

``` sql
      select first_name, last_name,allergies
      from patients
      where allergies is null or allergies = '' ;
```

3. Show first name of patients that start with the letter 'C'
``` sql
      select first_name
      from patients
      where first_name like "C%";

```
4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)
``` sql
      select first_name, last_name, weight
      from patients
      where weight between 100 and 120  ;
```
5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'
``` sql
     update patients 
     set allergies = "NKA"
     where allergies = '' ;
```
6. Show first name and last name concatenated into one column to show their full name.
``` sql
      select concat (first_name," ", last_name) as patients_full_name
      from patients ;
```
7. Show first name, last name, and the full province name of each patient.
``` sql
     select first_name, last_name, province_name
     from patients as p
     join province_names as pn
     on p.province_id = pn.province_id;
```

8. Show how many patients have a birth_date with 2010 as the birth year.
``` sql
     select count(*) as Patients_birth_year__2010
     from patients
     where birth_date like "2010-%-%" ;

```
9. Show the first_name, last_name, and height of the patient with the greatest height.
``` sql
      select first_name, last_name, height
      from patients
      order by height desc ;
```
10. Show all columns for patients who have one of the following patient_ids: 1,45,534,879,1000
``` sql
     select * 
     from patients
     where patient_id in (1,45,534,879,1000) ;
```
11. Show the total number of admissions
``` sql
      select count(*) as total_admissions
      from admissions ;
```
12. Show all the columns from admissions where the patient was admitted and discharged on the same day.
``` sql
      select * 
      from admissions 
      where admission_date = discharge_date;
```
13. Show the total number of admissions for patient_id 579.
``` sql
      select count(*) as total_admissions_patient_id_579
      from admissions
      where patient_id = 579 ;

```
14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?
``` sql
      select distinct city,province_id
      from patients
      where province_id =  "NS";

```
15. Write a query to find the first_name, last name and birth date of patients who have height more than 160 and weight more than 70
``` sql
      select first_name, last_name, birth_date,height,weight
      from patients 
      where height > 160 and weight > 70 ;
```
16. Show unique birth years from patients and order them by ascending.
``` sql
      select distinct extract(year from birth_date) as unique_birth_year
      from patients 
      order by unique_birth_year asc ;
```
17. Show unique first names from the patients table which only occurs once in the list.
  
For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output. Tip: HAVING clause was added to SQL because the WHERE keyword cannot be used with aggregate functions.
``` sql
      select first_name
      from patients
      group by first_name
      having COUNT(*) = 1
      order by first_name asc;
```
18. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.
``` sql
      select patient_id, first_name
      from patients
      where first_name like "s%____%s"
      order by first_name;
```
19. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.   Primary diagnosis is stored in the admissions table.
``` sql
      select pt.patient_id,pt.first_name,pt.last_name,ad.diagnosis
      from patients as pt 
      inner join admissions as ad
      on pt.patient_id = ad.patient_id 
      where ad.diagnosis = "Dementia" 
      order by ad.diagnosis asc;
```
20. Display every patient's first_name. Order the list by the length of each name and then by alphbetically.
``` sql
      select first_name
      from patients
      order by length(first_name), first_name ;
```
21/22. Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.

``` sql
     SELECT
    (SELECT COUNT(*) FROM patients WHERE gender = 'M') AS           total_male_patients,
    (SELECT COUNT(*) FROM patients WHERE gender = 'F') AS total_female_patients ;
```
23. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
``` sql
      select patient_id, diagnosis 
      from admissions 
      group by patient_id, diagnosis
      having count(diagnosis) > 1
      order by diagnosis;

```
24. Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.
``` sql
     select city,count(*) as total_patients
     from patients
     group by city
     order by total_patients desc, city ;
```
25. Show first name, last name and role of every person that is either patient or doctor.    The roles are either "Patient" or "Doctor".
``` sql
      select first_name, last_name, 'Patient' AS role
      from patients
      union 
      select first_name, last_name, 'Doctor' 
      from doctors;
```

26. Show all allergies ordered by popularity. Remove NULL values from query.
``` sql
    select allergies, COUNT(*) AS allergy_count
    from patients
    where allergies is not null 
    group by allergies
    order by allergy_count desc;
```
27. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.
``` sql
     select first_name, last_name, birth_date
     from patients
     where birth_date BETWEEN '1970' AND '1979'
     order BY birth_date ASC;
```
28. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order    EX: SMITH,jane
``` sql
     select  concat (upper(last_name),',',lower(first_name)) as  patients_fullname
    from patients 
    order by first_name desc;
```
29. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
``` sql
     select province_id, sum(height) as total_height
     from patients
     group by province_id 
     having total_height >= 7000 ;

```
30. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
``` sql
      select last_name,max(weight) - min(weight) as difference_between_weights
      from patients
      where last_name = "Maroni";
```
31. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
``` sql
      select day(admission_date) as days, count(*) as total_admissions
      from admissions
      group by day(admission_date)
      order by total_admissions desc;
```
32. Show all of the patients grouped into weight groups. Show the total amount of patients in each weight group. Order the list by the weight group decending. e.g. if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.
``` sql
     select 
    case 
        when weight between 1  and 50 then '1 - 50'
        when weight between 51 and 100 then '50 - 100'
        when weight between 101 and 150 then '101 - 150'
        end as weight_group,
    count(*) as total_patients
    from patients
    group by weight_group
    order by weight_group desc;
```
33. Show patient_id, weight, height, isObese from the patients table. Display isObese as a boolean 0 or 1. Obese is defined as weight(kg)/(height(m). Weight is in units kg. Height is in units cm.
``` sql
      select patient_id, 
             weight, height, round(weight/height * 100,2) as BMI,
             case 
                when round(weight/height * 100,2)  > 30 then "1"
                else "0"
                end as isObese
      from patients ;
```
34. Show patient_id, first_name, last_name, and attending doctor's specialty. Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'. Check patients, admissions, and doctors tables for required information.
``` sql
    select 
      p.patient_id, p.first_name, p.last_name,
      a.doctor_id,
      d.specialty
    from  admissions as a
    join doctors as d on a.doctor_id = d.doctor_id
    join patients as p on p.patient_id = a.patient_id ;
```
35. All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

   The password must be the following, in order:
    - patient_id
    - the numerical length of patient's last_name
    - year of patient's birth_date
``` sql
       select patient_id , concat(patient_id,length(last_name),extract(year from birth_date)) as "password" from patients ;

```

