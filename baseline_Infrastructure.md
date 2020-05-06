# Cloud Storage: Qwik Start - Cloud Console

## Create a Bucket
1. Navigation menu > Storage > Browser. Click Create Bucket:

## Share an Object Publicly
1.  To create a publicly accessible URL for the object, click the drop-down menu (three vertical dots).

2.  Select Edit permissions from the drop-down menu.

3.  In the dialog that appears, click the + Add item button.

4.  Add a permission for all users by entering in the following:
```
Select User for the Entity.
Enter allUsers for the Name.
Select Reader for the Access.
```
---

# Cloud IAM: Qwik Start

## Setup for two users
These represent identities in Cloud IAM, each with different access permissions allocated to them.

## View or reset the user in a browser tab
To view which user in signed into a browser tab, hover over your Avatar to view your username in that browser tab.


* To reset which user is signed into a browser tab:
1.  Click your Avatar and click Sign out to sign out.
2. In the Connection Details panel, click Open Google Console and sign back in using the appropriate Username and Password.

## IAM console and project level roles

Navigation menu > IAM & admin > IAM. You are now in the "IAM & admin" console.
Click +ADD button near the top of the page and explore the project roles associated with Projects by clicking on the "Select a role" dropdown menu


## Remove project access

* Remove Project Viewer for Username 2
Navigation menu > IAM & admin > IAM. Then click the pencil icon next to Username 2.
Remove Project Viewer access for Username 2 by clicking the trashcan icon next to the role name. Then click SAVE.

## Add Storage permissions
select Navigation menu > IAM & admin > IAM.

Click + ADD to then paste the Username 2 name into the New members field.

In the Roles field, select Storage > Storage Object Viewer from the drop-down menu.

Click SAVE.

## Verify access
```
gsutil ls gs://[YOUR_BUCKET_NAME]
```

---

# Cloud Monitoring: Qwik Start

Cloud Monitoring provides visibility into the performance, uptime, and overall health of cloud-powered applications. Cloud monitoring collects metrics, events, and metadata from Google Cloud, Amazon Web Services, hosted uptime probes, application instrumentation, and a variety of common application components including Cassandra, Nginx, Apache Web Server, Elasticsearch, and many others. Cloud monitoring ingests that data and generates insights via dashboards, charts, and alerts. Cloud monitoring alerting helps you collaborate by integrating with Slack, PagerDuty, HipChat, Campfire, and more.

This hands-on lab shows you how to monitor a Google Compute Engine virtual machine (VM) instance with Cloud monitoring. You'll also install monitoring and logging agents for your VM which collects more information from your instance, which could include metrics and logs from 3rd party apps.

## Create a Compute Engine instance
1. Navigation menu > Compute Engine > VM instances, then click Create.
```
Name: lamp-1-vm

Region: us-central1 (Iowa) or asia-south1 (Mumbai)

Zone: us-central1-a or asia south1-a

Machine type: n1-standard-2

Firewall: Select Allow HTTP traffic
```
## Add Apache2 HTTP Server to your instance
1.  In the Console, click SSH to open a terminal to your instance.
```
Run the following commands in the SSH window
sudo apt-get update
sudo apt-get install apache2 php7.0
sudo service apache2 restart
```
2. Click the External IP for lamp-1-vm instance to see the Apache2 default page for this instance.

## Create a Monitoring workspace
setup a Monitoring workspace that's tied to your Qwiklabs GCP Project. The following steps create a new account that has a free trial of Monitoring.

1. Navigation menu > Monitoring.
```
Install the Monitoring and Logging agents
Agents collect data and then send or stream info to Cloud Monitoring in the Console.

The Cloud Monitoring agent is a collectd-based daemon that gathers system and application metrics from virtual machine instances and sends them to Monitoring. By default, the Monitoring agent collects disk, CPU, network, and process metrics. Configuring the Monitoring agent allows third-party applications to get the full list of agent metrics. Learn more.

The Cloud Logging agent streams logs from your VM instances and from selected third-party software packages to Cloud Logging. It is a best practice to run the Cloud Logging agent on all your VM instances
```
* install the agents on the VM
2. In the Monitoring Overview window, click INSTALL AGENTS 
3. Run the Monitoring agent install script command in the SSH terminal of your VM instance to install the Cloud Monitoring agent
4. Run the Logging agent install script command in the SSH terminal of your VM instance to install the Cloud Logging agent

## Create an uptime check
Uptime checks verify that a resource is always accessible. For practice, create an uptime check to verify your VM is up.
1. in the left menu, click Uptime checks, and then click Create Uptime Check.
```
Title: Lamp Uptime Check

Check type: HTTP

Resource Type: Instance

Applies to: Single, lamp-1-vm

Path: leave at default

Check every: 1 min
```
2.  Click Test to verify that your uptime check can connect to the resource.

3.  When you see a green check mark everything can connect. Click Save.

4.  Click No, thanks to create an alerting policy for this check.

## Create an alerting policy

--- 

# Cloud Functions: Qwik Start - Console

Google Cloud Functions is a serverless execution environment for building and connecting cloud services. With Cloud Functions you write simple, single-purpose functions that are attached to events emitted from your cloud infrastructure and services. Your Cloud Function is triggered when an event being watched is fired. Your code executes in a fully managed environment. There is no need to provision any infrastructure or worry about managing any servers.

