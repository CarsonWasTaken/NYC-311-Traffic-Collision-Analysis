NYC-311-Traffic-Collision-Analysis

Introduction We are addressing the concerns around traffic light complaints in search of a correlation with vehicle accidents around New York City for 2018-2023. From the 311 complaints, we are using the complaint type: Traffic Signal Condition, alongside with dataset: https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9, and Motor Vehicle Collisions in New York City: https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95, both accessed on Oct 10, 2023. This project will assess if traffic light complaints correlate to the number of accidents and if more serious actions must be taken to address these complaints. Within the 311 complaints dataset, each record includes information about the locations (coordinates, zip code), description of the complaint, complaints’ status, dates, and others. In the 311 complaint dataset, for the complaint type: Traffic Signal Condition, there are 572593 records for 2010 to 2023 and for the last five years, 2018-2023, there are 186343 records as of Oct 10, 2023. The dataset Motor Vehicle Collisions- Crashes contains details on the crash event with different fields of the number of people injured or killed; such as; the number of persons injured and killed, coordinates, zip codes, dates, and others for each record. For this dataset, there are 665359 records from 2018 - 2023 as of Oct 10, 2023. Both 311 and Collisions datasets have the same location fields such as zip code, borough, longitude, and latitude, and the same date range of 2018-2023. By integrating those datasets through the records’ zip codes, and the same period using crash_date (Collision dataset) and complaint_created_date (311 Traffic Signal Condition dataset), we can determine an accurate location or proximity of where and when the complaints were made to observe a correlation. We will create two data marts for the two datasets. Frequently when complaints are filed regarding traffic light conditions, a specific location must be provided. Thus, it is within reason that grouping these two within a specific coordinate would be effective. Data Sources: 311 Service Requests from 2010 to Present: https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9 ● Year Range: 2010-2023 ● Records for Complaint Type: Traffic Signal Condition; ● Records as of Oct 10, 2023: 572,593; Attributes: 41 Complaint Type: Traffic Signal Condition: ● Motor Vehicle Collisions - Crashes - Motor Vehicle Collisions - Crashes | NYC Open Data [Link] ○ Citation: New York City. (2023). Motor Vehicle Collisions (Crashes) [Data set]. City of New York Open Data. Retrieved from https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95 ■ Year Range: 2012-2023 ■ Records: 2,032413; Attributes: 29 ■ (Note: contains details on the crash event with the number of people injured or killed)

Dimensional Model 311 Service Requests:

Model of 311 services transactional grain: image
Motor Vehicle Collisions - Crashes:

Model of Motor Vehicle Collisions transaction grain: 

<img width="653" height="442" alt="image" src="https://github.com/user-attachments/assets/e6b1bf30-a56d-40cc-a641-958a574578bd" />

MERGED Models:

<img width="655" height="534" alt="image" src="https://github.com/user-attachments/assets/405ab80a-60e2-4b96-9460-5cb8dc3c319c" />

ETL Process and Code We extract datasets using Python and Socate APIs, build the ETL process using dbt Cloud, and host the Data Warehouse in Google BigQuery.

Data Profiling: Before the ETL process, we used a data profiling process to analyze and explore the datasets which helped us to decide which fields to use for our project 311 data:

<img width="254" height="259" alt="image" src="https://github.com/user-attachments/assets/2f6a54ce-122b-4f78-828d-f415713a7d88" />

25.7% zip code missing in 311 from 2018-2023 for 311 Traffic Signal Condition.

<img width="641" height="312" alt="image" src="https://github.com/user-attachments/assets/d7da3d77-c845-4df9-8cda-c2ffb560a23b" />

[Created date frequency chart for 311 data when the complaint type is Traffic Signal Condition] 

<img width="825" height="335" alt="image" src="https://github.com/user-attachments/assets/36e5b642-2aa2-4cc8-bee2-b21744543e51" />

