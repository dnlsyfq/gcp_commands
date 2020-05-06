# Deploying a Python Flask Web Application to App Engine Flexible

Google App Engine applications are easy to create, easy to maintain, and easy to scale as your traffic and data storage needs change. With App Engine, there are no servers to maintain. You simply upload your application and it's ready to go.

App Engine applications automatically scale based on incoming traffic. Load balancing, microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, search, versioning, roll out and roll backs, and security scanning are all supported natively and are highly customizable.

App Engine's Flexible Environment supports a host of programming languages, including Java, Python, PHP, NodeJS, Ruby, and Go. App Engine's Standard Environment is an additional option for certain languages including Python. The two environments give users maximum flexibility in how their application behaves since each environment has certain strengths. 

```
git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/codelabs/flex_and_vision
```

* Authenticate API Requests

In order to make requests to the APIs, you will need service account credentials. You can generate credentials from your project using gcloud in Cloud Shell. 
```
export PROJECT_ID=[YOUR_PROJECT_ID]
```

* Create a Service Account to access the Google Cloud APIs when testing locally
```
gcloud iam service-accounts create qwiklab \
  --display-name "My Qwiklab Service Account"
```

* Give your newly created Service Account appropriate permissions
```
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
--member serviceAccount:qwiklab@${PROJECT_ID}.iam.gserviceaccount.com \
--role roles/owner
```

* create a Service Account key:
> This command generates a service account key stored in a JSON file named key.json in your home directory.
```
gcloud iam service-accounts keys create ~/key.json \
--iam-account qwiklab@${PROJECT_ID}.iam.gserviceaccount.com
```

* Using the absolute path of the generated key, set an environment variable for your service account key:
```
export GOOGLE_APPLICATION_CREDENTIALS="/home/${USER}/key.json"
```

* Testing the Application Locally
```
virtualenv -p python3 env
source env/bin/activate
pip install -r requirements.txt
```
* Creating an App Engine App

```
gcloud app create
```

* Creating a Storage Bucket
```
export CLOUD_STORAGE_BUCKET=${PROJECT_ID}
gsutil mb gs://${PROJECT_ID}
```

* Running the Application
```
python main.py
```

* Deploying the App to App Engine Flexible
```
nano app.yaml

###### 
runtime: python
env: flex
entrypoint: gunicorn -b :$PORT main:app

runtime_config:
    python_version: 3

env_variables:
    CLOUD_STORAGE_BUCKET: <your-cloud-storage-bucket>
    
#######

gcloud app deploy
```

* show project id
```
gcloud config list project
```
