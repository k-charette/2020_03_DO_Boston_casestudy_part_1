## General Overview of the Project ##
This is a project aimed to build a working CI/CD pipeline with the following tools: Git, Jenkins, Docker, Kubernetes, and Ansible. 

**Putting it all together let's understand the workflow!**

Here is a [link](https://docs.google.com/presentation/d/1Ds2vxI54l72EJfhTaR9f1ep7N6ep9Qd8PY06Zm3W2R0/edit?usp=sharing "ken google slides") to the presentation with more detailed information about the workflow and problems encountered. 

- Developers will commit code to the application's repository on Github.
- A webhook configured in Github and Jenkins will notify Jenkins to start building the application.
- In the Jenkins pipeline job will start executing the steps listed in the Jenkinsfile.
- The build step in the Jenkinsfile creates a Docker image(kcharette/ec2-pipeline) using the provided Dockerfile.
- The image is given a tag and pushed to Docker Hub.
- Ansible playbook starts to deploy the Docker image to a Kubernetes cluster.
- The Ansible playbook will read all the Kubernetes definitions for deployment/servive and create required components in the cluster.

## Installing and Configuring Jenkins on AWS  ##
**Step 1 : Set up Prerequisites (make sure to have ver JDK 1.8+)**

Follow the documentation below on how to setup an account and create an EC2 instance: <br>

1. Sign up for AWS and create and IAM user name and password <br>[https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html "Documentation") [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html "IAM User Steps")

**Step 2 : Launch an EC2 Instance**
  
1. Launch a EC2 Instance to host Jenkins.
2. Create a Security Group <br>[https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#creating-security-group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#creating-security-group "create security group")

**Step 3 : Install and Configure Jenkins**

1. Check your version of java or install with:<br> `sudo yum install java-1.8.0`
2. Ensure your packages are up to date <br> `sudo yum update -y`
3. Download and Install Jenkins following the documentation here: <br> [https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos](https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos "jenkins installation docs")

4. Start the Jenkins service and check the status <br> Run `sudo service jenkins start` and then `sudo service jenkins status` <br> If there is an output similar to the image below then Jenkins is running!
![](https://i.imgur.com/zJ678Pl.png)

5. Now finally using the IP address of the instance followed by `8080` You should see the **Getting Started** screen for Jenkins. 

**Note : Before running the Jenkins, make sure your 8080 port is available. Another option is running Jenkins on a different port. Inside the configuration file located /etc/sysconfig/jenkins just change JENKINS_PORT. (The location in Debian based linux is /var/default/jenkins)**

## Jenkins Integration with Github Webhooks ##

Following the documentation here: [https://plugins.jenkins.io/github/](https://plugins.jenkins.io/github/ "webhook") and [https://medium.com/faun/triggering-jenkins-build-on-push-using-github-webhooks-52d4361542d4](https://medium.com/faun/triggering-jenkins-build-on-push-using-github-webhooks-52d4361542d4 "github webhooks")

1. Ensure Git is installed on the EC2 instance by running `sudo yum install git -y`  
2. From the **Github** repository go to Settings -> Webhooks -> Add webhook
3. In the 'Payload URL' field, past the Jenkins environment URL. At the end of the URL add `/github-webhook/`. In the 'Content type' select `application/json` and leave the 'Secret' field empty. 
![](https://i.imgur.com/N9tokvb.png)
4. Under 'Which events would you like to trigger this webhook?' I chose 'Just the push event' for this example, however you can select as many events as you want. 
5. From **Jenkins** create a new project and click on the Source Code Management tab.
6. Click on Git and paste in your Github repository URL in 'Repository URL' field. 
7. Click on the Build Triggers tab, then on the 'Github hook trigger for GITScm polling' and Save!

## Installing and Configuring Docker ##

Following the documentation here: [https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html "aws docker")


## Building A Docker Image and Pushing to Dockerhub ##
Following the documentation here: [https://dzone.com/articles/building-docker-images-to-docker-hub-using-jenkins#](https://dzone.com/articles/building-docker-images-to-docker-hub-using-jenkins#)

## Using Ansible with Jenkins ##
The video here gives details on how to install Ansible with Jenkins: [https://www.youtube.com/watch?v=PRpEbFZi7nI&ab_channel=JavaHomeCloud](https://www.youtube.com/watch?v=PRpEbFZi7nI&ab_channel=JavaHomeCloud)

1. Install the Ansible plugin on Jenkins
2. Go to Global Tool Configuration and locate Ansible. 
3. Add the Name and Path to ansible executables directory

## Creating an Ansible Playbook  ##
Following this video:
[https://www.youtube.com/watch?v=NSk0NHkTjDs&t=690s&ab_channel=DeekshithSN](https://www.youtube.com/watch?v=NSk0NHkTjDs&t=690s&ab_channel=DeekshithSN)



## Setting up Environment for Kubernetes  ##
[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)



![](https://i.imgur.com/t5J3s02.png)