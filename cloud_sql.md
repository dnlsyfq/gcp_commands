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

* Cloud SQL instance
1.  Navigation menu > SQL
2.  Create Instance.
3.  Click MySQL, then Next.
4.  Click Choose Second Generation.
5.  Enter myinstance for Instance ID.
```
gcloud sql connect myinstance --user=root
CREATE DATABASE guestbook;
USE guestbook;
CREATE TABLE entries (guestName VARCHAR(255), content VARCHAR(255),
    entryID INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(entryID));
    INSERT INTO entries (guestName, content) values ("first guest", "I got here!");
INSERT INTO entries (guestName, content) values ("second guest", "Me too!");
```

---

## Loading Data into Google Cloud SQL


*   list the active account
```
gcloud auth list
```
*   list the project ID
```
gcloud config list project
```

* Clone the Data Science on GCP Repository
```
git clone \
   https://github.com/GoogleCloudPlatform/data-science-on-gcp/
   
cd data-science-on-gcp/03_sqlstudio

# Create environment variables that will be used later in the lab for your project ID and the storage bucket that will contain your data

export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=${PROJECT_ID}-ml
```
## Create a Cloud SQL instance

*   to create a Cloud SQL instance
```
gcloud sql instances create flights \
    --tier=db-n1-standard-1 --activation-policy=ALWAYS
```
*   Set a root password for the Cloud SQL instance
```
gcloud sql users set-password root --host % --instance flights \
 --password Passw0rd
```

*   create an environment variable with the IP address of the Cloud Shell
```
export ADDRESS=$(wget -qO - http://ipecho.net/plain)/32
```

*   Whitelist the Cloud Shell instance for management access to your SQL instance
```
gcloud sql instances patch flights --authorized-networks $ADDRESS
```

*   Get the IP address of your Cloud SQL instance
```
MYSQLIP=$(gcloud sql instances describe \
flights --format="value(ipAddresses.ipAddress)")
```

*   Check the variable MYSQLIP:
```
echo $MYSQLIP
```

* create table from sql file 
```
mysql --host=$MYSQLIP --user=root \
      --password --verbose < create_table.sql
```

* connect to mysql
```
mysql --host=$MYSQLIP --user=root  --password
```

##  add csv data to sql instance 
Ingesting Data into the Cloud using Google App Engine lab. For this lab, they have been provided. You'll only be importing two months of data,
```
counter=0
for FILE in 201501.csv 201502.csv; do
   gsutil cp gs://$BUCKET/flights/raw/$FILE \
             flights.csv-${counter}
   counter=$((counter+1))
done
```

* Import the CSV file data into Cloud SQL using mysql
```
mysqlimport --local --host=$MYSQLIP --user=root --password \
--ignore-lines=1 --fields-terminated-by=',' bts flights.csv-*
```
*   Connect to the mysql interactive console
```
mysql --host=$MYSQLIP --user=root  --password
```

## Build the initial data model
```
use bts;
select DISTINCT(FL_DATE) from flights;
select DISTINCT(CARRIER) from flights;
SET @ARR_DELAY_THRESH = 15;
SET @DEP_DELAY_THRESH = 10;
# Correct - true negative
select count(dest) from flights where arr_delay < @ARR_DELAY_THRESH and dep_delay < @DEP_DELAY_THRESH;
# False negative
select count(dest) from flights where arr_delay >= @ARR_DELAY_THRESH and dep_delay < @DEP_DELAY_THRESH;
# False positive
select count(dest) from flights where arr_delay < @ARR_DELAY_THRESH and dep_delay >= @DEP_DELAY_THRESH;
# True positive
select count(dest) from flights where arr_delay >= @ARR_DELAY_THRESH and dep_delay >= @DEP_DELAY_THRESH;
```
---

## Create a Cloud SQL Instance Using Deployment Manager


Deployment Manager is an infrastructure deployment service that automates the creation and management of your Google Cloud Platform resources for you. You can create flexible templates that deploy a variety of Cloud Platform services, such as Google Cloud Storage, Google Compute Engine, and Google Cloud SQL.

