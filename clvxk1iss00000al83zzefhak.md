---
title: "Demystifying Docker Compose: A Beginner's Guide"
datePublished: Wed May 08 2024 08:25:45 GMT+0000 (Coordinated Universal Time)
cuid: clvxk1iss00000al83zzefhak
slug: demystifying-docker-compose-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715156520508/c431b5ea-a1af-4bfa-a183-4d4ea45762c6.jpeg
tags: docker, automation, devops, dockerfile, docker-compose, docker-images, docker-network, devopscommunity

---

In today's fast-paced world of software development and deployment, efficiency is key. Docker, with its containerization technology, has revolutionized the way applications are built, shipped, and run across various environments. Docker Compose, an essential tool in the Docker ecosystem, simplifies the management of multi-container Docker applications. In this beginner's guide, we'll delve into the basics of Docker Compose, breaking down the concepts in a non-technical manner.

### **What is Docker Compose?**

Imagine you're managing a complex web application consisting of multiple services, such as a web server, a database, and a caching layer. Coordinating these services manually can be cumbersome and error-prone. Docker Compose comes to the rescue by allowing you to define and manage multi-container Docker applications using a simple YAML file.

### **Key Concepts:**

#### **1\. Services:**

In Docker Compose, each containerized component of your application is referred to as a service. For instance, your web server, database, and caching layer would each be defined as separate services in your `compose.yml` file.

#### **2\. Dockerfile:**

A Dockerfile is a text file that contains instructions for building a Docker image. Docker Compose utilizes these Dockerfiles to build images for each service in your application.

**3\. compose.yml:**

This YAML file serves as the heart of your Docker Compose setup. It defines the services, networks, and volumes required by your application. Using a simple syntax, you specify parameters such as the Dockerfile path, container ports, environment variables, and more.

#### **4\. Volumes:**

Volumes in Docker Compose enable data persistence by mapping host directories or named volumes to paths within containers. This ensures that data generated or modified within containers persists even after the containers are stopped or removed.

### **Getting Started:**

#### **1\. Installation:**

Before diving into Docker Compose, ensure Docker Engine is installed on your system. Docker Compose typically comes bundled with Docker Desktop for Windows and macOS. For Linux systems, you may need to install it separately.

To check docker compose is installed or not, run below command on your linux system:

```bash
[root@sid-vm ~]# docker compose version
Docker Compose version v2.25.0
[root@sid-vm ~]#
```

#### **2\. Define your services:**

Create a new directory for your Docker Compose project and navigate into it. Then, create a `docker-compose.yml` file and define your services using the appropriate syntax. Here's a simple example we are picking flask-redis application from [https://github.com/sgaurav7/awesome-compose](https://github.com/sgaurav7/awesome-compose) git hub repository :

```dockerfile
[root@sid-vm ~]# git clone https://github.com/sgaurav7/awesome-compose.git
Cloning into 'awesome-compose'...
remote: Enumerating objects: 2319, done.
remote: Total 2319 (delta 0), reused 0 (delta 0), pack-reused 2319
Receiving objects: 100% (2319/2319), 5.92 MiB | 5.56 MiB/s, done.
Resolving deltas: 100% (1012/1012), done.
[root@sid-vm ~]# ls
anaconda-ks.cfg  awesome-compose  Docker-Zero-to-Hero  Java-apps  Java-apps_old  Jenkins-Zero-To-Hero  learning
[root@sid-vm ~]#
[root@sid-vm ~]# cd awesome-compose/flask-redis/
[root@sid-vm flask-redis]#
[root@sid-vm flask-redis]# ls
app.py  compose.yaml  Dockerfile  README.md  requirements.txt
[root@sid-vm flask-redis]#
[root@sid-vm flask-redis]#
```

We have app.py and requirements.txt for building the flask application.

```bash
[root@sid-vm flask-redis]# cat app.py
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    counter = str(redis.get('hits'),'utf-8')
    return "This webpage has been viewed "+counter+" time(s)"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000, debug=True)
[root@sid-vm flask-redis]#
[root@sid-vm flask-redis]# cat requirements.txt
flask
redis
[root@sid-vm flask-redis]#
```

To create image for the above application, we have below Dockerfile for the same:

```bash
[root@sid-vm flask-redis]# cat Dockerfile
# syntax=docker/dockerfile:1.4
FROM --platform=$BUILDPLATFORM python:3.10-alpine AS builder

WORKDIR /code

COPY requirements.txt /code
RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install -r requirements.txt

COPY . /code

ENTRYPOINT ["python3"]
CMD ["app.py"]

FROM builder as dev-envs

RUN <<EOF
apk update
apk add git bash
EOF

RUN <<EOF
addgroup -S docker
adduser -S --shell /bin/bash --ingroup docker vscode
EOF
# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
[root@sid-vm flask-redis]#
```

To bring up both redis and flask together in docker, we have compose.yaml file for the same.

```bash
[root@sid-vm flask-redis]# cat compose.yaml
services:
  redis:
    image: redislabs/redismod  #taking image here
    ports:
      - '6379:6379'
  web:
    build:
      context: .    # will build from the Docker file present in the current directory
      target: builder
    # flask requires SIGINT to stop gracefully
    # (default stop signal from Compose is SIGTERM)
    stop_signal: SIGINT
    ports:
      - '8000:8000'
    volumes:
      - .:/code
    depends_on:
      - redis
[root@sid-vm flask-redis]#
```

