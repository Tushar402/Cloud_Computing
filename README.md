# Cloud_Computing ( Web application Deployment to Google Cloud.)
## 1. Project Overview
For the initial deployment of the web application to Google Cloud, I used Docker and created a Docker container. The Docker container mainly helps to pack up the project code, Dependencies, and configuration from the local machine. By using Docker, the project runs smoothly and performs similarly on the cloud server as on the local machine, without any uncertainty occurring.

After creating a Docker container, I use Kubernetes. This is the main tool for the deployment. Kubernetes mainly provides an API to control how and where those Docker containers will run. It also allows running the Docker containers and workloads to tackle some of the operating complexities when moving to scale the containers and deployed them on the google cloud servers. The deployment covers both local and cloud Kubernetes clusters. 

Finally, with the help of Kubernetes, Successfully deployed the project to the Google Cloud platform.
## 2. System Architecture 
The backbone of the system architecture is the Kubernetes cluster, which deploys the Docker container to Google Cloud. I have used GitHub as a source control repository and Docker Hub as a container repository. First, I pull the git project from the local machine and then create a local Docker image using Docker from the spring boot application. Then push the local docker image (Backend) and docker image (Frontend) to the docker hub and then from the google cloud platform using the Google Kubernetes engine. I can run the docker image as a container in the Kubernetes cluster. The high-level system architecture is illustrated in Figure 1.
![Capture](https://github.com/Tushar402/Cloud_Computing/assets/26556525/9168ea8a-3cc8-4105-a9da-34b7d949af38)
## 3.	Deployment
