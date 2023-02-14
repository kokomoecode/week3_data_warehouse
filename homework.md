Week 3 Homework

Important Note:

You can load the data however you would like, but keep the files in .GZ Format. If you are using orchestration such as Airflow or Prefect do not load the data into Big Query using the orchestrator.

Stop with loading the files into a bucket.

NOTE: You can use the CSV option for the GZ files when creating an External Table

SETUP:
Create an external table using the fhv 2019 data.
Create a table in BQ using the fhv 2019 data (do not partition or cluster this table).
Data can be found here: https://github.com/DataTalksClub/nyc-tlc-data/releases/tag/fhv

https://github.com/DataTalksClub/nyc-tlc-data/releases/download/fhv/fhv_tripdata_2019-02.csv.gz





CREATE OR REPLACE EXTERNAL TABLE `focus-electron-375717.dezoomcamp.fhv2019`
OPTIONS (
  format = 'CSV',
  uris = ['gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-01.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-02.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-03.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-04.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-05.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-06.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-07.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-08.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-09.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-10.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-11.csv.gz',
  'gs://dtc_data_lake_focus-electron-375717/data/fhv/fhv_tripdata_2019-12.csv.gz']
);
Cannot read and write in different locations: source: europe-west6, destination: US

Question 1:

What is the count for fhv vehicle records for year 2019?

65,623,481
43,244,696 - ANSWER
22,978,333
13,942,414
Question 2:

Write a query to count the distinct number of affiliated_base_number for the entire dataset on both the tables.

external - 2.99GB

bgtable - 318MB

maybe 2 is the answer


What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

25.2 MB for the External Table and 100.87MB for the BQ Table
225.82 MB for the External Table and 47.60MB for the BQ Table
0 MB for the External Table and 0MB for the BQ Table
0 MB for the External Table and 317.94MB for the BQ Table
Question 3:

How many records have both a blank (null) PUlocationID and DOlocationID in the entire dataset?

SELECT COUNT(*) FROM `focus-electron-375717.zoomcampeuwest6.bgtable_fhv2019` WHERE PUlocationID IS NULL 
PUlocationID 2024066

SELECT COUNT(*) FROM `focus-electron-375717.zoomcampeuwest6.bgtable_fhv2019` WHERE DOlocationID IS NULL 
DOlocationID 723689

717,748
1,215,687
5
20,332
Question 4:

What is the best strategy to optimize the table if query always filter by pickup_datetime and order by affiliated_base_number?

Cluster on pickup_datetime Cluster on affiliated_base_number
Partition by pickup_datetime Cluster on affiliated_base_number - Answer
Partition by pickup_datetime Partition by affiliated_base_number
Partition by affiliated_base_number Cluster on pickup_datetime
Question 5:

Implement the optimized solution you chose for question 4. Write a query to retrieve the distinct affiliated_base_number between pickup_datetime 2019/03/01 and 2019/03/31 (inclusive).
Use the BQ table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 4 and note the estimated bytes processed. What are these values? Choose the answer which most closely matches.

12.82 MB for non-partitioned table and 647.87 MB for the partitioned table
647.87 MB for non-partitioned table and 23.06 MB for the partitioned table :ANSWER
582.63 MB for non-partitioned table and 0 MB for the partitioned table
646.25 MB for non-partitioned table and 646.25 MB for the partitioned table
Question 6:

Where is the data stored in the External Table you created?

Big Query 
GCP Bucket - ANSWER
Container Registry
Big Table
Question 7:

It is best practice in Big Query to always cluster your data:

True
False ANSWER
(Not required) Question 8:

A better format to store these files may be parquet. Create a data pipeline to download the gzip files and convert them into parquet. Upload the files to your GCP Bucket and create an External and BQ Table.

Note: Column types for all files used in an External Table must have the same datatype. While an External Table may be created and shown in the side panel in Big Query, this will need to be validated by running a count query on the External Table to check if any errors occur.