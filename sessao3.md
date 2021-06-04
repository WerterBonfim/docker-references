# Creating and Using Containers Like a Boss

* 1 - Check Our Docker Install and Config
* 2 - Starting a Nginx Web Server
* 3 - Debrief: What happens when we run a container
* 4 - Container vs. VM: It's just a process

## 1 - Check Our Docker Install and Config

```bash
docker version

docker info # shows most configuration values for the engine

```
### Docker command format
* old(still works): docker <command> (options)
* new: docker <command> <sub-command> (options)

## 2 - Starting a Nginx Web Server

### Image vs. Container

* An image is the application we want to run
* A container is a instance of that image running as a process
* You can have many containers running off the same image

```bash
docker container run --publish 80:80 nginx
docker container run --publish 80:80 --detach nginx
docker container ls 

docker container run \
    # port short: -p
    --publish 80:80 
    # detach mode short: -d
    --detach nginx
    # specify name
    --name webhosts

# always start a new container
docker container run

# to start an existing stopped one
docker container start

# show logs for ua specific container
docker container logs webhosts

# extra options logs
    --details           # Show extra details provided to log
    --follow or -f      # follow log output
    --tail or -n [0-9]  # Number of line to show from the end of logs


# display the running processes of a container
docker container top webhosts

# Remove one or more containers
docker container rm 027

# extra options rm
    --force, -f     # Force the removal of a running container (uses SIGKILL)
    --link, -l      # Remove the specified link
    --volume, -v    # Remove thefolumes associates with the container

```

## 3 - Debrief: What happens when we run a container

What happens in 'docker container run'

1. Looks for that image locally in image cache, doesn't find anything
2. Then looks in remote image repository (efaults to Docker Hub)
3. Download the last version (nginx: lastest by default)
4. Create new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile

```bash
docker container run
    # change host listening port
    --publish 8080:80
    --name webhost
    -d
    # change version of image
    nginx:1.11 
    # change CMD run on start
    nginx -T
```

## 4 - Container vs. VM: It's just a process

Containers aren't Mini-VM's

* They are just processes
* Limited to what resources they can access
* Exit when process stops


```bash

docker run --name mongo -d mongo

# list running processes in specific container
docker top mongo

docker stop mongo
```
