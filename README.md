# DevOps-01-Hello

Welcome to **DevOps-01-Hello**! This repository demonstrates essential Docker commands, explaining how to containerize and manage applications in a DevOps environment.

## Prerequisites

- **Docker** installed on your local machine
- Basic understanding of Docker concepts and commands

## Getting Started

Follow the steps below to explore Docker commands and learn how to work with containers effectively.

---

# Docker

## Docker Login

You can log in to Docker with the following commands:

```bash
docker login --username asoner --password 123456789
docker login -u asoner -p 123456789
```

## Nginx

To start an Nginx container, use the following command. This command maps external port `9991` to internal port `80`.

```bash
docker run -it -d -p 9991:80 --name my-nginx nginx    
```
After starting Nginx, you can access it at: http://localhost:9991

## Postgres

To start a Postgres container, use the following command. This command maps external port `9999` to internal port `5432` and sets the `POSTGRES_PASSWORD`.

```bash
docker run --name my-postgres -p 9999:5432 -e POSTGRES_PASSWORD=123456789 -d postgres
```

## MySQL
To start a MySQL container, use the following command. 
This command maps external port 9990 to internal port 3306 and sets the MYSQL_ROOT_PASSWORD.

```bash
docker run --name my-mysql -p 9990:3306 -e MYSQL_ROOT_PASSWORD=123456789 -d mysql
```

## Docker Configuration for the Application

To run the application, JDK is required.

### Dockerfile Configuration

Below is an example of a Dockerfile configuration to set up the application.

```dockerfile
# Specify the JDK version required for the application
# Using Amazon Corretto JDK version 17 or OpenJDK 17
# FROM amazoncorretto:17
FROM openjdk:17

# Define the location of the project's JAR file
ARG JAR_FILE=target/*.jar

# Copy the JAR file into the Docker container with a specified name
COPY ${JAR_FILE} devops-hello-app.jar

# Use CMD to run any commands needed during setup
CMD apt-get update
CMD apt-get upgrade -y

# Expose port 8080 to fix the internal port
EXPOSE 8080

# Command to run the application
ENTRYPOINT ["java","-jar","devops-hello-app.jar"]
```

## Converting Our Project into a Docker Image

To build a Docker image of the project, use the following command. This command specifies the path to the JAR file and tags the image with a name and version.

```bash
docker build  --build-arg JAR_FILE=target/DevOps-01-hello-1.0.1.jar   --tag    asoner01/devops-01-hello:v001 .
docker build  --build-arg JAR_FILE=target/DevOps-01-hello-1.0.2.jar   --tag    asoner01/devops-01-hello:v002 .
docker build  --build-arg JAR_FILE=target/DevOps-01-hello-1.0.2.jar   --tag    asoner01/devops-01-hello:latest .
```

## Renaming a Docker Container

To rename an existing Docker container, use the following command. This example renames a container from `my-app5` to `my-app1`.

```bash
docker container rename my-app5 my-app1
```


## Running Containers from Our Docker Image

To create and run containers from the Docker image, use the following commands. Each command runs a container with a specified name and maps a unique external port to internal port `8080`.

```bash
docker run -it -d -p 8081:8080 --name my-app1 asoner01/devops-01-hello
docker run -it -d -p 8082:8080 --name my-app2 asoner01/devops-01-hello
docker run -it -d -p 8083:8080 --name my-app3 asoner01/devops-01-hello:v001
docker run -it -d -p 8084:8080 --name my-app4 asoner01/devops-01-hello:v002
docker run -it -d -p 8085:8080 --name my-app5 asoner01/devops-01-hello:latest
```

## Pulling an Image from Docker Hub

To download a specific version of the image from Docker Hub, use the `docker pull` command as shown below. You can pull specific versions or the latest version of the image.

```bash
docker pull asoner01/devops-01-hello:v001
docker pull asoner01/devops-01-hello:v002
docker pull asoner01/devops-01-hello:latest
docker pull asoner01/devops-01-hello
```    


