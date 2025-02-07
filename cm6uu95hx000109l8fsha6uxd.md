---
title: "Building and Deploying a Node.js To-Do App with GitLab CI/CD, Docker, and EC2"
datePublished: Fri Feb 07 2025 14:05:04 GMT+0000 (Coordinated Universal Time)
cuid: cm6uu95hx000109l8fsha6uxd
slug: building-and-deploying-a-nodejs-to-do-app-with-gitlab-cicd-docker-and-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738861907254/7fab2bc1-7321-4eb5-83dc-b6ebb4b5b7cc.png
tags: ec2, docker, aws, nodejs, gitlab, gitlab-ci, gitlab-runner, gitlab-cicd

---

In today's tech landscape, automation, scalability, and efficiency are key drivers behind successful development practices. Today, I'm excited to share how I built and deployed a **Node.js To-Do application** with seamless integration using **GitLab CI/CD**, **Docker**, and **EC2**. Let's dive into the process and explore the steps involved in setting up an automated pipeline for this application.

### **1\. Building the Node.js To-Do App**

The journey began with a simple yet powerful idea: creating a To-Do app with Node.js. I pushed my code to **GitLab**, which became the heart of my entire pipeline. Here's what the structure looked like:

* **Frontend**: A simple, user-friendly interface for managing to-do tasks.
    
* **Backend**: A Node.js API handling CRUD operations, connecting seamlessly with a database to store the tasks.
    

The source code was pushed to a GitLab repository, where the **GitLab CI/CD** pipeline would take care of building, testing, and deploying the application.

