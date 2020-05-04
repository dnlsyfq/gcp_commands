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
