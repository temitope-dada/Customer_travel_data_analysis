Customer Travel Data Analysis
This project focuses on cleaning and merging customer and travel data to create a unified dataset for analysis. The raw data was processed using SQL to address missing values, standardize formats, and combine two related tables.
Dataset
The dataset (customer_travel_data.sql) contains:
**Travel Table**: Includes details about trips, destinations, and policy statuses.
[travel.csv](https://github.com/user-attachments/files/18269515/travel.csv)
 **Customer Table**: Information about customers, including their demographics and occupations.
[Copy of Customer_cleaned_data(1).csv](https://github.com/user-attachments/files/18269514/Copy.of.Customer_cleaned_data.1.csv)

The SQL code used for data cleaning and merging is included in the file [`data_cleaning.sql`](./data_cleaning.sql). It outlines:
- Standardizing gender values.
- Handling missing `HealthID` entries.
- Merging the `Customer` and `Travel` tables into a unified dataset.
- 
--Creating a new database
CREATE Database Sql_projects;
 
 --Changing the database from sql_class_June to Sql_projects
 USE Sql_projects;

--Retreive all information from our customer data table
SELECT
    *
FROM
    Customer_data;

--Retrieve all informatiom from travel table
SELECT
    *
FROM
    Travel;

--Retrieve the healthid column from the customer_data table and the travelid from the travel table
SELECT
    healthid AS Health_and_travel_id
FROM
    Customer_data
UNION ALL
SELECT
    travelid
FROM
    travel;

--Retrieve records about customers travel policy and when it will end
SELECT
    C.CustomerID,
	CONCAT(Title,' ',Firstname,' ', '.',Middleinitial,'.',' ',Surname) AS Full_name,
	C.Comchannel,
	C.TravelID,
	t.PolicyEnd
FROM
    Customer_data AS C
INNER JOIN
    Travel AS t
ON
    C.TravelID = t.TravelID;

--Select records of our customerwith or without travel policy and when their travel ends
SELECT
    C.CustomerID,
	C.Title,
	C.Firstname,
	C.MiddleInitial,
	C.Surname,
	C.Comchannel,
	C.TravelID,
	t.PolicyEnd
FROM
    Customer_data AS C
LEFT JOIN
    Travel AS t
ON
    C.travelID = t.travelID
ORDER BY
    TravelID DESC;

--Case statement
--Changing the null values in the occupation to 'no ocupation'
SELECT
    Occupation,
	CASE WHEN Occupation IS NULL
	THEN 'No occupation'
	ELSE Occupation
	END New_occupation
FROM
    Customer_data;

--Changing the values in the gender column as follows;
--Male should be changed to M and Females should be changed to F
SELECT
    Gender,
	CASE WHEN Gender = 'Male'
	THEN 'M'
	WHEN Gender = 'Female'
	THEN 'F'
	ELSE 'Neither m/f'
	END New_gender_column
FROM
    Customer_data;

--Joining the title, the first name, middle initial and surname column to form full name
SELECT
    Title,
	Firstname,
	Middleinitial,
	Surname,
	CONCAT(Title,' ',Firstname,' ','.',Middleinitial,'.',' ',Surname) AS Full_name
FROM
    Customer_data;
	
--Concat result with speparator
SELECT
    Title,
	Firstname,
	Middleinitial,
	Surname,
	CONCAT_WS (' ',title,firstname,middleinitial,surname) AS Full_name
FROM
    Customer_data;

--Substring
-- Retrive the first letter of our customer's first name
SELECT
    Firstname,
	SUBSTRING(Firstname,1,1) AS First_name_letter
FROM
    Customer_data;

--Confirming that all strings in the middle intial column is one
WITH middle_length_cte AS (
SELECT
    Middleinitial,
	LEN(Middleinitial) AS Middleinitial_length
FROM
    Customer_data
)
SELECT
    *
FROM
   middle_length_cte
WHERE
   Middleinitial_length > 1;

--Changing the values in the gender colomn to capital letters using the upper function
SELECT
    Gender,
	UPPER(Gender) AS CAPITAL_GENDER
FROM
    Customer_data;

--Changing the values in the loction column to small letters using the lower function
SELECT
    Location,
	LOWER (location) AS Lower_location
FROM
    Customer_data;

--Checking my system time
SELECT
    SYSDATETIME() AS system_time;

--Add 2 years to the policy start column 
SELECT
    policystart,
	DATEADD(YY,2,policystart) AS '2_years_added'
FROM
    travel;

--Add I month to the policy start column 
SELECT
    policystart,
	DATEADD(MM,1,policystart) AS '1_month_added'
FROM
    travel;

--Datediff
--Year difference between policystart and policyend
SELECT
    Policystart,
	Policyend,
	DATEDIFF(YY,policystart,policyend) AS Year_difference
FROM
    travel;

--Creating views
CREATE VIEW
    Result_on_customer_travel_policy_end AS 
SELECT
    C.CustomerID,
	CONCAT(Title,' ',Firstname,' ', '.',Middleinitial,'.',' ',Surname) AS Full_name,
	C.Comchannel,
	C.TravelID,
	t.PolicyEnd
FROM
    Customer_data AS C
INNER JOIN
    Travel AS t
ON
    C.TravelID = t.TravelID;

SELECT
    *
FROM
    Result_on_customer_travel_policy_end;