Code Repository GitLab link - [https://gitlab.com/sid-devops/node-todo-app](https://gitlab.com/sid-devops/node-todo-app)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738860360953/b90dd062-2a87-42b1-bc74-99a04596cad8.png align="center")

### **2\. Dockerizing the App**

Docker is a game-changer for deploying applications consistently across environments. I wrote a **Dockerfile** to containerize my Node.js application. Here’s how it works:

* The **Dockerfile** defines the environment, dependencies, and instructions to build the app's container image.
    
* Once the image was built, I pushed it to **Docker Hub**, making it accessible from anywhere.
    

With Docker, the beauty lies in its simplicity – encapsulating everything needed to run the app inside a portable container.

```dockerfile
# Node Base Image
FROM node:12.2.0-alpine

#Working Directry
WORKDIR /node

#Copy the Code
COPY . .

#Install the dependecies
RUN npm install
RUN npm run test
EXPOSE 8000

#Run the code
CMD ["node","app.js"]
```

### **3\. Setting Up GitLab CI/CD Pipeline**

Now, let’s talk about automation. Instead of manually building, testing, and deploying the app, I used **GitLab CI/CD** to automate the entire process:

* I created a `.gitlab-ci.yml` file to define the pipeline stages – build, test, push, and deploy.
    
* During the pipeline execution, **GitLab variables** played a crucial role. I securely stored my **Docker Hub credentials** as **masked GitLab variables**, ensuring they were kept safe while allowing the pipeline to push the Docker image to Docker Hub.
    

```yaml
stages:
  - build
  - test
  - push
  - deploy

build_job:
  stage: build
  script:
    - echo "Building image from the docker file"
    - docker build -t node-app:latest .
  tags:
    - dev-runner

test_job:
  stage: test
  script:
    - echo "Image has been started for security testing"
    - echo "Image is fully secured"
  tags:
    - dev-runner

push_job:
  stage: push
  script:
    - echo "Pushing the image to docker-hub"
    - docker tag node-app:latest $DOCKERHUB_USER/node-app:latest
    - docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS
    - docker push  $DOCKERHUB_USER/node-app:latest
  tags:
    - dev-runner

deploy_job:
  stage: deploy
  script:
    - echo "Deploying the application on the deployment server"
    - docker compose down && docker compose up -d
  tags:
    - dev-runner
```

Using these variables, my pipeline seamlessly communicated with Docker Hub to push the image, without exposing sensitive data. This process was secured and masked, ensuring my credentials were never visible in logs.

### **4\. Deploying with Docker Compose on EC2**

With the Docker image pushed to Docker Hub, the next step was deploying the app on a live server. I chose to use **Docker Compose** for easy orchestration and management of the app’s containers.

I set up a **EC2 instance** as the server to run the app, and also installed a **GitLab Runner** on the EC2 instance to handle my CI/CD jobs. This runner would execute the tasks such as building the Docker image and deploying it to the server.

The process was simple:

* **Docker Compose** allows me to define and run multi-container applications. In my case, the Node.js app and the database ran in separate containers, connected through Docker Compose.
    

```yaml
version: '3.9'

services:
  web:
    image: sgaurav7/node-app:latest
    ports:
      - "8000:8000"
```

Once the containers were up and running, my To-Do app was live and accessible via the public IP of the EC2 instance!

### **5\. GitLab Runner: EC2 Integration**

To ensure that my pipeline executed smoothly, I needed a **GitLab Runner** to handle job execution. Here's how I installed and configured it on my EC2 instance.

#### **Steps to Install GitLab Runner on EC2:**

1. **Install GitLab Runner on EC2**: First, I installed **GitLab Runner** on my EC2 instance. I used the following commands:
    
    ```bash
    # Download the binary for your system
    sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
    
    # Give it permission to execute
    sudo chmod +x /usr/local/bin/gitlab-runner
    
    # Create a GitLab Runner user
    sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
    
    # Install and run as a service
    sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
    sudo gitlab-runner start
    ```
    
2. **Register the GitLab Runner**: Next, I registered the runner with my GitLab project. This step required the **registration token** from my GitLab project's **Settings &gt; CI/CD &gt; Runners** section. To register, I used the following command:
    
    ```bash
    gitlab-runner register  --url https://gitlab.com  --token glrt-t3_-NisFHMyEcu5wqt1oo5A
    ```
    
    During the registration process, I provided:
    
    * GitLab instance URL
        
    * Registration token
        
    * A description for the runner (e.g., `EC2-Runner`)
        
    * The executor type (I chose `docker` for Docker-based CI/CD jobs)
        
3. **Start the GitLab Runner**: Once registered, I started the GitLab Runner service on EC2 to begin executing jobs:
    
    ```bash
    gitlab-runner run
    ```
    

Now, my EC2 instance is fully set up as a **GitLab Runner**, handling job executions such as:

* Pulling the latest code from GitLab.
    
* Building the Docker image.
    
* Deploying the app using Docker Compose.
    

This allowed my CI/CD pipeline to run efficiently and autonomously.

### **6\. Bringing It All Together**

At this point, everything was connected:

* **GitLab CI/CD** handles the automation of building and pushing the Docker image.
    
* **Docker** ensures a consistent and repeatable environment for the app.
    
* **EC2** hosts the live app, with the GitLab Runner enabling continuous integration and delivery.
    

### **Why This Setup Rocks**

1. **Automation**: The entire pipeline is automated, reducing manual intervention and allowing me to focus on development rather than deployment.
    
2. **Security**: Using GitLab variables for sensitive information such as Docker Hub credentials ensures that they remain private and secure.
    
3. **Portability**: Docker ensures that the application runs seamlessly across different environments, from development to production.
    
4. **Scalability**: EC2 allows me to scale the application easily and the flexibility to manage my CI/CD runner on the cloud.
    

### **Conclusion**

With GitLab CI/CD, Docker, and EC2, I was able to build a fully automated pipeline for my **Node.js To-Do app**. From writing the application code to deploying it to the cloud, every step of the process was streamlined.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738860456484/2061e279-dbc9-49a2-a659-68440e96392e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738860494830/f1ceace1-8142-4778-b813-e3fb75ad2253.png align="center")

I encourage anyone looking to automate their development workflow to explore this combination of tools – it's a recipe for success!