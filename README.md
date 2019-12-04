# nginx-custom-image
How to build a custom nginx for a static web site

Install docker on Ubuntu 19.04
    
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu disco stable test"
    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io
    sudo adduser $(whoami) docker 

Create a folder for this project

Create "Dockerfile" file

    FROM ubuntu:xenial
    RUN apt update && apt install -y nginx
    EXPOSE 80
    COPY . /var/www/html
    CMD ["nginx", "-g", "daemon off;"]
    
Create your index.html file, this must be in the same directory than the Dockerfile

    <html>
    <head>
      <title>Raspberry Kubernetes</title>
    </head>
    <body>
      <h1>My static web site running on Raspberry Kubernetes</h1>
    </body>
    </html>
    
Build the image

    docker build -t <replace by your image name> .
    ie: docker build -t some-content-nginx .
    Note the point at the end is important dont delete
    
List image
    
    docker images
    You should be able to see you image listed.
    REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
    my-nginx             v1.0                4e3c3cfc6dbf        12 minutes ago      205MB
    some-content-nginx   latest              4e3c3cfc6dbf        12 minutes ago      205MB
    ubuntu               xenial              56bab49eef2e        7 days ago          123MB

Run the image

    docker run --name some-nginx -d -p 8080:80 some-content-nginx
    Note the name of the image is at the end.
    
See running containers
    docker ps -a
    Use name for docker stop [container name
    
To remove images use : docker rmi -f [image_id]

See it...
    go to http://localhost:8080/
    and you should be able to see the index.html content displayed
    
Push image to hub repository
- Create a repository

Login to docker hub
docker login -u "crhostservices" -p "o3&l49%1n8y2" docker.io
docker login --username=yourhubusername  , it will ask for the password


docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
    https://forums.docker.com/t/docker-push-error-requested-access-to-the-resource-is-denied/64468
    
    https://ropenscilabs.github.io/r-docker-tutorial/04-Dockerhub.html
    
   https://help.insight.com/app/answers/detail/a_id/121/~/getting-started-with-docker-part-2%3A-building-images-and-docker-compose
https://stackoverflow.com/questions/28349392/how-to-push-a-docker-image-to-a-private-repository

   https://hub.docker.com/_/nginx