The top 10 zip code for Traffic Signal Condition Collision Data: 

<img width="818" height="208" alt="image" src="https://github.com/user-attachments/assets/3fd74383-5af3-4c57-90fc-2568dc17b599" />

[Missing values] When the fields are Contributing Factore Vehicle 3 or larger and the Vehicle type Code is 3 and larger, more than 90% of data are missing for those values. This is why we decided to use Only Contributing Factore Vehicles 1 &2 and the Vehicle type Code is 1 & 2 instead of using all fields.

<img width="826" height="392" alt="image" src="https://github.com/user-attachments/assets/e1d3b579-652f-4705-b430-91bc6621544f" />

[Frequency of ZIp codes for collision data]

The most common words for reasons of collisions are unspecific, distractions, and inattention. ETL Process Summary: In this phase, we extracted and loaded the data using Python. We added the API in the Python code shown below (311, Collisions) in which that data was extracted into a CSV file and uploaded into GCS. From there the data is imported into a big query where we could move on towards the Transform process of this milestone. Additionally, the code for transformation was used through DBT to create dimensions for both 311 and collisions. which is also attached below in the appendix section for 311 and Collisions. 311 complaint dimensions and Fact Table: Locations dimensions:

<img width="825" height="360" alt="image" src="https://github.com/user-attachments/assets/3044f7e5-19c6-4b50-99d7-39447bd8fab8" />

Complaint type dimensions:

<img width="826" height="193" alt="image" src="https://github.com/user-attachments/assets/a219f159-6668-41c7-bf45-486a0baf4aa1" />

Status dimensions:

<img width="491" height="169" alt="image" src="https://github.com/user-attachments/assets/61376f12-a20a-414e-bdf2-b6b029e82fee" />

Agency dimensions:

<img width="829" height="121" alt="image" src="https://github.com/user-attachments/assets/c1879cee-aac8-4fe6-b7e5-11dd5850c9a8" />

Complaint Date dimensions:

<img width="825" height="93" alt="image" src="https://github.com/user-attachments/assets/bd5ec4a3-137c-4578-8212-a5df2d80c684" />

Note: This dimension is created from Date view

311 complaint FACT table:

<img width="822" height="214" alt="image" src="https://github.com/user-attachments/assets/09d163ea-1ea9-4e97-8d9c-6f93d7b2e7d2" />

Collisions dimensions and Fact Table: 

<img width="827" height="227" alt="image" src="https://github.com/user-attachments/assets/d74ca82a-05b8-4d01-986d-d5730d0df3ef" />

Collisions Location dimensions: 

<img width="827" height="88" alt="image" src="https://github.com/user-attachments/assets/227ddd6c-9c0a-4892-ab29-f0762700fc5b" />

Collisions time dimensions:

<img width="823" height="89" alt="image" src="https://github.com/user-attachments/assets/b980b5bc-7614-40dd-a834-a01839a454d0" />

Note: this dimension is created from time view

Collisions Date dimensions: 

<img width="824" height="88" alt="image" src="https://github.com/user-attachments/assets/2b83db88-08c5-47bc-bb3f-525e89477a15" />

Note: This dimension is created from Date view

Vehicles dimensions:

<img width="827" height="102" alt="image" src="https://github.com/user-attachments/assets/6fcefd33-e02c-447e-a5cc-3dbf01b093be" />

Collision Fact table:

<img width="827" height="83" alt="image" src="https://github.com/user-attachments/assets/110ea157-8b2a-4963-a4d8-76b7b722e82e" />

Merged location: 

<img width="826" height="126" alt="image" src="https://github.com/user-attachments/assets/b8f22cc1-80f5-479e-bc43-8aa2b4f1bd47" />

Additional Notes: For both datasets, there are no common latitude and longitude, in this case, we joined both datasets with zip codes Merged Dates 

<img width="827" height="86" alt="image" src="https://github.com/user-attachments/assets/65ff70b6-1aa4-4394-878c-dcfa2695b03b" />