Cloud SQL is a fully-managed database service that makes it easy to set up, maintain, manage, and administer your relational PostgreSQL and MySQL databases in the cloud. Cloud SQL offers high performance, scalability, and convenience. Hosted on Google Cloud Platform, Cloud SQL provides a database infrastructure for applications running anywhere.

*   Clone the DM template
fetch the deployment manager template that will deploy SQL instance
```
gsutil cp -r gs://spls/gsp148/create-sql-instance.jinja .
```

*   Modify the DM template
edit the create-sql-instance.jinja file

*   Deploy your configuration
> Create the initial deployment by running the following in Cloud Shell, adding a name for the deployment by replacing [deployment_name]:
```
gcloud deployment-manager deployments create [deployment_name] --template create-sql-instance.jinja
```

* list the deployments:
```
gcloud deployment-manager deployments list
```

---

# Using Ruby on Rails with Cloud SQL for PostgreSQL

Google Cloud SQL for PostgreSQL is a fully-managed database service that makes it easy to set up, maintain, manage, and administer your PostgreSQL relational databases on Google Cloud Platform.

Google App Engine Flexible environment applications are easy to create, maintain, and scale as your traffic and data storage changes. With App Engine, there are no servers to maintain. You simply upload your application and it's ready to go.

App Engine applications automatically scale based on incoming traffic. Load balancing, microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, search, versioning, roll out and roll backs, and security scanning are all supported natively and are highly customizable.

Google Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud

## Create a PostgreSQL Cloud SQL instance

* create a PostgreSQL instance named postgres-instance
```
gcloud sql instances create postgres-instance \
    --database-version POSTGRES_9_6 \
    --tier db-g1-small
```

* Set the password for the postgres user
```
gcloud sql users set-password postgres --host=% \
    --instance postgres-instance \
    --password [PASSWORD]
```

## Set up the Cloud SQL Proxy
When deployed, your application uses the Cloud SQL Proxy built into the App Engine environment to communicate with your Cloud SQL instance. However, to test your application using the Cloud Shell, you must install a copy of the Cloud SQL Proxy in the development environment.

*   Download the Cloud SQL Proxy:
```
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64
```

*   Rename the proxy to use the standard filename:
```
mv cloud_sql_proxy.linux.amd64 cloud_sql_proxy
```

*   Make the proxy executable:
```
chmod +x cloud_sql_proxy
```
*   Create a directory where the proxy sockets will live:
```
sudo mkdir /cloudsql
sudo chmod 0777 /cloudsql
```

> The proxy is now ready to be used. Next, you will obtain the instance connection name for the Cloud SQL for PostgreSQL instance and use it to connect the cloud_sql_proxy to the instance you created in the previous step. The instance connection name is connectionName.

*   Retrieve the instance connection name by running:
```
# Copy the connectionName output
gcloud sql instances describe postgres-instance | grep connectionName
```

*   Run the following to start the Cloud SQL Proxy, and replace [YOUR_INSTANCE_CONNECTION_NAME] with the connectionName value
```
./cloud_sql_proxy -dir=/cloudsql \
                  -instances="[YOUR_INSTANCE_CONNECTION_NAME]"
```

## Ruby on Rails

* install 
```
gem install rails
rails --version
```

* Create a Ruby on Rails app
```
rails new app_name

cd app_name
```

*   Test the Generated Rails Application
Now that we have a Rails app, let's test it using the Web Preview function provided by the Cloud Shell environment. By default the Web Preview uses port 8080
```
# Start the Rails app server listening on port 8080

bundle exec rails server --port 8080

# click on the Web Preview icon in the Cloud Shell toolbar and choose Preview on port 8080
```

## Set up Rails app with Cloud SQL for PostgreSQL

* add the pg gem to the file Gemfile to begin using Google Cloud SQL for PostgreSQL.
```
# Add pg gem in Gemfile
bundle add pg

# update installed dependencies
bundle install
```

* Configure your Rails app to communicate with Cloud SQL for PostgreSQL. Open the file config/database.yml
* Modify the database.yml production database configuration by adding the following and replacing [PASSWORD] and [YOUR_INSTANCE_CONNECTION_NAME] with their associated values:
```
production:
  adapter: postgresql
  pool: 5
  timeout: 5000
  username: postgres
  password: [PASSWORD]
  database: postgres-database
  host: /cloudsql/[YOUR_INSTANCE_CONNECTION_NAME]
```
## Generate a model

