Docker is an open platform for developing, shipping, and running applications. 
With Docker, you can separate your applications from your infrastructure and treat your infrastructure like a managed application. 
Docker helps you ship code faster, test faster, deploy faster, and shorten the cycle between writing code and running code.

Docker does this by combining kernel containerization features 
with workflows and tooling that helps you manage and deploy your applications.

Docker containers can be directly used in Kubernetes, 
which allows them to be run in the Kubernetes Engine with ease

* to run container
```
docker run hello-world
```

* to look at container image it pulled from Docker Hub
```
docker images
```

* look at the running containers
```
docker ps
```

* to see all all containes including finished one
```
docker ps -a
```
> container Names are also randomly generated but can be specified with docker run --name [container-name] hello-world

* build a Docker image that's based on a simple node application
```
mkdir test && cd test

# Create a Dockerfile
cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:6

# Set the working directory in the container to /app
WORKDIR /app

# Copy the current directory contents into the container at /app 
# current directory's contents (indicated by the "." ) 
ADD . /app

# Make the container's port 80 available to the outside world
EXPOSE 80

# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF
```

* to create the node application
```
cat > app.js <<EOF
const http = require('http');

const hostname = '0.0.0.0';
const port = 80;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
        res.end('Hello World\n');
});

* to build the image
> -t is to name and tag an image with the name:tag syntax
e.g. name of the image is node-app and the tag is 0.1
```
docker build -t node-app:0.1 .
```


server.listen(port, hostname, () => {
    console.log('Server running at http://%s:%s/', hostname, port);
});

process.on('SIGINT', function() {
    console.log('Caught interrupt signal and will exit');
    process.exit();
});
EOF
```

* to look at the images 
```
docker images

# node is the base image and node-app is the image you built
```

* to run containers based on the image 
> The --name flag allows you to name the container if you like. The -p instructs Docker to map the host's port 4000 to the container's port 80. Now you can reach the server at http://localhost:4000. 
```
docker run -p 4000:80 --name my-app node-app:0.1
```
---
The container will run as long as the initial terminal is running. 
If you want the container to run in the background (not tied to the terminal's session), 
you need to specify the -d flag.
---

* to stop and remove the container
```
docker stop my-app && docker rm my-app
```

* to start the container in the background
> Notice the container is running in the output of docker ps. You can look at the logs by executing docker logs [container_id].
```
docker run -p 4000:80 --name my-app -d node-app:0.1

docker ps
```

* modify and build this new image and tag it with 0.2
```
docker build -t node-app:0.2 .
```
* Run another container with the new image version. Notice how we map the host's port 8080 instead of 80. We can't use host port 4000 because it's already in use
```
docker run -p 8080:80 --name my-app-2 -d node-app:0.2
docker ps
```

* look at the logs of a container and follow the log's output as the container is running
```
docker logs -f [container_id]
```

* to start an interactive Bash session inside the running container.
> The -it flags let you interact with a container by allocating a pseudo-tty and keeping stdin open. Notice bash ran in the WORKDIR directory (/app) specified in the Dockerfile. 
> From here, you have an interactive shell session inside the container to debug.
```
docker exec -it [container_id] bash

# to Exit the Bash session
exit
```

* examine a container's metadata in Docker
```
docker inspect [container_id]
```

* inspect specific fields 
> --format to inspect specific fields from the returned JSON
```
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id]
```