Notes: Both datasets are merged by the full date

ETL ( EXTRACT) Completed Code for Collisions Dataset (updated) data_url2='data.cityofnewyork.us' # The Host Name for the API endpoint (the https:// part will be added automatically) data_set2='h9gi-nx95' # The data set at the API endpoint (311 data in this case) app_token2='fRtgN6KKZ5bEIkbEB12tbzK7T' # The app token created in the prior steps client = Socrata(data_url2,app_token2) # Create the client to point to the API endpoint

Set the timeout to 60 seconds
client.timeout = 60

Retrieve the first 2000 results returned as JSON object from the API
The SoDaPy library converts this JSON object to a Python list of dictionaries
where_clause2 = "date_extract_y(crash_date) BETWEEN 2018 AND 2023" results2 = client.get(data_set2, where=where_clause2, limit=100000000)

Convert the list of dictionaries to a Pandas data frame
df2 = pd.DataFrame.from_records(results2)

Save the data frame to a CSV file
df2.to_csv("collisions.csv")

Transform Step 311 complaint dataset 311 complaint location dimension WITH location AS ( SELECT DISTINCT ifnull(borough, City) as borough, ifnull(City, borough) as city, incident_zip, -- ifnull(intersection_street_1,cross_street_1) as intersection_street_1, -- ifnull(intersection_street_2,cross_street_2) as intersection_street_2, TRIM(COALESCE(intersection_street_1, cross_street_1) ||COALESCE(' ') || COALESCE(intersection_street_2, cross_street_2)) AS street_address, longitude, latitude

FROM cis4400project-403800.projectDatasets.311_service_requests) SELECT ROW_NUMBER() OVER () AS location_id_SKs, * -- ,case when cross_street_1 = intersection_street_1 then 1 else 0 end as borough_equal_city FROM location order by location_id_SKs 311 complaint Complaint Type dimension {{ config(materialized="table") }}

with complaint_type as (

   select distinct (complaint_type), descriptor

   from `cis4400project-403800.projectDatasets.311_service_requests`
) select row_number() over () as complaint_type_descriptor_SKs, * from complaint_type order by complaint_type_descriptor_SKs 311 complaint STATUS dimension {{ config(materialized="table") }}

WITH dim_status AS ( SELECT status, DENSE_RANK() OVER () AS Status_SKs FROM cis4400project-403800.projectDatasets.311_service_requests ) SELECT DISTINCT Status_SKs, status FROM dim_status order by Status_SKs 311 complaint Agency dimension {{ config(materialized="table") }} with agencies as ( select distinct agency, agency_name

FROM cis4400project-403800.projectDatasets.311_service_requests

) select row_number() over () as agency_ID_SK,* from agencies

Conformed date dimension: {{ config(materialized="table") }}

WITH date_data AS ( SELECT d, EXTRACT(YEAR FROM d) AS year, EXTRACT(WEEK FROM d) AS year_week, EXTRACT(DAY FROM d) AS year_day, EXTRACT(YEAR FROM d) AS fiscal_year, FORMAT_DATE('%Q', d) AS fiscal_qtr, EXTRACT(MONTH FROM d) AS month, FORMAT_DATE('%B', d) AS month_name, FORMAT_DATE('%w', d) AS week_day, FORMAT_DATE('%A', d) AS day_name, CASE WHEN FORMAT_DATE('%A', d) IN ('Sunday', 'Saturday') THEN 0 ELSE 1 END AS day_is_weekday FROM UNNEST(GENERATE_DATE_ARRAY('2018-01-01', '2024-01-01', INTERVAL 1 DAY)) AS d )

SELECT ROW_NUMBER() OVER() AS date_dim_id, FORMAT_DATE("%Y%m%d", d) AS date_integer, d AS full_date, year, year_week, year_day, month, month_name, week_day, day_name FROM date_data order by date_dim_id

