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

Run the following command in Cloud Shell:

gcloud -h

More verbose help can be obtained by appending --help flag, or executing gcloud help command. Run the following in Cloud Shell:

gcloud config --help


View the list of configurations in your environment:

$ gcloud config list

To check how other properties are set, see all properties by calling:

$ gcloud config list --all


List your components:

$ gcloud components list


gcloud interactive has auto prompting for commands and flags, and displays inline help snippets in the lower section as the command is typed.

Static information, like command and sub-command names, and flag names and enumerated flag values, are auto-completed using dropdown menus.

Install the beta components:

$ gcloud components install beta

Enter the gcloud interactive mode:

$ gcloud beta interactive


When using the interactive mode, click on the Tab key to complete file path and resource arguments. If a dropdown menu appears, use the Tab key to move through the list, and the Space bar to select your choice.

Try it out! Start typing the following command, using auto-complete to finish the command:

gcloud compute instances describe <your_vm>

SSH into your vm instance
gcloud compute makes connecting to your instances easy. The gcloud compute ssh command provides a wrapper around SSH, which takes care of authentication and the mapping of instance name to IP address.

Use gcloud compute ssh to SSH into your vm:

gcloud compute ssh gcelab2 --zone $ZONE
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