* The new model contains a list of cat names and ages. You can generate a new Rails ActiveRecord model 
```
bundle exec rails generate model Cat name:string age:decimal

# model named Cat, a database table named cats, and a migration file with a similar name to db/migrate/20170302062657_create_cats.rb
```

*   Update the production Cloud SQL for PostgreSQL instance database postgres_instance 
```
RAILS_ENV=production bundle exec rails db:create
RAILS_ENV=production bundle exec rails db:migrate
```
## Add entries
add a few cats using the Rails console to try out the new Cat model
```
#  start the Rails console
RAILS_ENV=production bundle exec rails console

# use the following Ruby code to add two cat friends
Cat.create name: "Mr. Whiskers", age: 4
Cat.create name: "Ms. Paws", age: 2
exit
```
## List the different cats
add a page to display a list of cats to the generated Rails application

```
# Generate scaffolding for a new page with the rails generate command.
# The following command creates a new Rails controller named CatFriendsController with an index action

bundle exec rails generate controller CatFriends index

# generates new directories and files by default for the new CatFriendsController controller and index action
```

* Set the index controller action as the root action for Rails, and whenever a user visits the Rails app, they see the list of cats.
Open the file config/routes.rb
```
# Modify this file by adding root cat_friends#index

Rails.application.routes.draw do
  get 'cat_friends/index'
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html

  root 'cat_friends#index'
end
```

## Display database entries

* Modify the Rails app to display entries in the database.
```
# Open the file app/controllers/cat_friends_controller.rb and set an instance variable @cats that contains all cat entries. 

class CatFriendsController < ApplicationController
  def index
    @cats = Cat.all
  end
end

```
```
# Next, open the file app/views/cat_friends/index.html.erb, and add Ruby code to display each cat entry in @cats.

<h1>A list of my Cats</h1>

<% @cats.each do |cat| %>
<%=cat.name%> is <%=cat.age%> years old!<br />
<% end %>
```

> Before you can test the Rails app, you need to define a secret key for the production environment in Cloud Shell. A secret key is used to protect user session data.

* Generate a secret key
```
bundle exec rails secret

# Save your secret key to your computer, you'll be using a couple of times in this lab
```

* Set the environment variable SECRET_KEY_BASE with your secret key as its value:
```
export SECRET_KEY_BASE=[SECRET_KEY]
```
*   Test the Rails app with the Web Preview:
```
RAILS_ENV=production bundle exec rails assets:precompile
RAILS_ENV=production bundle exec rails server --port 8080
```

Once the application starts, click on the Web Preview icon in the Cloud Shell toolbar and choose Preview on port 8080.

## Deployment Configuration

You now have a working Cloud SQL for PostgreSQL with Rails app. Let's deploy it to the App Engine flexible environment!

App Engine Flexible environment uses an app.yaml file to describe an application's deployment configuration. If this file is not present, the gcloud tool will try to guess the deployment configuration. However, it is a good idea to provide this file because Rails requires a secret key in production and App Engine requires the connectionName for the Cloud SQL instance.

* Move into the directory created by Rails for the application:
```
cd app_name
```

Create a new file called app.yaml and add the following to the file, replacing [SECRET KEY] with the generated secret key and [YOUR_INSTANCE_CONNECTION_NAME] with the generated connectionName of the Cloud SQL for PostgreSQL instance:

```
entrypoint: bundle exec rackup --port $PORT
env: flex
runtime: ruby

env_variables:
  SECRET_KEY_BASE: [SECRET KEY]

beta_settings:
  cloud_sql_instances: [YOUR_INSTANCE_CONNECTION_NAME]
```

When the app is deployed, the environment variable SECRET_KEY_BASE in production will be set with the secret key and App Engine will set up the Cloud SQL Proxy to communicate with the Cloud SQL for PostgreSQL instance.

## Deploying the Application on App Engine


*   create an App Engine instance
prompt displays with a list of regions to select from
```
gcloud app create
```

*   deploy your app on App Engine
```
gcloud app deploy
```

## Check out your Rails app

* After the application deploys, you can visit it by opening the URL
https://[PROJECT_ID].appspot.com

