# Create a custom nginx image
## How to build a custom nginx for a static web site

### Install docker on Ubuntu 19.04
    
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu disco stable test"
    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io
    sudo adduser $(whoami) docker 

### Create a folder for this project

### Create "Dockerfile" file

    FROM ubuntu:xenial
    RUN apt update && apt install -y nginx
    EXPOSE 80
    COPY . /var/www/html
    CMD ["nginx", "-g", "daemon off;"]
    
### Create your index.html file, this must be in the same directory than the Dockerfile

    <html>
    <head>
      <title>Raspberry Kubernetes</title>
    </head>
    <body>
      <h1>My static web site running on Raspberry Kubernetes</h1>
    </body>
    </html>
    
### Build the image

    docker build -t <replace by your image name> .
    ie: docker build -t some-content-nginx .
    Note the point at the end is important dont delete
    
### List image
    
    docker images
    You should be able to see you image listed.
    REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
    my-nginx             v1.0                4e3c3cfc6dbf        12 minutes ago      205MB
    some-content-nginx   latest              4e3c3cfc6dbf        12 minutes ago      205MB
    ubuntu               xenial              56bab49eef2e        7 days ago          123MB

### Run the image

    docker run --name some-nginx -d -p 8080:80 some-content-nginx
    Note the name of the image is at the end.
    
### See running containers
    docker ps -a
    Use name for docker stop [container name

### See it...
    
    go to http://localhost:8080/
    and you should be able to see the index.html content displayed
    
## Push image to hub repository
### Create a repository

    Login into your docker account, if you dont have create one
    Create a private repository

### Login to docker hub
    
    docker login -u "username" -p "password" docker.io
    
### Tag the image prior push
The tagname should be identical to your respository ie: 

    sudo docker tag some-content-nginx:latest crhostservices/custom_nginx:latest
    See the crhostservices/custom_ngnix

    docker tag local-image:tagname new-repo:tagname

    if you run:
    
    docker images
    You should see your tag image

### Pushing the image to Docket io

    same case here , push the image with the whole repository name
    docker push new-repo:tagname
    
    If you dont use the full repository name, you will get an error "requested access to resource is denied"
    
## Other useful docker commands

### To remove images use
 
    docker rmi -f [image_id]
    Note: if you are trying to remove a image and is in use by a container you will get an error like this
    image is being used by running container xxx
    so you will need to stop container first, see below
 
### Stop a container
    
    docker ps -a
    It will show all containers
    without -a will show only running containers
    get the container_id to stop
    
    docker stop [container_id]
 
 ### Pull you image to docker
 
    docker pull crhostservices/custom_nginx:latest
    Need to specify full repository
    Please note you need to make it public or login if it's private
    Go to run image example , to make it available on localhost.
    
## Deploy in Kube node

    you may need to loing in docker.
    kubectl run <YOUR_DEPLOYMENT_NAME> --image=docker.io/$docker_username/<YOUR_PUBLIC_REPO_IMAGE_NAME>:<TAG>

   ## Good repository for arm images - ARM is the processor of Pi, so images must comply with this standard
   https://github.com/alexellis/docker-arm/tree/master/images/armhf
    
 
   ### https://hub.docker.com/_/nginx
