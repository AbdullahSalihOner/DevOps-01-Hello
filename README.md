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
docker build  --build-arg JAR_FILE=target/DevOps-01-hello-1.0.1.jar   --tag    asoner01/devops-01-hello:v01 .
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
docker run -it -d -p 8083:8080 --name my-app3 asoner01/devops-01-hello:v01
docker run -it -d -p 8084:8080 --name my-app4 asoner01/devops-01-hello:v002
docker run -it -d -p 8085:8080 --name my-app5 asoner01/devops-01-hello:latest
```

## Pulling an Image from Docker Hub

To download a specific version of the image from Docker Hub, use the `docker pull` command as shown below. You can pull specific versions or the latest version of the image.

```bash
docker pull asoner01/devops-01-hello:v01
docker pull asoner01/devops-01-hello:v002
docker pull asoner01/devops-01-hello:latest
docker pull asoner01/devops-01-hello
```    

## Network

### Listing Networks

To list all existing Docker networks, use the following command:

```bash
docker network ls
```

### Changing Network Type with the --driver Parameter

To create a Docker network with a specific type, use the `--driver` parameter. For example, to create a network with the `host` driver, use the following command:

```bash
docker network create --driver host
```

### Network Information and Containers Using the Network

To view detailed information about a specific Docker network, including the containers connected to it, use the following command:

```bash
docker network inspect my-network
```

### Adding a Container to a Network

To connect a container to an existing Docker network, use the following `docker network connect` command. This example connects multiple containers to the `my-network` network.

```bash
docker network connect my-network my-app1
docker network connect my-network my-app2
docker network connect my-network my-app3
docker network connect my-network my-app4
```

### Network Information and Containers Using the Network

To view detailed information about a specific Docker network, including the containers connected to it, use the following command:

```bash
docker network inspect my-network
```

### Removing a Container from a Network

To disconnect a container from an existing Docker network, use the following `docker network disconnect` command. This example disconnects `my-app4` from the network.

```bash
docker network disconnect my-network my-app4
```

### Deleting a Network

To delete an existing Docker network, use the following command. This example removes the network named `my-network`.

```bash
docker network rm my-network
```


## Volume

### Listing Volumes

To list all existing Docker volumes, use the following command:

```bash
docker volume ls
```

### Creating a New Volume

To create a new Docker volume, use the following command:

```bash
docker volume create my-volume
docker volume ls
docker volume inspect my-volume
```

### Deleting a Volume

To delete an existing Docker volume, use the following command. This example removes the volume named `my-volume`.

```bash
docker volume rm my-volume
```

### Deleting All Unused Volumes

To delete all unused Docker volumes and free up space, use the following command:

```bash
docker volume prune
```

## Docker Compose

### Starting Services with Docker Compose

To start services defined in a `docker-compose.yml` file, use the following command:

```bash
docker compose -f docker-compose.yml up
docker ps
docker container ls
docker-compose logs mongo
docker-compose logs mongo
docker compose -f docker-compose.yml down
```

## docker-compose.yml Configuration

The following `docker-compose.yml` file sets up a MongoDB service and a Mongo Express interface for managing MongoDB through a web-based UI.

```yaml
# Use root/example as user/password credentials
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: 123456

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: 123456
      ME_CONFIG_MONGODB_URL: mongodb://root:123456@mongo:27017/
      ME_CONFIG_BASICAUTH: false
```

## ELK Stack Configuration with Docker Compose

The following `docker-compose2.yml` file sets up an ELK stack, which includes Elasticsearch, Logstash, and Kibana, for centralized logging and monitoring.

```yaml
version: '3'

networks:
  elk:

volumes:
  elasticsearch:
    driver: local

services:

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.2.1
    environment:
      http.host: 0.0.0.0
      transport.host: 127.0.0.1
    networks:
      elk: null
    ports:
      - 9200:9200
    restart: unless-stopped
    volumes:
      - elasticsearch:/usr/share/elasticsearch/data:rw

  logstash:
    image: docker.elastic.co/logstash/logstash-oss:6.2.1
    depends_on:
      - elasticsearch
    networks:
      elk: null
    ports:
      - 5044:5044
    restart: unless-stopped
    volumes:
      - ./etc/logstash/pipeline:/usr/share/logstash/pipeline:ro

  kibana:
    image: docker.elastic.co/kibana/kibana-oss:6.2.1
    depends_on:
      - elasticsearch
    environment:
      ELASTICSEARCH_URL: http://elasticsearch:9200
      ELASTICSEARCH_USERNAME: elastic
      ELASTICSEARCH_PASSWORD: changeme
    networks:
      elk: null
    ports:
      - 5601:5601
    restart: unless-stopped
```

## RabbitMQ Configuration with Docker Compose

The following `docker-compose3.yml` file sets up a RabbitMQ service with management features enabled for easier message queue management.

```yaml
version: "3.9"

services:

  rabbitmq:
    image: rabbitmq:3-management
    container_name: rabbitmq-1
    ports:
      - 5672:5672       # Port for RabbitMQ connections
      - 15672:15672     # Port for RabbitMQ management interface
    environment:
      - "RABBITMQ_DEFAULT_PASS=admin"
      - "RABBITMQ_DEFAULT_USER=admin"
      - "RABBITMQ_DEFAULT_VHOST='vhost'"
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq

volumes:
  rabbitmq_data:
```


## Starting Minikube

To start a Minikube cluster, use the following command:

```bash
minikube start
```

## Listing All Pods Across Namespaces

To view all running pods across all namespaces in your Kubernetes cluster, use the following command:

```bash
kubectl get po -A(cloud)