Cloud Functions are written in Javascript and execute in a Node.js environment on Google Cloud Platform. You can take your Cloud Function and run it in any standard Node.js runtime which makes both portability and local testing a breeze.

## Connect and Extend Cloud Services

Cloud Functions provides a connective layer of logic that lets you write code to connect and extend cloud services. Listen and respond to a file upload to Cloud Storage, a log change, or an incoming message on a Cloud Pub/Sub topic. Cloud Functions augments existing cloud services and allows you to address an increasing number of use cases with arbitrary programming logic. Cloud Functions have access to the Google Service Account credential and are thus seamlessly authenticated with the majority of Google Cloud Platform services such as Datastore, Cloud Spanner, Cloud Translation API, Cloud Vision API, as well as many others. In addition, Cloud Functions are supported by numerous Node.js client libraries, which further simplify these integrations.

## Events and Triggers
Cloud events are things that happen in your cloud environment.These might be things like changes to data in a database, files added to a storage system, or a new virtual machine instance being created.

Events occur whether or not you choose to respond to them. You create a response to an event with a trigger. A trigger is a declaration that you are interested in a certain event or set of events. Binding a function to a trigger allows you to capture and act on events. For more information on creating triggers and associating them with your functions, see Events and Triggers.

## Serverless
Cloud Functions removes the work of managing servers, configuring software, updating frameworks, and patching operating systems. The software and infrastructure are fully managed by Google so that you just add code. Furthermore, provisioning of resources happens automatically in response to events. This means that a function can scale from a few invocations a day to many millions of invocations without any work from you.

## Use Cases
Asynchronous workloads like lightweight ETL, or cloud automations like triggering application builds now no longer need their own server and a developer to wire it up. You simply deploy a Cloud Function bound to the event you want and you're done.

The fine-grained, on-demand nature of Cloud Functions also makes it a perfect candidate for lightweight APIs and webhooks. In addition, the automatic provisioning of HTTP endpoints when you deploy an HTTP Function means there is no complicated configuration required as there is with some other services

## Use Case

* Data Processing / ETL

Listen and respond to Cloud Storage events such as when a file is created, changed, or removed. Process images, perform video transcoding, validate and transform data, and invoke any service on the Internet from your Cloud Function.

* Webhooks

Via a simple HTTP trigger, respond to events originating from 3rd party systems like GitHub, Slack, Stripe, or from anywhere that can send HTTP requests.

* Lightweight APIs

Compose applications from lightweight, loosely coupled bits of logic that are quick to build and that scale instantly. Your functions can be event-driven or invoked directly over HTTP/S.

* Mobile Backend

Use Google's mobile platform for app developers, Firebase, and write your mobile backend in Cloud Functions. Listen and respond to events from Firebase Analytics, Realtime Database, Authentication, and Storage.

* IoT

Imagine tens or hundreds of thousands of devices streaming data into Cloud Pub/Sub, thereby launching Cloud Functions to process, transform and store data. Cloud Functions lets you do in a way that's completely serverless.

## Create a function
 Navigation menu > Cloud Functions.
```
Name : GCFunction

Memory allocated :Default

Trigger : HTTP trigger

Source code :Inline editor (use the default helloWorld function implementation already provided for index.js.)
```

## Test the function
In the Cloud Functions Overview page, display the menu for your function, and click Test function.
In the Triggering event field, enter the following text between the brackets {} and click Test the function.
```
"message":"Hello World!"
```

---

# Google Cloud Pub/Sub: Qwik Start - Console

Google Cloud Pub/Sub is a messaging service for exchanging event data among applications and services. A producer of data publishes messages to a Cloud Pub/Sub topic. A consumer creates a subscription to that topic. Subscribers either pull messages from a subscription or are configured as webhooks for push subscriptions. Every subscriber must acknowledge each message within a configurable window of time.

## Setting up Pub/Sub
To use a Pub/Sub, you create a topic to hold data and a subscription to access data .published to the topic.

1. Navigation menu > Pub/Sub > Topics.
2. Click Create a topic.
3. The topic must have a unique name. For this lab, name your topic MyTopic. In the Create a topic dialog:
```
Name your topic MyTopic.
Leave Encryption at the default value.
Click CREATE TOPIC.
```

## Add a subscription
Now you'll make a subscription to access the topic.

1. Click Topics in the left panel to return to the Topics dialog. For the topic you just made click the three dot icon > Create subscription.
2. In the Add subscription to topic dialog:
```
Type a name for the subscription, such as MySub
Set the Delivery Type to Pull.
Leave all other options at the default values.
Click Create.
```

## Publish a message to the topic
1. At the top of the Topics details dialog, click PUBLISH MESSAGE. You may have to widen the browser window to see the PUBLISH MESSAGE option.
2. Enter Hello World in the Message field and click Publish.


## View the message
1. To view the message you'll use the subscription (MySub) to pull the message (Hello World) from the topic (MyTopic).

Enter the following command in command line.
```
gcloud pubsub subscriptions pull --auto-ack MySub
```
 published to the topic, created a subscription, then used the subscription to pull data from the topic.
