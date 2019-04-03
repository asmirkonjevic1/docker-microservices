The project consists of three simple services:

spring-boot-app - Java application with SpringBoot
node-app - Node.js application with Express
postgresql

// BUILD DOCKER IMAGES

Steps: 
  git clone https://github.com/kkenan/basic-microservices
  
Inside basic-microservices there are 2 directories: node-app and spring-boot-app.
Create new directory in basic-microservices: postgres.

So you will have spring-boot-app, node-app and newly created postgres directory.

To build Spring App cd into spring-boot-app folder: 
uname@ubuntu$:cd basic-microservices/spring-boot-app

Change application.yml file in basic-microservices/spring-boot-app/src/main/resources/.
uname@ubuntu:~/basic-microservices/spring-boot-app/src/main/resources/$ nano application.yml

Replace app.node.url: http://localhost:8081/ with app.node.url: http://node:8081/ so that Spring app can find Node App when we run Containers.
Do the same for url: jdbc:postgresql://localhost:5432/demodb ----> url: jdbc:postgresql://db:5432/demodb.
Change password: DemoPa$$ with password: demopass (I had authentication problems when using symbols in password).

Save application.yml file and go back to basic-microservices/spring-boot-app directory.
run mvn clean package and wait for application to build jar file. Jar file will be located in target directory ( basic-microservices/spring-boot-app/target/ ).

Copy Dockerfile-spring in basic-microservices/spring-boot-app/ and rename it to Dockerfile.
Copy Dockerfile-node in basic-microservices/node-app/ and rename it to Dockerfile.
Copy Dockerfile-postgres in basic-microservices/postgres/ and rename it to Dockerfile.

Go in each directory where Dockerfile is located and run: docker build -t name/docker-spring-boot:latest or replace with your own name.
This will build 3 docker images.
In my case it is akonjevic/docker-spring-boot:final, akonjevic/postgresql2 and akonjevic/docker-node:firsttry

// MY IMAGES ON DOCKER HUB

Location: 
-https://cloud.docker.com/repository/docker/akonjevic/docker-spring-boot
-https://cloud.docker.com/repository/docker/akonjevic/postgresql2
-https://cloud.docker.com/repository/docker/akonjevic/docker-node


// DOCKER COMPOSE

Now run docker-compose.yml: docker-compose up (make sure you are in the same directory as docker-compose.yml file)

After that there should be 3 containers able to talk with each other over virtual network that is specified in docker-compose.yml file.

To test if everything is working go to browser and type http://localhost:8080//java/api/v1/status or http://localhost/java/api/v1/node

If containers are running on cloud make sure to type public ip address of server.

You can use images specified above if you dont want to build your own. If you choose to build your own make sure that image names in docker-compose.yml are matching your images.




