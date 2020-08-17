# Deployment Manager - Full Production 

* pip env
```
sudo apt-get install virtualenv
virtualenv -p python3 venv
source venv/bin/activate
```
* show put forward ip
```
gcloud compute forwarding-rules list
```

* set default compute zone 
```
gcloud config set compute/zone us-central1-f

```

```
gcloud source repos list
```

--- 

Base:
```
for i in {1..10}; do
  echo Hello, World!
done 
```

Powershell:
```
for($i=1; $i -le 10; $i++){
  Write-Host "Hello, World!"
}
```
