---
title: "Deployment of two-tier application with Docker Network"
datePublished: Sun Nov 26 2023 14:24:49 GMT+0000 (Coordinated Universal Time)
cuid: clpfknktk000409lgggx4630l
slug: deployment-of-two-tier-application-with-docker-network
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701007865678/60bd5d81-3c77-4091-88e0-42d470ba8e52.png
tags: docker, devops, docker-images, docker-network, 90daysofdevops, two-tier-application

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701007615085/1b8194cb-9dc6-4726-b4f4-62c33bd61fc3.png align="center")

[Siddhartha Gaurav](https://in.linkedin.com/in/siddhartha-gaurav-20354878?trk=profile-badge)

### What is a two-tier application??

A two-tier application refers to a software architecture where the application is divided into two main parts or tiers: the client (or front-end) tier and the server (or back-end) tier. In the case described, the two tiers are represented by:

1. **Application Tier (Front-end):** This is where the Python Flask app resides. It's the user interface or the part of the application that users interact with. The Flask app handles user requests, processes them, and communicates with the database tier to store or retrieve data.
    
2. **Database Tier (Back-end):** This is the MySQL database container. It's responsible for storing and managing the data used by the Flask app. The Flask app communicates with the database tier to store information (write) or retrieve information (read).
    

The two-tier architecture is a common setup, especially in simpler applications. It separates the user interface and application logic from the data storage and retrieval, making it easier to manage and scale each tier independently.

### Deploying a two-tier application

We're setting up a Python Flask app that talks to a MySQL database. There are two parts to this setup: one container for the app and another for the database. These two containers are isolated to each other.

Here's the key thing: the database container needs to be up and running before we start the Flask app container. Why? Because the app needs the database to save and fetch data. Without the database container, the app won't be able to handle data properly.

So, let's begin and deploy our first two-tier application -

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
    
    ```dockerfile
    
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
    
4. Build an image using the Dockerfile you created. Open a terminal or command prompt in the project directory and run the following command:
    
    ```bash
    
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker build -t flask-app:latest .
    DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                Install the buildx component to build images with BuildKit:
                https://docs.docker.com/go/buildx/
    
    Sending build context to Docker daemon  134.1kB
    Step 1/8 : FROM python:3.9-slim
    3.9-slim: Pulling from library/python
    1f7ce2fa46ab: Pull complete 
    1fb7efcf9eab: Pull complete 
    07483b63eff3: Pull complete 
    3d52c1551390: Pull complete 
    732bf57bb511: Pull complete 
    Digest: sha256:c54eecbf55a7527c913aef9a90e310c03bb78bea2204be57f39b28e43ab733ab
    Status: Downloaded newer image for python:3.9-slim
     ---> db813260829a
    Step 2/8 : WORKDIR /app
     ---> Running in aad8f571011c
    Removing intermediate container aad8f571011c
     ---> 004aeb1be581
    Step 3/8 : RUN apt-get update     && apt-get upgrade -y     && apt-get install -y gcc default-libmysqlclient-dev pkg-config     && rm -rf /var/lib/apt/lists/*
     ---> Running in 19cd424d566c
    Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
    Get:2 http://deb.debian.org/debian bookworm-updates InRelease [52.1 kB]
    Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
    Get:4 http://deb.debian.org/debian bookworm/main amd64 Packages [8780 kB]
    Get:5 http://deb.debian.org/debian bookworm-updates/main amd64 Packages [6668 B]
    Get:6 http://deb.debian.org/debian-security bookworm-security/main amd64 Packages [103 kB]
    Fetched 9141 kB in 2s (6087 kB/s)
    Reading package lists...
    Reading package lists...
    Building dependency tree...
    Reading state information...
    Calculating upgrade...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
    Reading package lists...
    Building dependency tree...
    Reading state information...
    Processing triggers for libc-bin (2.36-9+deb12u3) ...
    Removing intermediate container 19cd424d566c
     ---> 171b973ab65c
    Step 4/8 : COPY requirements.txt .
     ---> 84d75450e9a2
    Step 5/8 : RUN pip install mysqlclient
     ---> Running in 0f52e13662f0
    Collecting mysqlclient
      Downloading mysqlclient-2.2.0.tar.gz (89 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 89.5/89.5 kB 3.5 MB/s eta 0:00:00
      Installing build dependencies: started
      Installing build dependencies: finished with status 'done'
      Getting requirements to build wheel: started
      Getting requirements to build wheel: finished with status 'done'
      Installing backend dependencies: started
      Installing backend dependencies: finished with status 'done'
      Preparing metadata (pyproject.toml): started
      Preparing metadata (pyproject.toml): finished with status 'done'
    Building wheels for collected packages: mysqlclient
      Building wheel for mysqlclient (pyproject.toml): started
      Building wheel for mysqlclient (pyproject.toml): finished with status 'done'
      Created wheel for mysqlclient: filename=mysqlclient-2.2.0-cp39-cp39-linux_x86_64.whl size=131073 sha256=4603020e214b72043e048140ea6a519661acf238b93da73f646a902a9c66010c
      Stored in directory: /root/.cache/pip/wheels/aa/58/d4/2e7a1d266508fd74887c2f74ec1ae819509bae6711480d8666
    Successfully built mysqlclient
    Installing collected packages: mysqlclient
    Successfully installed mysqlclient-2.2.0
    WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
    
    [notice] A new release of pip is available: 23.0.1 -> 23.3.1
    [notice] To update, run: pip install --upgrade pip
    Removing intermediate container 0f52e13662f0
     ---> 56ed5de0d590
    Step 6/8 : RUN pip install --no-cache-dir -r requirements.txt
     ---> Running in fd8712125839
    Collecting Flask==2.0.1
      Downloading Flask-2.0.1-py3-none-any.whl (94 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 94.8/94.8 kB 4.7 MB/s eta 0:00:00
    Collecting Flask-MySQLdb==0.2.0
      Downloading Flask-MySQLdb-0.2.0.tar.gz (2.1 kB)
      Preparing metadata (setup.py): started
      Preparing metadata (setup.py): finished with status 'done'
    Collecting requests==2.26.0
      Downloading requests-2.26.0-py2.py3-none-any.whl (62 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 62.3/62.3 kB 118.9 MB/s eta 0:00:00
    Collecting Werkzeug==2.2.2
      Downloading Werkzeug-2.2.2-py3-none-any.whl (232 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 232.7/232.7 kB 70.2 MB/s eta 0:00:00
    Collecting Jinja2>=3.0
      Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.1/133.1 kB 136.6 MB/s eta 0:00:00
    Collecting itsdangerous>=2.0
      Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
    Collecting click>=7.1.2
      Downloading click-8.1.7-py3-none-any.whl (97 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 129.6 MB/s eta 0:00:00
    Requirement already satisfied: mysqlclient in /usr/local/lib/python3.9/site-packages (from Flask-MySQLdb==0.2.0->-r requirements.txt (line 2)) (2.2.0)
    Collecting charset-normalizer~=2.0.0
      Downloading charset_normalizer-2.0.12-py3-none-any.whl (39 kB)
    Collecting idna<4,>=2.5
      Downloading idna-3.4-py3-none-any.whl (61 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.5/61.5 kB 94.6 MB/s eta 0:00:00
    Collecting urllib3<1.27,>=1.21.1
      Downloading urllib3-1.26.18-py2.py3-none-any.whl (143 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 143.8/143.8 kB 128.7 MB/s eta 0:00:00
    Collecting certifi>=2017.4.17
      Downloading certifi-2023.11.17-py3-none-any.whl (162 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 162.5/162.5 kB 134.3 MB/s eta 0:00:00
    Collecting MarkupSafe>=2.1.1
      Downloading MarkupSafe-2.1.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
    Building wheels for collected packages: Flask-MySQLdb
      Building wheel for Flask-MySQLdb (setup.py): started
      Building wheel for Flask-MySQLdb (setup.py): finished with status 'done'
      Created wheel for Flask-MySQLdb: filename=Flask_MySQLdb-0.2.0-py3-none-any.whl size=2663 sha256=bda01f80005136964046f68d44587597a2f48c5a217545c69a34b3e8380f3fb3
      Stored in directory: /tmp/pip-ephem-wheel-cache-0r3alu5a/wheels/41/ab/e5/ac1bfe8e719b0c95880c23643ce001363e8240f615f260755e
    Successfully built Flask-MySQLdb
    Installing collected packages: urllib3, MarkupSafe, itsdangerous, idna, click, charset-normalizer, certifi, Werkzeug, requests, Jinja2, Flask, Flask-MySQLdb
    Successfully installed Flask-2.0.1 Flask-MySQLdb-0.2.0 Jinja2-3.1.2 MarkupSafe-2.1.3 Werkzeug-2.2.2 certifi-2023.11.17 charset-normalizer-2.0.12 click-8.1.7 idna-3.4 itsdangerous-2.1.2 requests-2.26.0 urllib3-1.26.18
    WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
    
    [notice] A new release of pip is available: 23.0.1 -> 23.3.1
    [notice] To update, run: pip install --upgrade pip
    Removing intermediate container fd8712125839
     ---> 01f67a9ffdd2
    Step 7/8 : COPY . .
     ---> 400d333b673c
    Step 8/8 : CMD ["python", "app.py"]
     ---> Running in a61072e978ec
    Removing intermediate container a61072e978ec
     ---> 028f8e85bb31
    Successfully built 028f8e85bb31
    Successfully tagged flask-app:latest
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$
    ```
    
5. To list the Docker images you've built, run the following command in your terminal or command prompt:
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker images
    REPOSITORY   TAG        IMAGE ID       CREATED         SIZE
    flask-app    latest     028f8e85bb31   2 minutes ago   391MB
    python       3.9-slim   db813260829a   5 weeks ago     126MB
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$
    ```
    
6. To deploy the MySQL container, use the following command in your terminal or command prompt. Make sure to replace the placeholder values with your specific configuration. These environment variables are crucial for setting up the MySQL container:
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=test@123 -e MYSQL_DATABASE-testdb -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin mysql:latest
    Unable to find image 'mysql:latest' locally
    latest: Pulling from library/mysql
    8e0176adc18c: Pull complete 
    2d2c52718f65: Pull complete 
    d88d03ce139b: Pull complete 
    4a7d7f11aa1e: Pull complete 
    ce5949193e4c: Pull complete 
    f7f024dfb329: Pull complete 
    5fc3c840facc: Pull complete 
    509068e49488: Pull complete 
    cbc847bab598: Pull complete 
    942bef62a146: Pull complete 
    Digest: sha256:1773f3c7aa9522f0014d0ad2bbdaf597ea3b1643c64c8ccc2123c64afd8b82b1
    Status: Downloaded newer image for mysql:latest
    46cff555a02d27924c253d9d3d9760c5f6da93e59ec1e9c2396f0176d7e326a4
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker ps
    CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
    46cff555a02d   mysql:latest   "docker-entrypoint.s…"   24 seconds ago   Up 23 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   awesome_shaw
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$
    ```
    
    In the above command, we are running the MySQL container in detached mode (`-d`), allowing it to run in the background. We have mapped the container's port 3306 to the host's port 3306 (`-p 3306:3306`), enabling communication between the MySQL container and the host system.
    
    Crucially, we've provided essential environment variables:
    
    * `MYSQL_ROOT_PASSWORD`: This sets the root password for the MySQL server.
        
    * `MYSQL_DATABASE`: This specifies the name of the initial database to be created when the container starts.
        
    * `MYSQL_USER`: This defines a new MySQL user with specific privileges.
        
    * `MYSQL_PASSWORD`: This sets the password for the MySQL user created above.
        
    
    These environment variables are instrumental in configuring the MySQL container according to your specific requirements.
    
    Once the command is executed successfully, the container is up and running, ready to handle database operations.
    
7. Now that our MySQL container is up and running, let's deploy our Flask application container in a similar manner. We'll use the 'flask-app' image that we created earlier from the Dockerfile.
    
    Execute the following command in your terminal or command prompt:
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker run -d -p 5000:5000 -e MYSQL_HOST=mysql -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DB=testdb --name=flask-app flask-a:latest
    acd1affda524f8c7ecbbf518df8c5dd9da99fdd0a1e5db4ccde41d69168bb588
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ 
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$ docker ps
    CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
    acd1affda524   flask-app:latest   "python app.py"          6 seconds ago    Up 3 seconds    0.0.0.0:5000->5000/tcp, :::5000->5000/tcp              flask-app
    46cff555a02d   mysql:latest       "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   awesome_shaw
    ubuntu@ip-172-31-47-90:~/projects/two-tier-flask-app$
    ```
    
    In the above command, we are deploying a container for the 'flask-app' using the image created earlier. The application within the container will be accessible on port 5000, and we've mapped that to the host system using the flag (-p 5000:5000).
    
    To ensure the Flask app communicates effectively with the MySQL database, we've provided the necessary environment variables:
    
    * `-e DB_HOST=mysql`: Specifies the hostname of the MySQL database container.
        
    * `-e DB_PORT=3306`: Sets the port on which the MySQL database is running.
        
    * `-e DB_USER=admin`: Defines the MySQL user that the Flask app will use.
        
    * `-e DB_PASSWORD=admin`: Sets the password for the MySQL user.
        
    * `-e DB_NAME=testdb`: Specifies the name of the MySQL database.
        
    
    These environment variables are essential for establishing a seamless connection between the Flask application and the MySQL database.
    
    After running this command, the Flask application container will be up and running, ready to serve requests on port 5000, interacting with the MySQL container using the provided configuration.
    
8. Now, let's access the application from your host by opening a web browser and navigating to the IP address of your EC2 instance followed by port 5000 (e.g., [`http://your_ec2_ip:5000`](http://your_ec2_ip:5000)).
    
    In the case of an EC2 instance, ensure that you have allowed traffic on port 5000 in your Security Group settings. This step is crucial for external access to the Flask application. If you haven't done so already, add an inbound rule to allow traffic on port 5000.
    
    Once the Security Group is configured, you should be able to see and interact with your Flask application through the specified IP address and port.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700999902967/42614dc2-152d-4486-b058-4b17dd46ff92.png align="center")
    
    After allowing the necessary ports in your Security Group, open your web browser and enter the following address: http://your-ip:5000
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701003629627/1c28d1a5-d0aa-48db-988c-bb7236bf4524.png align="center")
    
    The error you're encountering indicates a communication issue between the Flask application and the MySQL database. This is likely due to both containers running on separate networks.
    
9. To resolve this, consider creating a Docker network and connecting both containers to it. This enables them to communicate seamlessly. Here are the steps:
    
    ```bash
    ubuntu@ip-172-31-47-90:~$ docker network ls
    NETWORK ID     NAME      DRIVER    SCOPE
    9fdeca56567d   bridge    bridge    local
    9cab723192ad   host      host      local
    87f298874199   none      null      local
    ubuntu@ip-172-31-47-90:~$ 
    ubuntu@ip-172-31-47-90:~$ docker network create -d bridge two-tier-app-nw
    0199a9102bea5130ce4f3aa70738014f45d1005951429ace7609da6dfd4b60d2
    ubuntu@ip-172-31-47-90:~$ 
    ubuntu@ip-172-31-47-90:~$ docker network ls
    NETWORK ID     NAME              DRIVER    SCOPE
    9fdeca56567d   bridge            bridge    local
    9cab723192ad   host              host      local
    87f298874199   none              null      local
    0199a9102bea   two-tier-app-nw   bridge    local
    ubuntu@ip-172-31-47-90:~$
    ```
    
    In the previous step, we established a bridge network named 'two-tier-app-nw' using the following Docker command: `docker network create -d bridge two-tier-app-nw`
    
    Here, the `-d` flag allows us to specify the network driver, and 'bridge' is chosen as the driver type. Bridge networks are often preferred for enabling communication between containers on the same host.
    
    This network will serve as the shared space for our MySQL and Flask application containers, facilitating seamless communication between them
    
10. Let's inspect the 'two-tier-app-nw' network to gather information about its configuration, including the connected containers. Execute the following command in your terminal or command prompt:
    
    ```bash
    ubuntu@ip-172-31-47-90:~$ docker network inspect two-tier-app-nw 
    [
        {
            "Name": "two-tier-app-nw",
            "Id": "0199a9102bea5130ce4f3aa70738014f45d1005951429ace7609da6dfd4b60d2",
            "Created": "2023-11-26T13:11:29.076831355Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {},
            "Options": {},
            "Labels": {}
        }
    ]
    ubuntu@ip-172-31-47-90:~$
    ```
    
    The inspection result indicates that currently, no containers are bound to the 'two-tier-app-nw' network. To ensure proper communication between our MySQL and Flask application containers, both containers need to be connected to this network.
    
11. Let's create the necessary containers within our newly created bridge network named 'two-tier-app-nw.' Execute the following commands to launch the containers in this specific network:
    
    ```bash
    ubuntu@ip-172-31-47-90:~$ docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=test@123 -e MYSQL_DATABASE=testdb -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin --network two-tier-app-nw --name mysql mysql:latest
    8a6c4b1e8b09312c87e05320c6ab6c57dc7e6d395a3f2c8dff31f1971d3a348d
    ubuntu@ip-172-31-47-90:~$ 
    ubuntu@ip-172-31-47-90:~$ docker run -d -p 5000:5000 -e MYSQL_HOST=mysql -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DB=testdb --network two-tier-app-nw --name=flask-app flask-app:latest
    d61530c6fde9459338f87d1ac2880aa824ee113248c95396f2c6aef13ac4a0a3
    ubuntu@ip-172-31-47-90:~$ 
    ubuntu@ip-172-31-47-90:~$ docker ps
    CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
    8a6c4b1e8b09   mysql:latest       "docker-entrypoint.s…"   40 seconds ago   Up 39 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   mysql
    d61530c6fde9   flask-app:latest   "python app.py"          9 minutes ago    Up 9 minutes    0.0.0.0:5000->5000/tcp, :::5000->5000/tcp              flask-app
    ubuntu@ip-172-31-47-90:~$ 
    ubuntu@ip-172-31-47-90:~$
    ```
    
    \-In the preceding command, we utilized the `--network` option to explicitly designate the network to which the containers should be connected and operate within. This ensures that both the MySQL and Flask application containers are part of the same network, enabling seamless communication between them.
    
12. Let's inspect the 'two-tier-app-nw' bridge network again to ensure that the containers are now connected to it. Execute the following command in your terminal or command prompt:
    
    ```bash
    ubuntu@ip-172-31-47-90:~$ docker network inspect two-tier-app-nw 
    [
        {
            "Name": "two-tier-app-nw",
            "Id": "0199a9102bea5130ce4f3aa70738014f45d1005951429ace7609da6dfd4b60d2",
            "Created": "2023-11-26T13:11:29.076831355Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {
                "8a6c4b1e8b09312c87e05320c6ab6c57dc7e6d395a3f2c8dff31f1971d3a348d": {
                    "Name": "mysql",
                    "EndpointID": "78a6eee2fcdeeab17373618a69dcb5d4bcc53345dd7db8b023b0039072855d83",
                    "MacAddress": "02:42:ac:12:00:02",
                    "IPv4Address": "172.18.0.2/16",
                    "IPv6Address": ""
                },
                "d61530c6fde9459338f87d1ac2880aa824ee113248c95396f2c6aef13ac4a0a3": {
                    "Name": "flask-app",
                    "EndpointID": "aa5378f67d4ea36541ff01005bac56971f19d1f54ec99972b77252c444c6a7e9",
                    "MacAddress": "02:42:ac:12:00:03",
                    "IPv4Address": "172.18.0.3/16",
                    "IPv6Address": ""
                }
            },
            "Options": {},
            "Labels": {}
        }
    ]
    ubuntu@ip-172-31-47-90:~$
    ```
    
    After inspecting the 'two-tier-app-nw' bridge network, we observe that both containers' IP configuration details are listed. This signifies that the MySQL and Flask application containers are successfully connected within the same network, establishing the necessary communication link between them.
    
13. Now that both containers are successfully connected within the same network, open your web browser and enter the given address: http://your-ip:5000
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701008003957/819e9554-b9d7-4ef9-8905-848ef5916fea.png align="center")
    
    Seeing an error indicating that the required table doesn't exist? No worries. Let's log into the MySQL container and create the table. Execute the following command in your terminal or command prompt:
    
    ```bash
    ubuntu@ip-172-31-47-90:~$ docker exec -it mysql bash
    bash-4.4# mysql -u root -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 13
    Server version: 8.2.0 MySQL Community Server - GPL
    
    Copyright (c) 2000, 2023, Oracle and/or its affiliates.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> use testdb;
    Database changed
    mysql> CREATE TABLE messages (
        ->     id INT AUTO_INCREMENT PRIMARY KEY,
        ->     message TEXT
        -> );
    Query OK, 0 rows affected (0.06 sec)
    
    mysql> exit
    Bye
    bash-4.4# exit
    ubuntu@ip-172-31-47-90:~$
    ```
    
14. Now, let's again open the app over the web page -
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701008358802/4c7c9981-1a85-47ec-9fcf-c6c3e9de328e.png align="center")
    

Hurray! Our two-tier application is now up and running on Docker. We've successfully deployed the frontend using Flask and the backend using MySQL as Docker containers. The seamless integration of these components demonstrates the power and flexibility of containerized applications. Congratulations on the successful setup!

### Conclusion:

In conclusion, this blog has guided you through the process of setting up a robust two-tier application using Docker. From cloning the repository and creating Dockerfiles to deploying Flask and MySQL containers, we've covered each step to ensure a seamless integration of the front-end and back-end components.

By leveraging Docker's containerization, you've not only simplified the deployment process but also ensured a consistent environment for your application. The bridge network creation and container connections have facilitated efficient communication between the Flask app and MySQL database.

As you explore the possibilities of containerized applications, remember the scalability and portability benefits that Docker brings to the table. This hands-on experience lays a foundation for further customization and expansion of your projects.

Feel free to share your thoughts and experiences with containerized applications. Happy coding, and may your Dockerized applications thrive!