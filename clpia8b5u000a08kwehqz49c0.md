---
title: "Optimizing Docker Images: A Deep Dive into Multi-Stage Builds"
datePublished: Tue Nov 28 2023 11:56:19 GMT+0000 (Coordinated Universal Time)
cuid: clpia8b5u000a08kwehqz49c0
slug: optimizing-docker-images-a-deep-dive-into-multi-stage-builds
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701172513831/de7ed1cd-1ff6-4686-82bd-6846876ce14e.webp
tags: docker, docker-images, 90daysofdevops, docker-build, multistage-docker-build

---

Multi-stage Docker builds are a technique used to optimize the size of Docker images by dividing the build process into multiple stages. This is particularly useful for minimizing the final image size while still maintaining a clear and readable Dockerfile.

Let's break down the concept for beginners:

### **Basic Dockerfile Structure:**

A Dockerfile is a script that contains instructions for building a Docker image. Here's a basic structure:

```bash
dockerfileCopy code# Stage 1: Build stage
FROM base_image AS build_stage
WORKDIR /app
COPY . .
RUN some_build_command

# Stage 2: Final stage
FROM base_image
WORKDIR /app
COPY --from=build_stage /app/output /app
CMD ["some_command"]
```

### **Explanation:**

1. `FROM` **Statements:**
    
    * The `FROM` keyword specifies the base image for each stage. The first stage is often used for building and compiling code, while the second stage is for the final runtime environment.
        
2. **Stages Naming:**
    
    * Each stage has a name (e.g., `AS build_stage`). This allows you to reference the output of one stage in another stage using the `--from=build_stage` syntax.
        
3. `WORKDIR`**:**
    
    * Sets the working directory inside the container for subsequent commands.
        
4. `COPY`**:**
    
    * Copies files or directories from the host machine into the container. In the example, the entire context is copied in the first stage, but only the necessary output is copied from the build stage to the final stage.
        
5. `RUN`**:**
    
    * Executes a command during the build process. In the build stage, this might include commands to compile code or perform other build tasks.
        
6. `CMD`**:**
    
    * Specifies the default command to run when the container starts. It is often used in the final stage to define the application or service to run.
        

### **Benefits:**

* **Smaller Image Size:**
    
    * The final image only contains the necessary artifacts and binaries from the build stage, resulting in a smaller image size.
        
* **Security:**
    
    * Build dependencies and tools are only present in the build stage, not in the final image, reducing potential security risks.
        
* **Readability:**
    
    * The Dockerfile is more readable and maintains a clear separation between build and runtime concerns.
        

### **Example Use Case:**

Let's take an example of a Python multistage application. We will build the image in both the standard and multistage ways.

Let's first build it in the standard way:

1. Clone the python-multistage application repository.
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects$ git clone https://github.com/sgaurav7/python-multistage-docker.git
    Cloning into 'python-multistage-docker'...
    remote: Enumerating objects: 9, done.
    remote: Counting objects: 100% (9/9), done.
    remote: Compressing objects: 100% (7/7), done.
    remote: Total 9 (delta 0), reused 9 (delta 0), pack-reused 0
    Receiving objects: 100% (9/9), done.
    ubuntu@ip-172-31-47-90:~/projects$ ls
    python-multistage-docker  two-tier-flask-app
    ubuntu@ip-172-31-47-90:~/projects$ cd python-multistage-docker/
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ ls
    README.md  backend
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ```
    
2. Let's write a standard Dockerfile for our Python application.
    
    ```dockerfile
    FROM python:3.9
    
    WORKDIR /app
    
    COPY ./backend /app
    
    RUN pip install -r requirements.txt
    
    EXPOSE 5000
    
    CMD ["python", "app.py"]
    ```
    
3. Let's build an image named `python-app` using the above Dockerfile.
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ docker build -t python-app .
    DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                Install the buildx component to build images with BuildKit:
                https://docs.docker.com/go/buildx/
    
    Sending build context to Docker daemon  67.58kB
    Step 1/6 : FROM python:3.9
    3.9: Pulling from library/python
    90e5e7d8b87a: Already exists 
    27e1a8ca91d3: Already exists 
    d3a767d1d12e: Already exists 
    711be5dc5044: Already exists 
    7ad48fee4003: Already exists 
    00ba6b570755: Already exists 
    b7551a8d91b5: Already exists 
    f36f46aa93f3: Already exists 
    Digest: sha256:f2f14d6dabcbc113512050906474d44cdb2a1139fa26d9b05bef0eedb36ac94a
    Status: Downloaded newer image for python:3.9
     ---> e9047a20be92
    Step 2/6 : WORKDIR /app
     ---> Running in 4781caf6159d
    Removing intermediate container 4781caf6159d
     ---> 4bfd29028354
    Step 3/6 : COPY ./backend /app
     ---> aa64fd82f3e9
    Step 4/6 : RUN pip install -r requirements.txt
     ---> Running in ceb6f0f478c2
    Collecting Flask==2.0.1
      Downloading Flask-2.0.1-py3-none-any.whl (94 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 94.8/94.8 kB 3.5 MB/s eta 0:00:00
    Collecting Jinja2>=3.0
      Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.1/133.1 kB 10.4 MB/s eta 0:00:00
    Collecting click>=7.1.2
      Downloading click-8.1.7-py3-none-any.whl (97 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 12.8 MB/s eta 0:00:00
    Collecting itsdangerous>=2.0
      Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
    Collecting Werkzeug>=2.0
      Downloading werkzeug-3.0.1-py3-none-any.whl (226 kB)
         ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 226.7/226.7 kB 23.2 MB/s eta 0:00:00
    Collecting MarkupSafe>=2.0
      Downloading MarkupSafe-2.1.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
    Installing collected packages: MarkupSafe, itsdangerous, click, Werkzeug, Jinja2, Flask
    Successfully installed Flask-2.0.1 Jinja2-3.1.2 MarkupSafe-2.1.3 Werkzeug-3.0.1 click-8.1.7 itsdangerous-2.1.2
    WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
    
    [notice] A new release of pip is available: 23.0.1 -> 23.3.1
    [notice] To update, run: pip install --upgrade pip
    Removing intermediate container ceb6f0f478c2
     ---> 6126decda17c
    Step 5/6 : EXPOSE 5000
     ---> Running in bc86b44904ae
    Removing intermediate container bc86b44904ae
     ---> 7b3f53ff51c0
    Step 6/6 : CMD ["python", "app.py"]
     ---> Running in ca001f4a445b
    Removing intermediate container ca001f4a445b
     ---> 2253e3baf996
    Successfully built 2253e3baf996
    Successfully tagged python-app:latest
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ```
    
