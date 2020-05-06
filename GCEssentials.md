# Google Cloud Platform
* Compute: houses a variety of machine types that support any type of workload. The different computing options let you decide how involved you want to be with operational details and infrastructure amongst other things.
* Storage: data storage and database options for structured or unstructured, relational or non relational data.
* Networking: services that balance application traffic and provision security rules amongst other things.
* Stackdriver: a suite of cross-cloud logging, monitoring, trace, and other service reliability tools.
* Tools: services for developers managing deployments and application build pipelines.
* Big Data: services that allow you to process and analyze large datasets.
* Artificial Intelligence: a suite of APIs that run specific artificial intelligence and machine learning tasks on the Google Cloud platform.

## Roles and Permissions
**Cloud Identity and Access Management (IAM)**

|Role Name|Permissions|
|---|---|
|roles/viewer|Permissions for read-only actions that do not affect state, such as viewing (but not modifying) existing resources or data.|
|roles/editor|All viewer permissions, plus permissions for actions that modify state, such as changing existing resources.able to create, modify, and delete GCP resources.  won't be able to add or delete members from GCP projects|
|roles/owner|All editor permissions and permissions for the following actions: * Manage roles and permissions for a project and all resources within the project.* Set up billing for a project.|

* lists the credentialed account(s) in your GCP project
```
gcloud auth list
```
---
# Creating a Virtual Machine

* list the project ID 
```
gcloud config list project
```
* Understanding Regions and Zones

Certain Compute Engine resources live in regions or zones. A region is a specific geographical location where you can run your resources. Each region has one or more zones

>  us-central1 region denotes a region in the Central United States that has zones us-central1-a, us-central1-b, us-central1-c, and us-central1-f.

Resources that live in a zone are referred to as zonal resources. Virtual machine Instances and persistent disks live in a zone. To attach a persistent disk to a virtual machine instance, both resources must be in the same zone. Similarly, if you want to assign a static IP address to an instance, the instance must be in the same region as the static IP.

* Create a new instance from the Cloud Console

  * Name : gcelab
  * Region : us-central1 (Iowa)
  * Zone : us-central1-c
  * Machine Type : 2 vCPUs
  * Boot Disk : New 10 GB standard persistent disk , OS Image: Debian GNU/Linux 9 (stretch)
  * Firewall : Allow HTTP traffic
  
* Install a NGINX web server
install NGINX web server, to connect your virtual machine to something
```
sudo su -
apt-get update
apt-get install nginx -y
ps auwx | grep nginx
```
* Create a new instance with gcloud
```
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone us-central1-c
```

```
 # to see all the defaults.
gcloud compute instances create --help

# SSH into your instanc
gcloud compute ssh gcelab2 --zone [YOUR_ZONE]
exit
```
---
# Cloud Shell & gcloud

* Default regions and zones are set by using the following values:
```
google-compute-default-zone google-compute-default-region
```

* To see what your default region and zone settings
```
gcloud compute project-info describe --project <your_project_ID>
```

> If the google-compute-default-region and google-compute-default-zone keys and values are missing from the response, that means no default zone or region is set.

* Initializing Cloud SDK

The gcloud CLI is a part of the Google Cloud SDK. You need to download and install the SDK on your own system and initialize it (by running gcloud init) before you can use the gcloud command-line tool.

* Setting environment variables

```
export PROJECT_ID=<your_project_ID>
export ZONE=<your_zone>

echo $PROJECT_ID
echo $ZONE
```
* Create a virtual machine with gcloud

```
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone $ZONE
```
The name of the vm is "gcelab2",
You're using the --machine-type flag to specify the machine type as "n1-standard-2"
You're using the --zone flag to specify that it gets created in the zone you defined with your environment variable.

If you omit the --zone flag, gcloud can infer your desired zone based on your default properties. Other required instance settings, like machine type and image, if not specified in the create command, are set to default values.

gloud help
```
gcloud -h
gcloud config --help
gcloud help config
```

* View the list of configurations in your environment:
```
gcloud config list
```
* To check how other properties are set, see all properties by calling:
```
gcloud config list --all
```

* List your components:
```
gcloud components list
```

* gcloud interactive has auto prompting for commands and flags, and displays inline help snippets in the lower section as the command is typed.When using the interactive mode, click on the Tab key to complete file path and resource arguments. If a dropdown menu appears, use the Tab key to move through the list, and the Space bar to select your choice.

```
gcloud components install beta

# Enter the gcloud interactive mode:
gcloud beta interactive
```
* describe instance 
```
gcloud compute instances describe <your_vm>
```

* SSH into your vm instance

* to SSH into your vm
```
gcloud compute ssh gcelab2 --zone $ZONE
```
---

