# Google Cloud SDK: Qwik Start - Redhat/Centos

install Google Cloud SDK to a virtual machine, 
initialize it and run core gcloud commands from the command-line. 
The Cloud SDK RPM packages are supported for Red Hat Enterprise Level 7 and CentOS 7.

## Set up a VM to use
## Update the Cloud SDK RPM packages
```
# Update YUM with Cloud SDK repo information:
sudo tee -a /etc/yum.repos.d/google-cloud-sdk.repo << EOM
[google-cloud-sdk]
name=Google Cloud SDK
baseurl=https://packages.cloud.google.com/yum/repos/cloud-sdk-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
       https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOM

# The indentation for the 2nd line of gpgkey is important.

# Install the Cloud SDK
sudo yum install google-cloud-sdk
```

## Initialize the SDK in your instance

Use the gcloud init command to perform several common SDK setup tasks. These include authorizing the SDK tools to access 
Google Cloud Platform using your user account credentials and setting up the default SDK configuration

```
gcloud init --console-only
```

1.  This prevents the gcloud init command from launching a web browser. Choose option 2, to log in with a new account.
Type the number for adding a new account.

2.  You will get confirmation that you're running on virtual machine. Type Y to allow the credentials you 
logged into the lab with (this is your personal account for this lab) to be used to authenticate your account

3.  You'll be given a long URL click on it or paste it into a new browser. 
You may be asked to select your lab credentials again, and Allow access to your account.

4.  This URL will give you your authentication code. 
Copy the code and paste it into the SSH window at the command prompt, then press Enter.

5.  Now type the number corresponding to you GCP Project ID

## Run core gcloud commands
to view information about your SDK installation

* List accounts whose credentials are stored on this VM
```
gcloud auth list
```
* list the properties in your active SDK configuration
```
gcloud config list
```

* to view information on your Cloud SDK installation and the active SDK configuration
```
gcloud info
```
---

# App Engine: Qwik Start - Python

## Enable Google App Engine Admin API
The App Engine Admin API enables developers to provision and manage their App Engine Applications.

1.     In the left menu click APIs & Services > Library.
2.     Type "App Engine Admin API" in search box.
3.     Click App Engine Admin API.

## Download the Hello World app

1.     git clone https://github.com/GoogleCloudPlatform/python-docs-samples
2.     cd python-docs-samples/appengine/standard_python37/hello_world

## Test the application
Test the application using the Google Cloud development server (dev_appserver.py), which is included with the preinstalled App Engine SDK.

1. From within your helloworld directory where the app's app.yaml configuration file is located,
```
dev_appserver.py app.yaml
```
2. View the results by clicking the Web preview > Preview on port 8080.

## Make a change
```
cd python-docs-samples/appengine/standard_python37/hello_world
nano main.py

# Reload the Hello World! Browser or click the Web Preview > Preview on port 8080 to see the results.
```

## Deploy your app
run the following command from within the root directory of your application where the app.yaml file is located:
```
gcloud app deploy
gcloud app browse
```
---

# Cloud Source Repositories: Qwik Start

Google Cloud Source Repositories provides Git version control to support collaborative development of any application or service. In this lab, you will create a local Git repository that contains a sample file, add a Google Source Repository as a remote, and push the contents of the local repository. You will use the source browser included in Source Repositories to view your repository files from within the Google Cloud Platform Console.

```
gcloud source repos create REPO_DEMO
gcloud source repos clone REPO_DEMO
cd REPO_DEMO
echo "Hello World!" > myfile.txt
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git add myfile.txt
git commit -m "First file using Cloud Source Repositories" myfile.txt
git push origin master

```
Source Repositories > Source Code.

---
# Container-Optimized OS: Qwik Start

Container-Optimized OS is an operating system image for your Compute Engine VMs that is optimized for running Docker containers, and is Google's recommended OS for running containers on Google Cloud Platform. 

Since it comes with all container-related dependencies preinstalled, Container-Optimized OS allows your cluster to quickly scale up or down in response to traffic or workload changes, optimizing your spend and improving your reliability.

Container-Optimized OS powers many GCP services such as Kubernetes Engine and Cloud SQL, making it Google's go-to solution for container workloads.


## Container-Optimized OS benefits

*      Run Containers Out of the Box: Container-Optimized OS instances come pre-installed with the Docker runtime and cloud-init. With a Container-Optimized OS instance, you can bring up your Docker container at the same time you create your VM, with no on-host setup required.

*      Smaller attack surface: Container-Optimized OS has a smaller footprint, reducing your instance's potential attack surface.

*      Locked-down by default: Container-Optimized OS instances include a locked-down firewall and other security settings by default.

*      Automatic Updates: Container-Optimized OS instances are configured to automatically download weekly updates in the background; only a reboot is necessary to use the latest updates.


### Use cases for Container-Optimized OS
Container-Optimized OS can be used to run most Docker containers. You should consider using Container-Optimized OS as the operating system for your Compute Engine instance if you have the following needs:

*      You need support for Docker containers or Kubernetes with minimal setup.

*      You need an operating system that has a small footprint and is security hardened for containers.

*      You need an operating system that is tested and verified for running Kubernetes on your Compute Engine instances.


* Container-Optimized OS features
Compute Engine provides several public VM images that you can use to create instances and run your container workloads. Some of these public VM images have a minimalistic container-optimized operating system that includes newer versions of Docker, rkt, or Kubernetes preinstalled. The following public image families are designed specifically to run containers:

* Container-Optimized OS from Google
```
Includes: Docker, Kubernetes
Image project: cos-cloud
Image family: cos-stable
```
* CoreOS
```
Includes: Docker, rkt, Kubernetes
Image project: coreos-cloud
Image family: coreos-stable
```
* Ubuntu
```
Includes: LXD
Image project: ubuntu-os-cloud
Image family: ubuntu-1604-lts
```
* Windows
```
Includes: Docker
Image project: windows-cloud
Image family: windows-1709-core-for-containers
```

In a production environment, if you need to run specific container tools and technologies on images that do not include them by default, install those technologies manually.

## Create an instance using the console
1. Click on Compute Engine > VM instances, then click on Create.

## Verify your Docker environment
Click SSH on the containerized-vm line to SSH into the containerized-vm instance.
```
sudo docker ps
```
On the VM instance console, click on the External IP for containerized-vm instance, which will open a new tab

## Create an instance using CLI
use the Cloud Shell command line to create a Container-Optimized OS instance.
```
#  to see what Container-Optimized OS images are available on Google Cloud Platform to create an instance.
gcloud compute images list \
    --project cos-cloud \
    --no-standard-images
```

Use the gcloud compute instances create command with --image and --image-project flags to create a cos node image instance
```gcloud beta compute instances create-with-container containerized-vm2 \
     --image cos-stable-72-11316-136-0 \
     --image-project cos-cloud \
     --container-image nginx \
     --container-restart-policy always \
     --zone us-central1-a \
     --machine-type n1-standard-1
     
In the above example, cos-stable-72-11316-136-0 is one of the available cos releases. Please use the latest available image from cos-stable family and replace it with an image for your VM instance. It is recommended to use --preemptible flag for one-off experimental instances     
```

 to create firewall rules to allow HTTP traffic from the internet and to enable all internal traffic within the VPC:
 ```
 gcloud compute firewall-rules create allow-containerized-internal\
  --allow tcp:80 \
  --source-ranges 0.0.0.0/0 \
  --network default
 ```
 
In a browser, enter your external IP address to verify that nginx is running

