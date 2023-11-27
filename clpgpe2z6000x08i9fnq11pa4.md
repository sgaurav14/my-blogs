---
title: "Deployment of two-tier application with Docker Compose"
datePublished: Mon Nov 27 2023 09:25:10 GMT+0000 (Coordinated Universal Time)
cuid: clpgpe2z6000x08i9fnq11pa4
slug: deployment-of-two-tier-application-with-docker-compose
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701077039428/fdf46d93-17db-4db1-81fb-8626c49758cd.webp
tags: devops, dockerfile, docker-compose, docker-images, 90daysofdevops

---

We learned in our last blog ([https://sgaurav.hashnode.dev/deployment-of-two-tier-application-with-docker-network](https://sgaurav.hashnode.dev/deployment-of-two-tier-application-with-docker-network)) how to set up a two-tier application using Docker. In that, we ran each container separately.

Now, in this blog, we'll make things easier by automating the deployment of these containers using something called Docker Compose.

### What is Docker Compose?

Docker Compose is like a helper for your Docker containers. Imagine your application is made up of different parts, like databases, web servers, and other services. Docker Compose lets you describe all these parts and how they should work together in a special file called `docker-compose.yml`.

So, instead of managing each piece separately, you use Docker Compose to start everything with just one command. It's like a handy way to organize and launch all the parts of your application at once. This is especially useful when your project gets more complicated with various components.

In a nutshell, Docker Compose makes it easier to handle and run your entire application, simplifying things for you and your team.

### Deploying two-tier application using docker-compose

Let's kick off this project! Working with Docker Compose is going to be a lot of fun.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701007615085/1b8194cb-9dc6-4726-b4f4-62c33bd61fc3.png align="left")

So, let's begin and deploy our first two-tier application using docker-compose -

1. Clone the Git repository using the following command in your terminal or command prompt:
    
    ```bash
     ubuntu@ip-172-31-47-90:~/projects$ git clone https://github.com/sgaurav7/two-tier-flask-app.git
     Cloning into 'two-tier-flask-app'...
     remote: Enumerating objects: 140, done.
     remote: Counting objects: 100% (77/77), done.
     remote: Compressing objects: 100% (38/38), done.
     remote: Total 140 (delta 62), reused 39 (delta 39), pack-reused 63
     Receiving objects: 100% (140/140), 35.15 KiB | 3.51 MiB/s, done.
     Resolving deltas: 100% (69/69), done.
     ubuntu@ip-172-31-47-90:~/projects$ 
     ubuntu@ip-172-31-47-90:~/projects$ ls
     two-tier-flask-app
     ubuntu@ip-172-31-47-90:~/projects$
    ```
    
2. Navigate to the 'two-tier-flask-app' directory using the 'cd' command in your terminal or command prompt:
    
    ```bash
     ubuntu@ip-172-31-47-90:~/projects$ cd two-tier-flask-app/
     ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
     ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ ls
     Dockerfile  Jenkinsfile  README.md  app.py  docker-compose.yml  eks-manifests  k8s  message.sql  requirements.txt  templates
     ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
     ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$
    ```
    
3. Let's create a dockerfile for our application.
    
    ```bash
    
     # Use an official Python runtime as the base image
     FROM python:3.9-slim
    
     # Set the working directory in the container
     WORKDIR /app
    
     # install required packages for system
     RUN apt-get update \
         && apt-get upgrade -y \
         && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
         && rm -rf /var/lib/apt/lists/*
    
     # Copy the requirements file into the container
     COPY requirements.txt .
    
     # Install app dependencies
     RUN pip install mysqlclient
     RUN pip install --no-cache-dir -r requirements.txt
    
     # Copy the rest of the application code
     COPY . .
    
     # Specify the command to run your application
     CMD ["python", "app.py"]
     
    ```
    
4. Now, this Docker file is designed for our Python Flask app. We need to build this Dockerfile and run the container from that image. Additionally, this app requires a MySQL container, which we created separately in our last blog.
    
    To simplify the process, let's create a single file that includes building the above Docker file into an image and running that image as a Flask app container alongside the MySQL container. This file is known as `docker-compose.yml`.
    
    Make sure the naming convention for the Docker Compose file is the same and has the extension .yml (which stands for Yet Another Markup Language).
    
    Let's proceed to create our `docker-compose.yml` file for our application -
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ ls
    Dockerfile  Jenkinsfile  README.md  app.py  docker-compose.yml  eks-manifests  k8s  message.sql  requirements.txt  templates
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ cat docker-compose.yml 
    version: '3'
    services:
      backend:
        build:
          context: .
        ports:
          - '5000:5000'
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: admin
          MYSQL_PASSWORD: admin
          MYSQL_DB: testdb
        depends_on:
          - mysql
    
      mysql:
        image: mysql:5.7
        ports:
          - '3306:3306'
        environment:
          MYSQL_ROOT_PASSWORD: admin
          MYSQL_DATABASE: testdb
          MYSQL_USER: admin
          MYSQL_PASSWORD: admin
        volumes:
          - ./message.sql:/docker-entrypoint-initdb.d/message.sql   # Mount sql script into container's /docker-entrypoint-initdb.d directory to get table automatically created
          - mysql-data:/var/lib/mysql  # Mount the volume for MySQL data storage
    
    volumes:
      mysql-data:
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ```
    
5. So, we now have our `docker-compose.yml` file ready. Let's bring up the containers mentioned in the file. To do this, run the command `docker-compose up -d`. This command will start the containers in the background (`-d` flag).
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker-compose up -d
    Creating network "two-tier-flask-app_default" with the default driver
    Pulling mysql (mysql:5.7)...
    5.7: Pulling from library/mysql
    11a38aebcb7a: Pull complete
    91ab01309bd6: Pull complete
    6c91fabb88c2: Pull complete
    8f46e806ab5c: Pull complete
    29f5af1d1661: Pull complete
    62aca7179a54: Pull complete
    85023e6de3be: Pull complete
    6d5934a87cbb: Pull complete
    c878502d3f70: Pull complete
    4756467c684a: Pull complete
    ee9043dd2677: Pull complete
    Digest: sha256:f566819f2eee3a60cf5ea6c8b7d1bfc9de62e34268bf62dc34870c4fca8a85d1
    Status: Downloaded newer image for mysql:5.7
    Building backend
    DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                Install the buildx component to build images with BuildKit:
                https://docs.docker.com/go/buildx/
    
    Sending build context to Docker daemon  134.1kB
    Step 1/8 : FROM python:3.9-slim
     ---> db813260829a
    Step 2/8 : WORKDIR /app
     ---> Using cache
     ---> 004aeb1be581
    Step 3/8 : RUN apt-get update     && apt-get upgrade -y     && apt-get install -y gcc default-libmysqlclient-dev pkg-config     && rm -rf /var/lib/apt/lists/*
     ---> Using cache
     ---> 171b973ab65c
    Step 4/8 : COPY requirements.txt .
     ---> Using cache
     ---> 84d75450e9a2
    Step 5/8 : RUN pip install mysqlclient
     ---> Using cache
     ---> 56ed5de0d590
    Step 6/8 : RUN pip install --no-cache-dir -r requirements.txt
     ---> Using cache
     ---> 01f67a9ffdd2
    Step 7/8 : COPY . .
     ---> 4265d283668b
    Step 8/8 : CMD ["python", "app.py"]
     ---> Running in bbc968a8983b
    Removing intermediate container bbc968a8983b
     ---> b91d26d19667
    Successfully built b91d26d19667
    Successfully tagged two-tier-flask-app_backend:latest
    WARNING: Image for service backend was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
    Creating two-tier-flask-app_mysql_1 ... done
    Creating two-tier-flask-app_backend_1 ... done
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ```
    
    In the command mentioned above (`docker-compose up -d`), it not only builds and runs the image for the Flask app but also pulls the MySQL image from Docker Hub and runs it as a container. Additionally, with the help of this file, we've set up a volume that is linked to MySQL. This volume ensures that data persists even if the MySQL container crashes or stops unexpectedly.
    
    It's worth noting that we ran the MySQL script to create a table in the `testdb` database directly from this file. This is a significant improvement because, in our previous blog, we had to log in to the MySQL container and manually create the table.
    
6. Now, let's check if both the Python Flask app and MySQL containers are up and running:
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker ps
    CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                                  NAMES
    d4ac861c51ac   two-tier-flask-app_backend   "python app.py"          8 minutes ago   Up 8 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp              two-tier-flask-app_backend_1
    a7661432edde   mysql:5.7                    "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   two-tier-flask-app_mysql_1
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ```
    
7. Hurray! Both containers are up and running, indicating that our Python Flask app should be accessible. Let's validate this by opening the URL [http://your-ip:5000](http://your-ip:5000/).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701076644368/83e41ed1-03c5-408c-8189-e5ea36eaa79e.png align="center")
    
    And there we have it – our application is up and running! With the help of the Docker Compose file, we've successfully automated the entire manual process of deploying containers.
    

### Conclusion

In conclusion, we've explored the power of Docker Compose in streamlining the deployment of our two-tier application. By creating a simple yet powerful `docker-compose.yml` file, we automated the process of building and running both the Python Flask app and MySQL containers. This not only simplified the deployment but also ensured consistency across different environments.

The ability to manage multiple services, networks, and volumes in a single configuration file makes Docker Compose an invaluable tool for developers. It eliminates the need for manual intervention, providing a seamless and reproducible deployment process.

As we've seen, the automation achieved through Docker Compose significantly improves efficiency, allowing us to focus more on our application logic rather than the intricacies of container orchestration. With this newfound simplicity, deploying complex applications becomes more accessible, making Docker Compose a key asset in the world of containerization.