# nginx-custom-image
How to build a custom nginx for a static web site

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

    sudo docker build -t my-nginx:v1.0 .
    Note the point at the end is important dont delete
    
   https://help.insight.com/app/answers/detail/a_id/121/~/getting-started-with-docker-part-2%3A-building-images-and-docker-compose
   
   