4. Check the size of the newly created `python-app` image.
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ docker images
    REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
    python-app   latest    2253e3baf996   4 seconds ago   1.01GB
    python       3.9       e9047a20be92   6 weeks ago     997MB
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ```
    
5. Now, we will create a multi-stage Dockerfile to reduce the size of the previous image.
    
    ```dockerfile
    
    # stage1
    
    FROM python:3.9 AS backend-builder #given name backend-builder to this image
    
    WORKDIR /app
    
    COPY ./backend /app
    
    RUN pip install -r requirements.txt
    
    #stage1 ends- we create a umage above with our requirements
    
    #stage2
    
    FROM python:3.9-slim-buster  #picking smaller image of python from docker hub
    
    WORKDIR /app
    
    COPY --from=backend-builder /app /app #copy the stage1 content to stage2 image
    
    EXPOSE 5000
    
    CMD ["python", "app.py"]
    ```
    
6. Let's build the image from the above Dockerfile with the name `python-compressed-app`.
    
    ```bash
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ docker build -t python-compressed-app .
    DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                Install the buildx component to build images with BuildKit:
                https://docs.docker.com/go/buildx/
    
    Sending build context to Docker daemon  67.58kB
    Step 1/9 : FROM python:3.9 AS backend-builder
     ---> e9047a20be92
    Step 2/9 : WORKDIR /app
     ---> Using cache
     ---> 4bfd29028354
    Step 3/9 : COPY ./backend /app
     ---> Using cache
     ---> aa64fd82f3e9
    Step 4/9 : RUN pip install -r requirements.txt
     ---> Using cache
     ---> 6126decda17c
    Step 5/9 : FROM python:3.9-slim-buster
     ---> c84dbfe3b8de
    Step 6/9 : WORKDIR /app
     ---> Running in 124c142360cb
    Removing intermediate container 124c142360cb
     ---> aec6f0dec5ef
    Step 7/9 : COPY --from=backend-builder /app /app
     ---> 1c0c963a5a09
    Step 8/9 : EXPOSE 5000
     ---> Running in b25e53633a49
    Removing intermediate container b25e53633a49
     ---> 44b5c17e8910
    Step 9/9 : CMD ["python", "app.py"]
     ---> Running in 533a179503b7
    Removing intermediate container 533a179503b7
     ---> c0dbca269041
    Successfully built c0dbca269041
    Successfully tagged python-compressed-app:latest
    ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
    ```
    

Now, let's check the size of the newly created Docker image.

```bash
ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ docker images
REPOSITORY              TAG               IMAGE ID       CREATED          SIZE
python-compressed-app   latest            c0dbca269041   11 seconds ago   116MB
python-app              latest            2253e3baf996   13 minutes ago   1.01GB
python                  3.9               e9047a20be92   6 weeks ago      997MB
python                  3.9-slim-buster   c84dbfe3b8de   5 months ago     116MB
ubuntu@ip-172-31-47-90:~/projects/python-multistage-docker$ 
```

Hurray! We can see that we have a significantly smaller image for our application. The image created using the standard method is 1.01 GB, whereas the new multi-stage Dockerfile image is now only 116MB, which is significantly smaller.

Now, you can proceed with any additional steps or information you'd like to provide after comparing the image sizes.

### Conclusion

In conclusion, adopting a multi-stage Dockerfile approach has proven to be a powerful strategy for optimizing the size of our application images. By carefully crafting a Dockerfile that utilizes multiple build stages, we achieved a substantial reduction in image size compared to the traditional single-stage build. The standard Dockerfile resulted in an image size of 1.01 GB, while the new multi-stage Dockerfile produced a much leaner 116MB image.

This reduction in size not only contributes to more efficient image storage but also facilitates faster deployment and scaling of our application. The multi-stage approach allows us to include only the necessary dependencies in the final image, resulting in a more streamlined and production-ready container.

Incorporating best practices like multi-stage builds into our Dockerfile not only optimizes resource utilization but also enhances the overall performance and maintainability of our containerized applications. As we continue to evolve our containerization strategies, adopting efficient techniques becomes imperative for building scalable, lightweight, and high-performance Docker images.