vi ./.bashrc 
> Press the ESC key and then :wq to exit the editor.


---

# Kubernetes Engine: Qwik Start

1. list the active account name with this command

```
$ gcloud auth list
```

2. list the project ID 
```
$ gcloud config list project
```

3. Setting a default compute zone
```
$ gcloud config set compute/zone us-central1-a
```

4. Creating a Kubernetes Engine cluster
```
$ gcloud container clusters create [CLUSTER-NAME]
```

5. Get authentication credentials for the cluster
After creating your cluster, you need to get authentication credentials to interact with the cluster.
```
$ gcloud container clusters get-credentials [CLUSTER-NAME]
```

6. Deploying an application to the cluster

deploy a containerized application . 
 
Kubernetes Engine uses Kubernetes objects to create and manage your cluster's resources. Kubernetes provides the Deployment object for deploying stateless applications like web servers. Service objects define rules and load balancing for accessing your application from the Internet.
```
# to create a new Deployment hello-server from the hello-app container image:

$ kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

# This Kubernetes command creates a Deployment object that represents hello-server. In this case, --image specifies a container image to deploy. The command pulls the example image from a Google Container Registry bucket. gcr.io/google-samples/hello-app:1.0 indicates the specific image version to pull. If a version is not specified, the latest version is used.
```
create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application to external traffic
```
$ kubectl expose deployment hello-server --type=LoadBalancer --port 8080

# --port specifies the port that the container exposes.
# type="LoadBalancer" creates a Compute Engine load balancer for your container.
```
Inspect the hello-server Service
```
$ kubectl get service
```

From this command's output, copy the Service's external IP address from the EXTERNAL IP column.
> http://[EXTERNAL-IP]:8080

7. to delete the cluster
```
$ gcloud container clusters delete [CLUSTER-NAME]
```

---

# Set Up Network and HTTP Load Balancers

* Set the default region and zone for all resources
```
# set the default zone
$ gcloud config set compute/zone us-central1-a

# Set the default region
$ gcloud config set compute/region us-central1

```

1. Create multiple web server instances

To simulate serving from a cluster of machines, create a simple cluster of Nginx web servers to serve static content using Instance Templates and Managed Instance Groups

> Instance Templates define the look of every virtual machine in the cluster (disk, CPUs, memory, etc)

> Managed Instance Groups instantiate a number of virtual machine instances using the Instance Template

To create the Nginx web server clusters, create the following:

* A startup script to be used by every virtual machine instance to setup Nginx server upon startup
* An instance template to use the startup script
* A target pool
* A managed instance group using the instance template

 create a startup script to be used by every virtual machine instance
 ```
 cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
 ```
Create an instance template, which uses the startup script:
```
gcloud compute instance-templates create nginx-template \
         --metadata-from-file startup-script=startup.sh
```

Create a target pool. A target pool allows a single access point to all the instances in a group and is necessary for load balancing in the future steps.
```
gcloud compute target-pools create nginx-pool
```
Create a managed instance group using the instance template:
```
gcloud compute instance-groups managed create nginx-group \
         --base-instance-name nginx \
         --size 2 \
         --template nginx-template \
         --target-pool nginx-pool
```

List the compute engine instances
```
gcloud compute instances list
```

Now configure a firewall so that you can connect to the machines on port 80 via the EXTERNAL_IP addresses:
```
gcloud compute firewall-rules create www-firewall --allow tcp:80
```

* Create a Network Load Balancer
Network load balancing allows you to balance the load of your systems based on incoming IP protocol data, such as address, port, and protocol type

Pros over HTTP load balancing:
 * you can load balance additional TCP/UDP-based protocols such as SMTP traffic
 * if your application is interested in TCP-connection-related characteristics, network load balancing allows your app to inspect the packets
 
 Create an L3 network load balancer targeting your instance group:
 ```
 gcloud compute forwarding-rules create nginx-lb \
         --region us-central1 \
         --ports=80 \
         --target-pool nginx-pool
 ```
List all Google Compute Engine forwarding rules in your project
```
gcloud compute forwarding-rules list
```

* Create a HTTP(s) Load Balancer

HTTP(S) load balancing provides global load balancing for HTTP(S) requests destined for your instances. configure URL rules that route some URLs to one set of instances and route other URLs to other instances. Requests are always routed to the instance group that is closest to the user, provided that group has enough capacity and is appropriate for the request. If the closest group does not have enough capacity, the request is sent to the closest group that does have capacity.

Health checks verify that the instance is responding to HTTP or HTTPS traffic:
```
gcloud compute http-health-checks create http-basic-check
```

Define an HTTP service and map a port name to the relevant port for the instance group. Now the load balancing service can forward traffic to the named port:
```
gcloud compute instance-groups managed \
       set-named-ports nginx-group \
       --named-ports http:80
```

