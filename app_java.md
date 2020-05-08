# App Dev: Setting up a Development Environment - Java

```
sudo su
sudo vim /etc/apt/sources.list
deb http://ftp.us.debian.org/debian sid main
sudo apt-get update
sudo apt-get install openjdk-8-jdk
sudo update-alternatives --config java
```


```


sudo apt-get update
sudo apt-get install git -y
sudo apt-get install -yq openjdk-8-jdk # sudo apt-get install openjdk-8-jdk
sudo update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
sudo apt-get install -yq maven
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
export GCLOUD_PROJECT=[GCP_PROJECT_ID]

git clone https://github.com/GoogleCloudPlatform/training-data-analyst
cd ~/training-data-analyst/courses/developingapps/java/devenv/
mvn clean install
mvn spring-boot:run
```