311 complaint Date dimension SELECT distinct date_dim.* FROM cis4400project-403800.projectDatasets.vehicle_collisions left join {{ ref('date_dim') }} as date_dim on DATE(crash_date) = full_date

order by date_dim_id

311 complaint FACT Table {{ config(materialized="table") }} -- list of 311 dimentions: -- 311 dimentions: -- Location -- Complaint type -- status -- agency -- date

-- -- non-null location with all_complaints_data as (select * from {{ ref("All_complaint_data") }}), complaint_location as (select * from {{ ref("311_location") }}), complaint_type as (select * from {{ ref("complaint_type") }}), status as (select * from {{ ref("Status") }}), agecny as (select * from {{ ref("agency") }}), dates as (select * from {{ ref("date_dim") }}),

all_ids as (select complaint_type_descriptor_sks, location_id_sks, agecny.agency_id_sk, status_sks, date_dim_id from all_complaints_data left join complaint_location on all_complaints_data.latitude = complaint_location.latitude or (all_complaints_data.latitude is null and complaint_location.latitude is null) and all_complaints_data.longitude = complaint_location.longitude or (all_complaints_data.longitude is null and complaint_location.longitude is null) and all_complaints_data.borough = complaint_location.borough or (all_complaints_data.borough is null and complaint_location.borough is null) and all_complaints_data.city = complaint_location.city or (all_complaints_data.city is null and complaint_location.city is null) and all_complaints_data.incident_zip = complaint_location.incident_zip or ( all_complaints_data.incident_zip is null and complaint_location.incident_zip is null ) and all_complaints_data.street_address = complaint_location.street_address or ( all_complaints_data.street_address is null and complaint_location.street_address is null ) left join complaint_type on all_complaints_data.complaint_type = complaint_type.complaint_type and all_complaints_data.descriptor = complaint_type.descriptor left join status on all_complaints_data.status = status.status left join dates on all_complaints_data.created_date = dates.full_date left join agecny on all_complaints_data.agency = agecny.agency)

select row_number()over() as Main_Ids_Sks, * from all_ids order by Main_Ids_Sks 311 entire data (It is created to build fact table) {{ config(materialized="table") }}

-- non-null location with complaint_data as ( select unique_key, cast(created_date as date) created_date, cast(closed_date as date) closed_date, agency, agency_name, complaint_type, descriptor, status, incident_zip, ifnull(borough, city) as borough, ifnull(city, borough) as city, TRIM(COALESCE(intersection_street_1, cross_street_1) ||COALESCE('None') || COALESCE(intersection_street_2, cross_street_2)) AS street_address, latitude, longitude from cis4400project-403800.projectDatasets.311_service_requests )

select

ROW_NUMBER() over() as complaint_data_SKs,* from complaint_data where incident_zip is not null Transform Step Collision dimensions and Facts Location Dimension {{ config(materialized="table") }}

WITH location AS ( SELECT DISTINCT latitude, longitude, -- on_street_name, -- off_street_name, -- ifnull(cross_street_name, on_street_name) as cross_street_name, COALESCE(cross_street_name, on_street_name) ||'None' || COALESCE(on_street_name, 'None') || 'None'||COALESCE(off_street_name, 'None') AS street_address, zip_code, borough FROM cis4400project-403800.projectDatasets.vehicle_collisions ) SELECT ROW_NUMBER() OVER ( ) AS collision_location_SK, * FROM location

order by collision_location_SK

Time VIEW {{ config(materialized="table") }}

WITH dim_time AS ( SELECT t, EXTRACT(HOUR FROM t) AS hour, EXTRACT(MINUTE FROM t) AS minute FROM UNNEST(GENERATE_TIMESTAMP_ARRAY('2023-01-01T00:00:00', '2023-01-01T23:59:59', INTERVAL 1 MINUTE)) AS t )