#### **3\. Build and run your application:**

Once you've defined your services, you can build and run your application using a single command `docker compose up -d`:

With -d flag, it will bring up the container in background. If you don't specify this flag, the containers will run in foreground shell.

```bash
[root@sid-vm flask-redis]# docker compose up -d
[+] Running 24/24
 ✔ redis 23 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                                                                                      101.1s
   ✔ 1fe172e4850f Pull complete                                                                                                                                                                            9.4s
   ✔ b37ded7c5478 Pull complete                                                                                                                                                                            6.2s
   ✔ 731f72c7a1a0 Pull complete                                                                                                                                                                            6.2s
   ✔ 34cfdf5ff0b4 Pull complete                                                                                                                                                                            7.9s
   ✔ f34dd688846e Pull complete                                                                                                                                                                            9.9s
   ✔ 6c5b270e591b Pull complete                                                                                                                                                                            9.4s
   ✔ 85e4511d5c0c Pull complete                                                                                                                                                                           13.3s
   ✔ 8ae042d0aa02 Pull complete                                                                                                                                                                           10.9s
   ✔ 8f36acf34341 Pull complete                                                                                                                                                                           11.1s
   ✔ 4f4fb700ef54 Pull complete                                                                                                                                                                           12.3s
   ✔ 6de9a4176384 Pull complete                                                                                                                                                                           12.9s
   ✔ d3e57aaad013 Pull complete                                                                                                                                                                           14.5s
   ✔ 8eb23391b30a Pull complete                                                                                                                                                                           17.6s
   ✔ 93afd04540e1 Pull complete                                                                                                                                                                           14.5s
   ✔ fbab70c08f99 Pull complete                                                                                                                                                                           16.3s
   ✔ d5a580d6eb66 Pull complete                                                                                                                                                                           44.1s
   ✔ a78faad883d5 Pull complete                                                                                                                                                                           18.5s
   ✔ 414b4d29d83b Pull complete                                                                                                                                                                           22.0s
   ✔ 41b72fc2f30c Pull complete                                                                                                                                                                           20.7s
   ✔ ae9c2ce92fb1 Pull complete                                                                                                                                                                           23.0s
   ✔ aeb01ec80daf Pull complete                                                                                                                                                                           24.0s
   ✔ b90ccc8ea95b Pull complete                                                                                                                                                                           24.5s
   ✔ 0e6204cd32ab Pull complete                                                                                                                                                                           51.4s
[+] Running 2/3
 ⠴ Network flask-redis_default    Created                                                                                                                                                                  2.5s
 ✔ Container flask-redis-redis-1  Started                                                                                                                                                                  1.4s
 ✔ Container flask-redis-web-1    Started                                                                                                                                                                  1.9s
[root@sid-vm flask-redis]#
```

If you notice above clearly, it created a custom network name flask-redis\_default network which is of driver type bridge. This network and the containers configured from this network will be isolate from all the other containers present on the host system.

```bash
[root@sid-vm flask-redis]# docker network ls
NETWORK ID     NAME                  DRIVER    SCOPE
cecd65813a07   bridge                bridge    local
1034a2c0e2a2   flask-redis_default   bridge    local  #This is created
3871aa9026f1   host                  host      local
73e1f3862799   none                  null      local
[root@sid-vm flask-redis]#
```

Docker Compose will start the defined services, build Docker images as necessary, and orchestrate their communication.

Let's see if our containers are up and running or not:

```bash
[root@sid-vm flask-redis]# docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS         PORTS                                       NAMES
51558ec559b4   flask-redis-web      "python3 app.py"         2 minutes ago   Up 2 minutes   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   flask-redis-web-1
cdd2ce0f8a53   redislabs/redismod   "redis-server --load…"   2 minutes ago   Up 2 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   flask-redis-redis-1
[root@sid-vm flask-redis]#
```

#### **4\. Interact with your application:**

With your application up and running, you can interact with it through your web browser or command line tools. Docker Compose handles the networking aspect, ensuring seamless communication between services.

Our flask-redis-web container 8000 port is mapped with our host machine port 8000.

Try to access, http://your-host-ip:8000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715156153284/10c5f39a-0ace-48bd-8284-95e08d4aacea.png align="center")

Hurray! our application is responding.

#### **5\. Cleanup:**

After you're done experimenting, you can stop and remove the containers using:

```bash
[root@sid-vm flask-redis]# docker compose down
[+] Running 3/3
 ✔ Container flask-redis-web-1    Removed                                                                                                                                                                  0.8s
 ✔ Container flask-redis-redis-1  Removed                                                                                                                                                                  0.5s
 ✔ Network flask-redis_default    Removed                                                                                                                                                                  0.3s
[root@sid-vm flask-redis]#
```

This cleans up the resources allocated by Docker Compose, ensuring a tidy development environment.

### **Conclusion:**

Docker Compose simplifies the management of multi-container Docker applications, making it an indispensable tool for developers and DevOps engineers alike. By defining your application's architecture in a single YAML file, you gain greater control and flexibility in deploying and scaling your services. With this beginner's guide, you're now equipped to dive into the world of Docker Compose with confidence.

Happy containerizing!