minikube kubectl -- get po -A(local)

```

## Accessing the Minikube Dashboard

To open the Kubernetes dashboard in Minikube, use the following command:

```bash
minikube dashboard
```
This command launches the Kubernetes dashboard in your default web browser, providing a graphical interface for monitoring and managing cluster resources.

The dashboard allows you to view, create, and manage Kubernetes resources such as deployments, services, and pods in a user-friendly way.


## Kubernetes

### Checking Kubernetes Version

To display the version of Kubernetes currently in use, both client and server, use the following command:

```bash
kubectl version
```

### Listing Pods in the Current Namespace

To view all running pods in the current namespace of your Kubernetes cluster, use the following command:

```bash
kubectl get pod
```

### Pulling an Image from Docker Hub and Running it as a Container

To pull an image from Docker Hub and run it as a container on your local machine, use the following command. This example pulls the image `asoner01/devops-01-hello:latest` and runs it as a container named `my-app5`:

```bash
docker run -it -d -p 8085:8080 --name my-app5 asoner01/devops-01-hello:latest
```


### Running Docker Hub Images as Containers in Kubernetes Pods

To deploy Docker Hub images as containers in Kubernetes pods, use the following `kubectl run` commands. Each command creates a pod with a specified image:

```bash
kubectl run my-pod1 --image=asoner01/devops-01-hello:latest
kubectl run my-pod2 --image=asoner01/devops-01-hello:v01
kubectl run my-pod3 --image=asoner01/devops-01-hello:v002
kubectl run my-pod4 --image=asoner01/devops-01-hello:v002
kubectl run my-pod5 --image=asoner01/devops-01-hello:latest
kubectl run my-pod6 --image=asoner01/devops-01-hello:latest
kubectl run my-pod7 --image=asoner01/devops-01-hello:v003
kubectl run my-pod8 --image=mysql
kubectl run my-pod9 --image=postgres
```


### Retrieving Kubernetes Nodes and Pods

To view information about nodes and pods in your Kubernetes cluster, use the following commands:

#### Viewing Nodes

To list all nodes in the cluster, you can use either of these commands:

```bash
kubectl get nodes
kubectl get node
```

These commands display the nodes in the cluster along with their statuses, roles, and other details.

### Viewing Pods

To list all pods in the current namespace, you can use either of these commands:

```bash
kubectl get pods
kubectl get pod
```

These commands provide a list of pods, showing their names, statuses, and other relevant details. To view pods in all namespaces, add the -A flag:
```bash
kubectl get pods -A
```

### Deleting a Pod in Kubernetes

To delete a specific pod in your Kubernetes cluster, use the following command. This example deletes the pod named `my-pod8`:

```bash
kubectl delete pod my-pod8
```

### Creating a Kubernetes Pod with a Manifest File

The following YAML configuration file defines a Kubernetes pod named `devops-01-hello`. This pod runs a container using the `asoner01/devops-01-hello` image.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: devops-01-hello
  labels:
    name: devops-01-hello
    type: backend
    app: hello-service
    project-name: mydemo
spec:
  containers:
    - name: devops-01-hello
      image: asoner01/devops-01-hello
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
      ports:
        - containerPort: 9090
```

Explanation:
Metadata: Specifies the pod name (devops-01-hello) and labels for categorizing and identifying the pod.
Container Configuration:
 - Image: Uses the asoner01/devops-01-hello image from Docker Hub.
 - Resources: Limits memory usage to 128Mi and CPU usage to 500m, helping to manage resource consumption within the cluster.
 - Ports: Exposes port 9090 within the container.

### Creating the Pod

To create this pod, save the YAML configuration as pod.yaml and run the following

```yaml
kubectl apply -f pod.yaml
```

### Creating Multiple Kubernetes Pods with a Manifest File

The following YAML configuration file defines two Kubernetes pods: `devops-01-hello` and `my-new-pod2`. Each pod runs a container with specific resource limits and exposed ports.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: devops-01-hello
  labels:
    name: devops-01-hello
    type: backend
    app: hello-service
    project-name: mydemo
spec:
  containers:
    - name: devops-01-hello
      image: asoner01/devops-01-hello
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
      ports:
        - containerPort: 9090
---
apiVersion: v1
kind: Pod
metadata:
  name: my-new-pod2
  labels:
    name: my-new-pod2
    type: frontend
    app: mydemo2-service
    project-name: mydemo2
spec:
  containers:
    - name: my-new-pod2
      image: asoner01/devops-01-hello:v001
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
      ports:
        - containerPort: 9091
```


### Creating a Kubernetes Deployment with a Manifest File

The following YAML configuration file defines a Kubernetes Deployment named `devops-01-hello`. This deployment manages a set of identical pod replicas for scalability and high availability.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: devops-01-hello
spec:
  replicas: 5
  selector:
    matchLabels:
      app: devops-01-hello
  template:
    metadata:
      labels:
        app: devops-01-hello
    spec:
      containers:
        - name: devops-01-hello
          image: asoner01/devops-01-hello:v001
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
          ports:
            - containerPort: 9090
```

### Creating a Kubernetes LoadBalancer Service with a Manifest File

The following YAML configuration file defines a Kubernetes `Service` of type `LoadBalancer`, which distributes traffic to the pods labeled `app: devops-01-hello`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: docker-loadbalancer
spec:
  selector:
    app: devops-01-hello
  type: LoadBalancer
  ports:
    - port: 8087
      targetPort: 8080
```