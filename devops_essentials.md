# Kubernetes

Dev Ops practices will regularly make use of multiple deployments to manage application deployment scenarios such 
as "Continuous Deployment", "Blue-Green Deployments," "Canary Deployments" and more

* Set your working GCP zone
```
gcloud config set compute/zone us-central1-a
```

* create cluster
```
gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
```

* deployment object
> explain command in kubectl can tell us about the Deployment object
```
kubectl explain deployment

kubectl explain deployment.metadata.name
```

* Create a deployment
1. modify deployments/auth.yaml

Deployment is creating one replica and it's using version 1.0.0 of the auth container.
Scale the number of Pods by changing the number specified in the replicas field.
```
kind: Deployment
metadata:
  name: auth
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: auth
        track: stable
    spec:
      containers:
        - name: auth
          image: "kelseyhightower/auth:1.0.0"
          ports:
            - name: http
              containerPort: 80
            - name: health
              containerPort: 81
```

2. create your deployment object
When you run the kubectl create command to create the auth deployment, it will make one pod that conforms to the data in the Deployment manifest
```
kubectl create -f deployments/auth.yaml
```

3. verify that it was created
```
kubectl get deployments
```
4. verify that a ReplicaSet was created 
```
kubectl get replicasets
```
5. view the Pods that were created as part of our Deployment
```
kubectl get pods
```
6. to create a service for our auth deployment
```
kubectl create -f services/auth.yaml
```

7. create and deploy
```
kubectl create -f deployments/hello.yaml
kubectl create -f services/hello.yaml

kubectl create -f deployments/frontend.yaml
kubectl create -f services/frontend.yaml
```

8. gran external ip
```
kubectl get services frontend
```

9. scale , update replicas
```
kubectl scale deployment hello --replicas=5
```

10. verify no. of pods 
```
kubectl get pods | grep hello- | wc -l
```

11. To update your Deployment,
```
kubectl edit deployment hello
```
12. see new replicas
```
kubectl get replicaset
```

