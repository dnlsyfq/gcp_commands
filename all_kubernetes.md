* set the default zone:
```
gcloud config set compute/zone us-central1-a
```

* Creating a private cluster
When you create a private cluster, you must specify a /28 CIDR range for the 
VMs that run the Kubernetes master components and you need to enable IP aliases.

Next you'll create a cluster named private-cluster, 
and specify a CIDR range of 172.16.0.16/28 for the masters. 
When you enable IP aliases, you let Kubernetes Engine automatically create a subnetwork for you.

```
gcloud beta container clusters create private-cluster \
    --enable-private-nodes \
    --master-ipv4-cidr 172.16.0.16/28 \
    --enable-ip-alias \
    --create-subnetwork ""
```

* List the subnets in the default network
```
gcloud compute networks subnets list --network default
```

* get information about the automatically created subnet
```
gcloud compute networks subnets describe [SUBNET_NAME] --region us-central1
```

* Enabling master authorized networks
Run the following to Authorize your external address range, 
replacing [MY_EXTERNAL_RANGE] with the CIDR range of the external addresses from the previous output (your CIDR range is natIP/32). 
With CIDR range as natIP/32, we are whitelisting one specific IP address.

In a production environment replace [MY_EXTERNAL_RANGE] with your network external address CIDR range.
```
gcloud container clusters update private-cluster \
    --enable-master-authorized-networks \
    --master-authorized-networks [MY_EXTERNAL_RANGE]
```

* delete cluster 
```
gcloud container clusters delete private-cluster --zone us-central1-a
```

* create vm to check connectivity to cluster
```
gcloud compute instances create source-instance --zone us-central1-a --scopes 'https://www.googleapis.com/auth/cloud-platform'
```
* Get the <External_IP> of the source-instance 
```
gcloud compute instances describe source-instance --zone us-central1-a | grep natIP
```

* SSH into source-instance with:
```
gcloud compute ssh source-instance --zone us-central1-a
gcloud components install kubectl
sudo apt-get install kubectl
```
* Configure access to the Kubernetes cluster from SSH shell
gcloud container clusters get-credentials private-cluster --zone us-central1-a

* Verify that your cluster nodes do not have external IP addresses:
kubectl get nodes --output yaml | grep -A4 addresses
kubectl get nodes --output wide
