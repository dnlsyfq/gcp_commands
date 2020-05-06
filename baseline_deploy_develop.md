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