SELECT ROW_NUMBER() OVER () AS hour_id_sk, FORMAT_TIMESTAMP('%H:%M', t) AS real_time, hour, minute, CASE WHEN hour >= 6 AND hour < 12 THEN 'Morning' WHEN hour >= 12 AND hour < 18 THEN 'Afternoon' ELSE 'Night' END AS time_of_day FROM dim_time

order by hour_id_sk Collision Time dimensions {{ config(materialized="table") }} SELECT DISTINCT time_dim.*, crash_time

FROM cis4400project-403800.projectDatasets.vehicle_collisions left join {{ ref('time_dim') }} as time_dim on crash_time = real_time

Date Dimension (created by using date view) SELECT distinct date_dim.*

FROM cis4400project-403800.projectDatasets.311_service_requests left join {{ ref('date_dim') }} as date_dim on DATE(created_date) = full_date

order by date_dim_id

Vehicles Dimension {{ config(materialized="table") }} WITH vehicles AS ( SELECT DISTINCT contributing_factor_vehicle_1, contributing_factor_vehicle_2, -- COALESCE(contributing_factor_vehicle_1,' ') || ' ' || COALESCE(contributing_factor_vehicle_2, ' ') AS final_contributing_factor_vehicle, vehicle_type_code1, vehicle_type_code2, trim (COALESCE(vehicle_type_code1,' ') || ' ' || COALESCE(vehicle_type_code2, ' ') || ' ' || COALESCE(vehicle_type_code2, ' ') || ' ' || COALESCE(vehicle_type_code_3, ' ') || ' ' || COALESCE(vehicle_type_code_4, ' ')) AS final_vehicle_type FROM cis4400project-403800.projectDatasets.vehicle_collisions ) SELECT ROW_NUMBER() OVER () AS vehicle_collision_ID_SK, * FROM vehicles order by vehicle_collision_ID_SK

Collisions Fact {{ config(materialized="table") }}

-- list of collisions dimentions: -- all data -- Location -- time -- vehicles -- date -- -- non-null location WITH all_collision_data AS (SELECT * FROM {{ ref("all_Collisions_data") }}), collisions_location AS (SELECT * FROM {{ ref("collisions_location") }}), types_of_vehicles AS (SELECT * FROM {{ ref("vehicles") }}), time_dim AS (SELECT * FROM {{ ref("time_dim") }}), dates AS (SELECT * FROM {{ ref("date_dim") }}),

all_ids as( SELECT date_dim_id, hour_id_sk, collision_id, collision_location_sk, vehicle_collision_id_sk, number_of_persons_injured, number_of_persons_killed, number_of_pedestrians_injured, number_of_pedestrians_killed, number_of_cyclist_injured, number_of_cyclist_killed, number_of_motorist_injured, number_of_motorist_killed, total_killed, total_harmed_killed_injured FROM all_collision_data LEFT JOIN collisions_location ON ( all_collision_data.latitude = collisions_location.latitude OR (all_collision_data.latitude IS NULL AND collisions_location.latitude IS NULL) ) AND ( all_collision_data.longitude = collisions_location.longitude OR (all_collision_data.longitude IS NULL AND collisions_location.longitude IS NULL) ) AND ( all_collision_data.borough = collisions_location.borough OR (all_collision_data.borough IS NULL AND collisions_location.borough IS NULL) ) AND ( all_collision_data.zip_code = collisions_location.zip_code OR (all_collision_data.zip_code IS NULL AND collisions_location.zip_code IS NULL) ) AND ( all_collision_data.street_address = collisions_location.street_address OR (all_collision_data.street_address IS NULL AND collisions_location.street_address IS NULL) ) LEFT JOIN types_of_vehicles ON ( all_collision_data.final_vehicle_type = types_of_vehicles.final_vehicle_type OR (all_collision_data.final_vehicle_type IS NULL AND types_of_vehicles.final_vehicle_type IS NULL) ) AND ( all_collision_data.contributing_factor_vehicle_1 = types_of_vehicles.contributing_factor_vehicle_1 OR (all_collision_data.contributing_factor_vehicle_1 IS NULL AND types_of_vehicles.contributing_factor_vehicle_1 IS NULL) ) AND ( all_collision_data.contributing_factor_vehicle_2 = types_of_vehicles.contributing_factor_vehicle_2 OR (all_collision_data.contributing_factor_vehicle_2 IS NULL AND types_of_vehicles.contributing_factor_vehicle_2 IS NULL) ) LEFT JOIN time_dim ON all_collision_data.crash_time = time_dim.real_time LEFT JOIN dates ON all_collision_data.crash_date = dates.full_date)

