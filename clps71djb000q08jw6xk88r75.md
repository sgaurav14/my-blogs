---
title: "Code, Build, Deploy: Transforming Your Workflow with Jenkins Pipeline Mastery"
datePublished: Tue Dec 05 2023 10:24:38 GMT+0000 (Coordinated Universal Time)
cuid: clps71djb000q08jw6xk88r75
slug: code-build-deploy-transforming-your-workflow-with-jenkins-pipeline-mastery
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701771761582/31786f85-030e-4d3f-992a-669584bf2367.jpeg
tags: jenkins, jenkins-devops, 90daysofdevops, jenkins-pipeline

---

**What is a Pipeline in Jenkins?**

A Jenkins Pipeline is a set of instructions given in the form of code that defines a series of automated steps for building, testing, and deploying your software. It allows you to script your entire build process, making it more flexible and easier to manage.

Think of a pipeline as a workflow or a set of instructions that Jenkins follows to do things like:

1. **Fetch Code:** Get the latest version of your code from a version control system (like Git).
    
2. **Build:** Compile your code into an executable or deployable form.
    
3. **Test:** Run automated tests to ensure that your code works as expected.
    
4. **Deploy:** If all tests pass, deploy your code to a server or platform where it can be used.
    

**Why use Jenkins Pipeline?**

Using a pipeline is like having a recipe for cooking. Instead of manually going through each step of preparing a meal, you have a set of instructions that you can follow. Similarly, Jenkins Pipeline allows you to automate and script the steps involved in the software development process, making it more efficient and less error-prone.

**How does a Jenkins Pipeline look like?**

A Jenkins Pipeline is often written in a language called Groovy. It looks something like this:

```bash
codepipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Code to build the project
            }
        }
        stage('Test') {
            steps {
                // Code to run automated tests
            }
        }
        stage('Deploy') {
            steps {
                // Code to deploy the project
            }
        }
    }
}
```

Here, you have defined three stages (Build, Test, Deploy), and each stage has a set of steps. Jenkins will execute these steps in order, and if any step fails, the pipeline can be configured to stop, preventing the deployment of potentially faulty code.

In essence, Jenkins Pipeline is a way to automate and visualize the entire software delivery process, from code commit to deployment, in a structured and repeatable way. It helps in achieving Continuous Integration and Continuous Deployment (CI/CD) practices, ensuring that your software is always in a deployable state.

Let's perform a real-time practical on this.

### Creating a pipeline for to-do app:  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701771835505/f9541cc2-4ecc-4ded-aa8c-1cc2a7ff05ba.png align="center")

1. Log in to the Jenkins dashboard by opening your jenkins URL: `http://jenkin-server-ip:8080`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701767059530/e10e1d88-bf56-4364-9992-37c2f86168db.png align="center")
    
2. Click on "New Item" -&gt; Enter the project name you desire -&gt; Choose the "Pipeline" option.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701767156616/3d4f3edf-b587-4524-bce7-82139a5c51c8.png align="center")
    
    Click "OK".
    
3. Now, you will encounter the following form or template, where you need to define all the parameters:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701767291677/2d109c5c-fdfe-450a-bbe4-ec1411a17bf3.png align="center")
    
    Provide the description.
    
4. Select the GitHub option if your code is hosted there.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701767429368/4814c63d-fbfe-49f8-8c42-dfee1ef4eb18.png align="center")
    
5. In the "Build Triggers" option, choose any option as per your project requirements. In our example, we are not selecting anything.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701767525351/773f83a1-0a57-4652-96d5-b53a6faa8e44.png align="center")
    
6. Now, you will encounter the form to fill for "Advanced Project Options" and "Pipeline." Leave the Advanced Project Options blank and write your pipeline script in the Pipeline template box.
    
    ```bash
    pipeline {
        agent any
         stages {
            stage("code"){
                steps {
                    git url: "https://github.com/sgaurav7/node-todo-cicd", branch: "master"
                    echo "code cloned"
                }
            }
            stage("build"){
                steps {
                    sh "docker build -t todo-app ."
                    echo "code build"
                }
            }
            stage("test"){
                steps {
                    echo "code tested"
                }
            }		
            stage("push"){
                steps {
                   withCredentials([usernamePassword(credentialsId:"dockerhub-id",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker tag todo-app ${env.dockerhubuser}/todo-app:latest"
                    sh "docker push ${dockerhubuser}/todo-app:latest"
                    echo "code pushed"
                }
                }
            
            }
            stage("deploy"){
                steps {
                    sh "docker compose down && docker compose up -d"
                    echo "code deployed on the server"
                }
            }
        }
    }
    ```
    
    In the "Definition" option, you will find two choices:
    
    a. **Pipeline script:** In this option, you write the Jenkins Pipeline script directly in this section.
    
    b. **Pipeline script from SCM:** In this option, the pipeline script is retrieved from your source code management tool, and it expects the file for the pipeline to follow the naming convention "Jenkinsfile."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701768004180/d5741a77-4c76-44fd-bc96-2200bc2cdb25.png align="center")
    
7. Select the "Pipeline script" option and input the pipeline script mentioned above.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701768143318/8efea14c-750e-41e3-a272-b11a41152ba4.png align="center")
    
    Click on "Save" option.
    
8. After saving all our configurations, we will see the following dashboard.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701768208997/adc96167-bf07-4417-8269-5351625e299d.png align="center")
    
9. Click on the "Build Now" option.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701768289275/6f6a89ea-8916-40bc-8fb7-c9e8fb804d61.png align="center")
    
    It will display the pipeline, progressing through each stage sequentially.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701770377347/bcb5011d-b7d5-4063-b45f-dc975bd6bf4f.png align="center")
    
    And hurray! The green lights indicate that our pipeline has passed successfully.
    
10. If you wish, you can view the logs by selecting the build from the bottom-left option under "Build History."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701770499646/1f4ea8b6-7323-47a9-9fa2-6247384cd8a4.png align="center")
    
    Now, select "Console Output" to view the real-time logs.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701770573007/8a20f869-5c8f-4944-92f4-4affa41527a7.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701770595166/e1886411-add1-42ff-bac8-8ea50d56e206.png align="center")
    
    In the end, you should observe that it has finished successfully.
    
11. Since the above pipeline has passed successfully, let's open our to-do application by navigating to the URL `http://Machine-IP:8000/`.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701770716366/1c6d2822-e012-45c3-9343-ee045f451dc2.png align="center")
    
    And there you have it! Our application is live now. Without manually cloning the code, building the image, pushing the image to Docker Hub, and deploying it on the server, we automated all these tasks by creating the pipeline and instructing Jenkins to handle the job for us. Automation at its best!
    

### Conclusion

So, we've just wrapped up a cool adventure using Jenkins Pipeline to make our software work smarter, not harder. Imagine Jenkins as a super helpful assistant that takes care of all the boring, repetitive stuff in creating and delivering our software.

We started by telling Jenkins what our software needs to do - like grabbing the code, building it, and making sure it works. Then, we watched as Jenkins followed these instructions step by step, kind of like a chef following a recipe. The best part? Everything happened automatically!

The little green lights at the end of our Jenkins journey signaled success. Our to-do application is now up and running, and we didn't have to lift a finger once we set things in motion.

By automating these tasks, we not only saved time but also made sure everything is done right every time. No more worrying about mistakes! This blog guided you through the process, showcasing how Jenkins Pipeline can be your superhero in making software development easier and more reliable.

As you dive into this world of automation, play around with Jenkins, and discover how it can make your software journey a whole lot smoother. Happy automating!