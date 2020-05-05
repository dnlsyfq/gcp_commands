## Exploring the BigQuery Console

BigQuery is a fully-managed petabyte-scale data warehouse that runs on the Google Cloud Platform. managing and querying data housed in GCP projects.

1.  Navigation menu > BigQuery:
2.  Click on the + ADD DATA link then select Explore public datasets:

GCP Project → bigquery-public-data
Dataset → london_bicycles
Table → cycle_hire

3.  COMPOSE NEW QUERY and RUN 
4.  SAVE RESULTS > CSV
---

## Cloud Storage
1.  Navigation menu > Storage > Browser, and then click Create bucket
2.  Upload files and select the CSV
---


## Cloud SQL

1. Navigation menu > SQL
2. Click Create Instance.
3. From here, you will be prompted to choose a database engine. Select MySQL.
4. Now enter in a name for your instance (like "qwiklabs-demo") and enter in a secure password in the Root password field (remember it!), 
5. then click Create
6. click the Activate Cloud Shell button.
7. Run the following command in Cloud Shell to connect to your SQL instance, replacing qwiklabs-demo if you used a different name for your instance:
```
gcloud sql connect  qwiklabs-demo --user=root

# When prompted, enter the root password you set for the instance.
```
8. Run the following command at the MySQL server prompt to create a database
```
CREATE DATABASE bike;
USE bike;
CREATE TABLE london1 (start_station_name VARCHAR(255), num INT);
USE bike;
CREATE TABLE london2 (end_station_name VARCHAR(255), num INT);
```
9. In your Cloud SQL instance page, click IMPORT
10. In the Cloud Storage file field, click Browse, and then click the arrow opposite your bucket name, and then click start_station_data.csv. Click Select.
11. Select the bike database and type in "london1" as your table.

---

## Cloud SQL for MySQL: Qwik Start