select row_number() over() as main_ids, * from all_ids

Merged Location {{ config(materialized="table") }} WITH collisions_location AS (SELECT * FROM {{ ref("collisions_location") }}), raw_311_location AS (SELECT * FROM {{ ref("311_location") }}), all_location AS ( SELECT -- COALESCE(raw_311_location.street_address, collisions_location.street_address) AS street_address, -- COALESCE(raw_311_location.borough, collisions_location.borough) AS borough, -- COALESCE(raw_311_location.incident_zip, collisions_location.zip_code) AS zip_code, -- COALESCsE(raw_311_location.latitude, collisions_location.latitude) AS latitude, -- COALESCE(raw_311_location.longitude, collisions_location.longitude) AS longitude, collisions_location.latitude AS collision_latitude, collisions_location.longitude AS collision_longitude, TRIM(collisions_location.street_address) AS collisions_street_address, collisions_location.borough AS collisions_borough, collisions_location.zip_code AS collisions_zip_code, raw_311_location.latitude AS complaint_latitude, raw_311_location.longitude AS complaint_longitude, TRIM(raw_311_location.street_address) AS complaint_street_address, raw_311_location.borough AS complaint_borough, raw_311_location.incident_zip AS complaint_zip_code FROM collisions_location FULL JOIN raw_311_location ON ( raw_311_location.incident_zip = collisions_location.zip_code OR ( raw_311_location.incident_zip IS NULL AND collisions_location.zip_code IS NULL ) ) AND COALESCE(raw_311_location.borough, '') = COALESCE(collisions_location.borough, '') where incident_zip is not null and zip_code is not null )

SELECT ROW_NUMBER() OVER () AS locations_sks, -- COUNT(CASE WHEN complaint_zip_code IS -- NULL THEN 1 END) * FROM all_location

ORDER BY locations_sks MERGED Date {{ config(materialized="table") }}

WITH collisions_date AS (SELECT * FROM {{ ref('collisions_date') }}), complaint_date AS (SELECT * FROM {{ ref('complaint_date') }})

SELECT DISTINCT collisions_date.date_dim_id, collisions_date.full_date, collisions_date.year, collisions_date.year_week, collisions_date.year_day, collisions_date.fiscal_year, collisions_date.fiscal_qtr, collisions_date.month, collisions_date.month_name, collisions_date.week_day, collisions_date.day_name, collisions_date.day_is_weekday FROM collisions_date FULL JOIN complaint_date ON collisions_date.full_date = complaint_date.full_date order by date_dim_id

  Finalized Dimensional Schema The following portrays the finalized dimensional schema the business analytics tools are working with. A union was created in Tableau between dim_location and dim_date to create merged_date_location Tableau dimensional schema:

<img width="824" height="219" alt="image" src="https://github.com/user-attachments/assets/5f48e157-ccb3-4051-a03f-f6d03565141a" />

Star Schema model: 

<img width="822" height="406" alt="image" src="https://github.com/user-attachments/assets/f5b44591-8b09-4598-b11e-2573900d9ac1" />

KPI visualizations

