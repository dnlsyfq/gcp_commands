 list the active account name with this command:
 $gcloud auth list
 
  list the project ID with this command:
  $gcloud config list project
  
  
  Resources that live in a zone are referred to as zonal resources. Virtual machine Instances and persistent disks live in a zone. To attach a persistent disk to a virtual machine instance, both resources must be in the same zone. Similarly, if you want to assign a static IP address to an instance, the instance must be in the same region as the static IP.
  
  
  Install a NGINX web server
Now you'll install NGINX web server, one of the most popular web servers in the world, to connect your virtual machine to something

sudo su -
apt-get update
apt-get install nginx -y
ps auwx | grep nginx


gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone us-central1-c


gcloud compute instances create --help
gcloud compute ssh gcelab2 --zone [YOUR_ZONE]
exit

---
# Cloud Shell & gcloud

Default regions and zones are set by using the following values:

google-compute-default-zone google-compute-default-region

To see what your default region and zone settings are, run the following gcloud command, replacing <your_project_id> which you can see on the Home page in the Console or look in the Qwiklabs tab where you started this lab, with your Project ID:

gcloud compute project-info describe --project <your_project_ID>

> If the google-compute-default-region and google-compute-default-zone keys and values are missing from the response, that means no default zone or region is set.


Make a couple of environment variables:

export PROJECT_ID=<your_project_ID>

Set your ZONE environment variable (use the value for zone from the earlier command):

export ZONE=<your_zone>

echo $PROJECT_ID
echo $ZONE


$ gcloud compute 
* which enables you to easily manage your Google Compute Engine resources in a friendlier format than using the Compute Engine API.
$ instances create 
* creates a new instance.
```
gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone $ZONE
```
The name of the vm is "gcelab2",
You're using the --machine-type flag to specify the machine type as "n1-standard-2"
You're using the --zone flag to specify that it gets created in the zone you defined with your environment variable.

If you omit the --zone flag, gcloud can infer your desired zone based on your default properties. Other required instance settings, like machine type and image, if not specified in the create command, are set to default values.

