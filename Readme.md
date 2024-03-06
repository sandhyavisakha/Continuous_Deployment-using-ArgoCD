**# Continuous Deployment using GitOps tool, Argo CD and Release Management with Argo Rollouts**

**Project Summary:**

The project includes multiple important stages: First, configured the environment by creating a GitHub repository for the Django application and installing kubectl for management along with Minikube for a local Kubernetes cluster. The Django application was then Dockerized by making a Dockerfile, specifying requirements in requirements.txt, and submitting the Docker image to Docker Hub public registry. Next, integrated Argo CD with GitOps by setting it up on the Kubernetes cluster, defining the application in the Argo CD user interface, and approving automated deployments from the GitHub repository. Furthermore, used Argo Rollouts to implement Canary Release strategies. Defined rollout steps in the Kubernetes manifests of the Django app and tracked the progress of the canary release using the Argo CD UI.

**Implementation Steps:**

**1.	Setup and Configuration:**

•	Created a new repository on GitHub to host my application’s source code. For this assignment, I have used a Django web application that I have built before on my local machine. The same repository is used to store the Kubernetes manifests.
•	Installed minikube tool to run a single node Kubernetes cluster locally. Installed kubectl to interact with the Kubernetes cluster and manage resources. (In my case, I already had these two installed before for personal hands-on tasks).
•	Installed Argo CD on the Kubernetes cluster with the help of official documentation. This created a new namespace called ‘argocd’ for Argo CD components.
•	Installed Argo Rollouts on the Kubernetes cluster with the help of official documentation which created a new namespace called ‘argo-rollouts’ for Argo Rollouts components. This enables some advanced deployment strategies like canary releases.

**2.	Deploying Application using Argo CD:**

•	Built a Docker image locally using the docker build command, tagged it with a version v1.0, and then pushed the image to a public container registry, which in my case Docker Hub.
•	Created Kubernetes manifest files ‘deployment.yaml’,  ‘service.yaml’ and pushed them to the same Git repository where the application’s source code is stored.
•	Accessed the Argo CD UI for application management, created a new application for the Django app, and configured automatic deployments from the GitHub repository (configured repository link and the path for Kubernetes manifest files).
•	The application is synced correctly, and the application health is in healthy state.

**3.	Argo-Rollouts for Canary Deployment:**

•	Modified the application’s code (changed background color from blue to green) and built a new Docker image, tagged it with a version v2.0, and pushed the new image to Docker Hub.
•	Defined a canary release strategy in the Django app's Kubernetes manifests (replaced ‘deployment.yaml’ with ‘rollout.yaml’ file). Updated the rollout manifest to use the new version, v2.0 of Docker image. After making these changes, pushed the updates to the Git repository.
•	Monitored the canary release progress using Argo Rollouts plugin from the command line (This kubectl plugin was installed for easy management and visualization of rollouts). Observed the gradual rollout of the new version, ensuring that each canary step completed successfully.

**Clean Up:** 

•	Deleted the application on the Argo CD user interface.
•	Deleted the Argo CD installation from the cluster.
•	Deleted the Argo Rollouts installation from the cluster.
•	Deleted the Minikube cluster.

**Note:** I have kept the Docker images on the Docker Hub Registry.

**Challenges:**

I have tried to make use of ingress manifest to expose the application and test it from the browser, but after multiple modifications to the configuration file, failed to achieve healthy state of the application. Alternatively, I tried to expose one of the pods using port forwarding and tested the application on http://localhost:8000, before and after the Argo Rollout to see that the modification to the application reflects. Later, I figured out that I have missed to install ingress controller on the Kubernetes cluster.

**Learnings:**

By doing this project, I have learnt that the Canary release strategy provides a systematic way to roll out software updates. It makes it possible to closely monitor behavior and performance by progressively shifting a small portion of user traffic to the new version—incrementally increasing from 20% to 40%, 60%, 80%, and finally 100%. There are breaks in between each stage of this methodical procedure to allow for observation and evaluation. This approach reduces the possibility of bugs or other issues and makes it possible to identify issues early on. This facilitates a smoother transition and allows for well-informed decisions to be made before the product is fully rolled out to all users.
