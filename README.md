# Cloud_Computing
## Deploying Web application to Google Cloud.
For the initial deployment of the web application to Google Cloud, I used Docker and created a Docker container. The Docker container mainly helps to pack up the project code, Dependencies, and configuration from the local machine. By using Docker the project runs smoothly and performs similarly in the cloud server as in the local machine and without any uncertainty occurs.

After creating a docker container I use Kubernetes. This is the main tool for the deployment. Kubernetes mainly provides an API to control how and where those docker containers will run. It also allows running the Docker containers and workloads to tackle some of the operating complexities when moving to scale the containers and deployed them on the google cloud servers. The deployment covers both local and cloud Kubernetes clusters. 

Finally, with the help of Kubernetes, Successfully deployed the project to the Google cloud platform.
## System Architecture 
The backbone of the system architecture is the Kubernetes cluster, which deploys the Docker container to Google Cloud. I have used GitHub as source control repository, and docker hub as a container repository. First, I pull the git project from local machine and then create a local Docker image using Docker from the spring boot application. Then push the local docker image (Backend) and docker image (Frontend) to the docker hub and then from the google cloud platform using the Google Kubernetes engine. I am able to run the docker image as a container in the Kubernetes cluster. The high-level system architecture is illustrated in Figure 1.