<img width="824" height="600" alt="image" src="https://github.com/user-attachments/assets/01d9fd0c-1501-412c-ac60-3ba107e7c592" />

Top 10 Zip Codes With Highest Collision Counts: The top 10 displayed shows that 11385 is the Zip Code with the highest collision accidents which hold true to the heatmap as well and 11385 is notorious for this problem. This finding is consistent with our other visualizations.

Traffic Complaints Number by Month by Year: This chart depicts a time series graph of complaints made per month by year. This is consistent with COVID-19, where no accidents could occur due to the pandemic. Interestingly, there have been fewer complaints after most COVID-19 restrictions were lifted. Collisions Heat Map: The heat map shows a temperature model of collision incidents, where light green is the lowest occurrence and red is the highest.

Count of People Injured by Collisions: The following line shows that in the middle of each year, the line tends to peak which means people are affected the most when it is around the middle of the year. The graph also shows the forecast of the total injured for 2024-25 which will increase than previous years.

Collision Time This line shows that when it is 16:00 or 4 p.m. the collision happens the most (based on 2018-2023). Descriptions of the tools

DBT (Data Build Tool): An open-source analytics engineering tool that is used to transform data in the warehouse more effectively. Python: Python, a high-level programming language, is used to extract datasets, and data profiling by using libraries. Socrata APIs: Socrata is a platform for open data sharing and analysis. Socrata APIs allowed users to access and interact with NYC Open datasets. SQL (Structured Query Language): SQL is a language used for managing relational databases. It allows users to query and update data in a database. SQL is applied when we are transforming the datasets using DBT, Google BigQuery: BigQuery is a serverless data warehouse provided by Google Cloud. It is used to hold the datasets that were extracted from NYC public data. Jupyter Notebook/Google Collab: Jupyter is an open-source web application that allows to creation and sharing of documents containing live code. Narrative Conclusion: A. Software Used: a. DBT: Transformation portion of ETL b. Google BigQuery: Database storage c. Tableau: Dashboard and Business Intelligence d. Google Collab: Extraction and Loading e. Google Cloud Storage: Storing data loaded by Google Collab to load into BQ f. WhatsApp: fast communication between team members
g. Discord : Real-time Live screen sharing for productivity and collaboration.
B. the group’s experience with the project (which steps were the most difficult? Which were the easiest? What did you learn that you did not imagine you would have? If you had to do it all over again, what would you have done differently?) a. The most difficult step was making Google BigQuery accessible to all team members to collaborate because we did not have previous experience with this and it was a challenge to learn and figure out and use DBT to connect it to the Google BigQuery. b. The easiest step was creating the Tableau data visualizations and the dashboard because we had previous experience working with Tableau. c. One thing that we learned that we did not imagine we would have learned is the panda's data profiling tool. We did not know this tool existed until now, and it is very useful. d. If we had to do this all over again, something we would have done differently is take time to slowly develop our database schema so that we wouldn’t run into any problems later on when we try to transform the data into tables and join them together. e. The new proposed benefits can be realized by the new system by going back and readjusting the DBT according to the new model. For instance, we had to convert latitude and longitude for collisions and complaints into dimensions to use in Tableau, however, this process was easy as Tableau allowed that change easily. f. Overall, this project greatly expanded our knowledge of databases, data warehousing, data engineering, and business intelligence tools like Tableau. From creating a dimensional model diagram, creating and executing an ETL process, and connecting the Google BigQuery database to Tableau for data visualization, we were able to understand the process of developing a data warehouse from start to finish.

Reference List https://help.tableau.com/current/pro/desktop/en-us/examples_googlebigquery.htm https://help.tableau.com/current/pro/desktop/en-us/examples_googlebigquery.htm https://colab.research.google.com/notebooks/snippets/gcs.ipynb https://cloud.google.com/bigquery/docs/cloud-storage-transfer https://holowczak.com/getting-started-with-nyc-opendata-and-the-socrata-api/
