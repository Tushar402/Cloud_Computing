# Cloud_Computing ( Web application Deployment to Google Cloud)
## 1. Project Overview
For the initial deployment of the web application to Google Cloud, I used Docker and created a Docker container. The Docker container mainly helps to pack up the project code, Dependencies, and configuration from the local machine. By using Docker, the project runs smoothly and performs similarly on the cloud server as on the local machine, without any uncertainty occurring.

After creating a Docker container, I use Kubernetes. This is the main tool for the deployment. Kubernetes mainly provides an API to control how and where those Docker containers will run. It also allows running the Docker containers and workloads to tackle some of the operating complexities when moving to scale the containers and deployed them on the google cloud servers. The deployment covers both local and cloud Kubernetes clusters. 

Finally, with the help of Kubernetes, Successfully deployed the project to the Google Cloud platform.
## 2. System Architecture 
The backbone of the system architecture is the Kubernetes cluster, which deploys the Docker container to Google Cloud. I have used GitHub as a source control repository and Docker Hub as a container repository. First, I pull the git project from the local machine and then create a local Docker image using Docker from the spring boot application. Then push the local docker image (Backend) and docker image (Frontend) to the docker hub and then from the google cloud platform using the Google Kubernetes engine. I can run the docker image as a container in the Kubernetes cluster. The high-level system architecture is illustrated in Figure 1.
![Capture](https://github.com/Tushar402/Cloud_Computing/assets/26556525/9168ea8a-3cc8-4105-a9da-34b7d949af38)
## 3.	Deployment
### Docker
Docker uses a client-server architecture. The Docker client connects to the Docker host, which can
build, run, and distribute Docker containers. The Docker client and Docker host can run on the
same system. Then it can easily push to the docker hub. Docker is free and open-source
platform for creating, delivering, and operating applications. Docker allows to separate the
application from the infrastructure which allows to manage the infrastructure in the same
manner that I can control the applications. It helps to minimize the time between writing
code and executing it by utilizing Docker's methodology for fast implementation, testing, and
deploying code for the project.
![Docker](https://github.com/Tushar402/Cloud_Computing/assets/26556525/e023221d-a1f8-47e9-ab37-763bd6bcda47)
#### Docker Image Create (Backend)
1. First install Docker on the local machine. Open the project folder on the terminal and run the following
commands on the terminal for creating the docker image.
```bash
  mvnw spring-boot:build-image
```
2. After creating the docker image we check the images of our project by running the following
command.
```bash
  docker images
```
3. Now we create a Docker hub account to push the local Docker image into the Docker hub. Login
to the docker hub by providing credentials using the following commands.
```bash
docker login
```
4. Now we need to tag a docker image before we push it into the docker hub. For this, we need the ID
of our docker image give a name and tag it by 5 by using the following command.
```bash
docker tag 4652b5678ce1 tonmoy002/ppmtapi:5
```
5. Now we push our docker image into the docker hub using the tag by the following command.
```bash
docker push tonmoy002/ppmtapi:5
```
6. We can see that our docker image was successfully uploaded to our docker hub account and no
issue occurs.

<img width="1432" alt="dckr1" src="https://github.com/Tushar402/Cloud_Computing/assets/26556525/45d1d1fb-ec97-4da1-b142-dff405c51bd0">

#### Google Cloud Account
1. Create google cloud account.
2. Created two new projects on Google cloud named PPMT and PPMT-CLIENT
respectively.
3. Created Database instance in PPMT project named ppmtdb and created database name:
ppmt

#### Kubernetes
Kubernetes is a portable and scalable open-source platform which manage the container-based
applications and services. It is a container orchestration system that organizes computer, network,
and storage infrastructure. The master and node components are the most important parts of
Kubernetes. The Kubernetes cluster is controlled and managed by the master node, which also
comprises a set of worker nodes. As the smallest deployable unit on the nodes, the system
orchestrates so-called pods. One or more containers are contained in each pod. The master node's storage and network resources are shared by these containers. Each container has its own set of
instructions about how to execute it. They have a unique IP address that they use to access the
pod. However, each time a pod fails, a new one must be produced, along with a new IP address which is shown in the figure.

![kubernetes1](https://github.com/Tushar402/Cloud_Computing/assets/26556525/a308dd25-5444-48ea-a70d-a83628e64809)

#### Kubernetes Implementation
1. We already have the docker image in the docker hub. Now we want to run our application as a
pod in Kubernetes. For this, we first create a dummy project named PPMT on google cloud and
open cloud shell. This cloud shell allows us to create resources and interact using this shell. First
we check the configuration list by using the command.

```bash
gcloud config list

```
 and see we are assigned and we have a project.
 
2. Now we need to set our zone before we create our cluster for Kubernetes. To do that we have
to use the command.

```bash
gcloud config set compute/zone us-centrall-a

```
3. Now we are ready to create our container cluster. For creating the container cluster, we use the command and set the number of nodes is 1. Because our project is small so one node is enough for the project.

```bash
gcloud container clusters my-cluster –num-nodes=1

```
4. Now our cluster is ready, and we want to deploy our project to this cluster. We are now
able to get the credentials for interacting with this cluster using kubectl. To do this we use
the command below.

```bash
gcloud container clusters get-credentials my-cluster

```
5. Now we can use kubeclt and want to create a deployment using the image that we created. For
creating deployment of the web server, we use the following command

```bash
kubectl create deployment web-server --image=docker.io/tonmoy002/ppmtapi:5
```
6. Now we can get the deployment information with the following command:
```bash
kubectl get deployments
```

7. For checking the pods status we use the following command.

```bash
kubectl get pods
```
8.Now we check the spring boot logs status by using the below command.

```bash
kubectl logs web-server-75b8b9d457-spqnt
```
9. After successfully running the application, it will show a status.

![kubr2](https://github.com/Tushar402/Cloud_Computing/assets/26556525/9628eb83-90a7-4bf1-9783-d014e7e6a5fd)

10. We already have the pods, but this pod cannot be accessed from the public internet. So, we
need to expose it using services. So, we create a service using kubectl using the expose
deployment command and the name of the deployment, which is web server and type, that is Load
balancer, and after that, we need to map our ports. Here we put our port 8080 and also our targeted
port 8080.

```bash
kubectl expose deployment web-server --type LoadBalancer --port 8080 --target-port 8080

```
11. We use the following command for the external IP. 

```bash
kubectl get services -w
```
Below the screenshot, we can see that our external IP is generated.

![kubernetes3](https://github.com/Tushar402/Cloud_Computing/assets/26556525/05467ac1-fe88-41eb-90bd-ebf87b017f4b)

By using this external IP address, we can see that our pod is running our application which has a
controller that returns the services of the application.

#### Frontend Deployment
Now we will deploy the react application for user interaction using docker.

1. First create a Docker file without any extension and add these commands to Docker
file:

![frnt1](https://github.com/Tushar402/Cloud_Computing/assets/26556525/0c04772c-c0ee-4036-883c-bb1523f59519)

This command will initiate node package and install necessary packages from
package.json file.

2. After the necessary package installation, we build an image file using the command in
docker:


```bash
docker build -t tonmoy002/ppmtclient
```

3. We Check the Docker images were created successfully with the following command:
```bash
docker images
```

4. Now we need the image tag ID to push the Docker image into the Docker hub. By using the
commend we get the tag id.
```bash
docker tag a34a1920b06c tonmoy002/ppmtclient:3
```
5. Now we have the tag id and need to push it to docker hub. To upload our docker image
we use the ‘docker push’ command.
```bash
docker push tonmoy002/ppmtclient:3
```

Now our docker image is ready to deploy to google cloud.
<img width="1436" alt="dckr2f" src="https://github.com/Tushar402/Cloud_Computing/assets/26556525/7e140420-1858-4077-ba93-57a97e92cd68">

Initially, we created a dummy Google Cloud project and named it PPMT-CLIENT. Open the cloud
shell and set up the basic needs:

1. First, we opened the cloud shell and by the following command, we
Check the configuration list.
```bash
gcloud config list
```
2. Now we have to update the server zone before creating the Kubernetes cluster same as before. For this
we use the following command.
```bash
gcloud config set computer/zone us-centrall-a
```

3. After updating the server zone, we enable the Kubernetes API engine from that particular
project. Then we run the following command to create the cluster.
```bash
gcloud container clusters create my-cluster –-num-nodes=1

```
4. Now we need the credential of the cluster to deploy it on google cloud. By using the
the following command, we get the cluster credentials.

```bash
gcloud container clusters get-credentials my-cluster

```
5. Now our cluster is ready for deployment using Kubectl. So, we use the command.
```bash
kubectl create deployment web-server –image=docker.io/tonmoy002/ppmtclient:3 
```

After that, we can see that our deployment is created successfully.

6. For checking the deployment status, we use **“kubectl get deployments”** command and
see the credentials.
7. Now we use **“kubectl get pods”** to get the pod credentials which is needed to check the
logs.
8. We already have the pod name so we can easily check the react log status using
**“kubectl logs”**

9. We have the pods, but this pod cannot be accessed from the public internet. So, we need
to expose it using services. So, we create a service using kubectl using the expose deployment
command and the name of the deployment which is webserver and type that is Load Balancer
after that we need to map our ports. Here we put our port 3000 and also our targeted port is 3000.

```bash
kubectl expose deployment web-server --type LoadBalancer --port 3000 --target-port 3000

```

10. Now we successfully deployed our frontend to google cloud. For further use we need the
external IP. For this we use **“kubectl get services -w”** command and our external IP is created.

![kubr3](https://github.com/Tushar402/Cloud_Computing/assets/26556525/04e5b6a7-58b5-4c2c-b7a8-e2b5c42286fa)

From the screenshot we get our external IP 35.224.212.242, and our port is 3000. So now anyone can
easily get our services by using the IP address and port. Here is our final IP address.

11. Form the google cloud account We can see the overview status of our Kubernetes cluster.
<img width="1437" alt="status" src="https://github.com/Tushar402/Cloud_Computing/assets/26556525/2c8ab66c-eb9a-4d17-923d-206611e29710">

12. From the cluster, we can see the Node status. We have only one node because our project is small.
<img width="1438" alt="pods" src="https://github.com/Tushar402/Cloud_Computing/assets/26556525/889b1956-1940-4263-be88-0f1a8e5cda38">

#### Final Outcome
Finally, we are able to deploy our application on google cloud platform (GCP) using dockers and
Kubernetes. We further went ahead to integrate with Docker and Kubernetes. We did validations and performance testing to check our deployment. The test results were tremendous as services could be redirected by our gateway.

<img width="1434" alt="final" src="https://github.com/Tushar402/Cloud_Computing/assets/26556525/4e66c6fc-bd2f-492d-942d-7acd76b4ff6e">

To summarize, This readme file explored docker container deployment using Kubernetes cluster to Google Cloud
Platform (GCP) and our methodologies for doing so. It contains detailed instructions for
configuring and installing the required tools and services. Finally, we went over the deployment,
testing methods and reviewed the results of the deployments.