Create a backend service:
```
gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global
```

Add the instance group into the backend service:
```
gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global
```

Create a default URL map that directs all incoming requests to all your instances:
```
gcloud compute url-maps create web-map \
    --default-service nginx-backend
```

Create a target HTTP proxy to route requests to your URL map:
```
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-map
```

Create a global forwarding rule to handle and route incoming requests. A forwarding rule sends traffic to a specific target HTTP or HTTPS proxy depending on the IP address, IP protocol, and port specified. The global forwarding rule does not support multiple ports.
```
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
```

```
gcloud compute forwarding-rules list
```
---
# APIs Explorer: Cloud SQL

The Google APIs Explorer is a tool that helps you explore various Google APIs interactively. With the APIs Explorer, you can:

Browse quickly through available APIs and versions.
See methods available for each API and what parameters they support along with inline documentation.
Execute requests for any method and see responses in real time.
Make authenticated and authorized API calls.
Search across all services, methods, and your recent requests to quickly find what you are looking for.

Cloud SQL is a fully-managed database service that makes it easy to set up, maintain, manage, and administer your relational PostgreSQL and MySQL databases in the cloud. Cloud SQL offers high performance, scalability, and convenience. Hosted on Google Cloud Platform, Cloud SQL provides a database infrastructure for applications running anywhere.

## Build a Cloud SQL instance with instances.insert

1. select APIs & Services > Library
2. find Cloud SQL Admin API  , Make sure that API is enabled, if not click Enable
3. open https://cloud.google.com/sql/docs/mysql/admin-api/rest/
4. From the left APIs & Reference section navigate to API and reference > Cloud SQL v1beta4 API > Instances > insert, to select sql.instances.insert method to create an SQL instance resource.
5. fill out a form to use the sql.instances.insert method. The Request body contains the resource properties that you want to use for creating your MySql instance:
```
project: = your Qwiklabs GCP Project ID
"name": "my-instance"
settings:tier: db-n1-standard-1
```
6. Make sure that Google OAuth 2.0 and API Key checkbox is selected under Credentials section
7. click the Execute button.

## View your Cloud SQL instance
1. From the left menu, select SQL,
2. instance has been created

## Create a database with databases.insert
1. API and reference > Cloud SQL v1beta4 API > Databases > insert to select sql.databases.insert method 
```
instance: my-instance
name: mysql-db
project: your Qwiklabs GCP Project ID.
```
## View your new database

1. From the Navigation menu select SQL, which is located under the Storage header. This will bring you to the instances page. Click on my-instance. Then select the databases tab.

## Create a table in your MySQL database and upload a CSV file to a Cloud Storage Bucket

* connect to your MySQL instance
```
gcloud sql connect my-instance --user=root
```

* to create a new Cloud Storage bucket,
```
gsutil mb gs://<YOUR_BUCKET_NAME>
```

*  to upload the CSV file to your Cloud Storage bucket
```
gsutil cp employee_info.csv gs://<YOUR_BUCKET_NAME>/
```

* To upload this file to your MySQL database, update specific permissions with your Cloud SQL service account

1. select SQL and then click on my-instance.
From the overview page, scroll down and find the "Service account" card and copy the service account name

2. open the navigation menu and select Storage > Browser.
Click on the three-dotted menu on the far right side of the bucket and click Edit bucket permissions.
For the member field, Click + Add member.
Now paste the Cloud SQL service account name you copied earlier in the New members.
Click the roles drop down and select Storage > Storage Admin.

## Add a CSV file to your database using instances.import
1. From the left APIs & Reference section navigate to API and reference > Cloud SQL v1beta4 API > Instances > import to select sql.instances.import method
```
project: = your Qwiklabs GCP Project ID

instance: = my-instance

Request body = Click inside the brackets to select the following properties:

importContext:
database: mysql-db
uri: gs://<YOUR_BUCKET_NAME>/employee_info.csv, replacing <YOUR_BUCKET_NAME> with the name of your bucket
fileType: csv
csvImportOptions:
table: info
```

## Inspect your updated database

Return to the GCP Console and return to your MySQL Cloud Shell tab that you left open. You will now see if the info table picked up the CSV file data

## Delete your database
1. From the left APIs & Reference section navigate to API and reference > Cloud SQL v1beta4 API > Databases > delete to select sql.databases.delete method
2. fill out a form to use the sql.databases.delete method
```
project: = your Qwiklabs GCP Project ID

instance: = my-instance

database: = mysql-db
```

## View your updated Cloud SQL instance
From the GCP Console, from the Navigation menu select SQL, which is located under the Storage header. This will bring you to the instances page.

Click on my-instance, then click on the databases tab


