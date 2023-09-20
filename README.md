<h1>Microservices Architecture with AWS ECS</h1>


<h2>Description</h2>
In this lab I will show how to deploy a microservices architecture to AWS ECS. Create an ECS cluster with two instances which will contain the services deployed. Create a load balancer to have a single entry point and redirect all the request to all my existing services. Create a task definition for my micro service with the latest version from a Docker image. Deploy a microservice in the cluster specifying the rollout strategy. And last, created autoscaling rules for my cluster and my service.
<br />


<h2>AWS Applications Used</h2> 

- <b>EC2</b> 
- <b>ECS</b>

<h2>Environments Used </h2>

- <b>AWS</b> 

<h2>Lab walk-through:</h2> 

<p align="center">
Lab 1.1 Target Groups <br/> 

<p align="center">
Step 1: Sepcify group details. Choose "instances" for your target type.  <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/18f4363e-261a-4d8e-9284-c312e5dff19f" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Create a target group name, use HTTP protocol and port 8080, use default VPC you created, and chose HTTP1 for your protocol version.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/95cfd9af-c3ca-473d-91b6-d6715c7cbf82" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 2: For health check protocol use HTTP, Health check path name use "/health" then click next.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/01c751ae-0db1-4b63-92d9-52e7d17d42bb" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Lab 1.2 Load Balancer <br/>

<p align="center">
Step 1: Next, you will navigate to the "Load Balancer tab and click "create load balancer". Then you will choose the "Application Load Balancer" type.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/9012d64f-3464-4195-8d40-c6fd8177fa5c" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 2: Choose your load balancer name, "Internet-facing" for your scheme, and "IPv4" for your IP address type.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/f3aee1d9-1c12-4a0b-bbb3-98ac957069ae" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 3: Choose your VPC that you have already created. And for "mappings" use the default subnets you created for "us-east-1a" and "us-east-1b".      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/c51b149e-2c14-4ea4-a616-aec3eb8228d0" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 4: Choose your default security group that you have created. Next select HTTP protocol and port 8080. Then select your "target group" that was previously created.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/e62056a4-6f36-4773-9ddd-fce1d336e67d" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 5: Scroll down to the bottom and click "Create Load Balancer".      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/64af2e95-cf3a-43ec-aad3-8f3353afe2b4" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Lab 1.3 ECS Cluster <br/>

<p align="center">
Step 1: Navigate to the "ECS Clusters" page and click on "create cluster". Choose your cluster name "my-cluster".      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/73eca94d-d705-404e-ae7d-b5def55fae3b" alt="Disk Sanitization Steps"/>
<br />

<p align="center">
Step 2: Select AWS Fargate(Serverless), and select Amazon EC2 instances. Choose "Amazon Linux 2" for your operating system, "t2.micro" for your EC2 instance type, leave the rest of the settings default.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/6f84bac2-3a85-4f85-8620-ebd69782fe7b" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 3: Choose VPC you created, choose at least three subnets for production, and turn on Auto-assign public IP. Then click "create".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/597443f6-7bb0-41a4-b0f5-2f0d13f032d7" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Lab 1.4 ECR (Docker Repository) <br/>

<p align="center">
Step 1: Navigate to the "ECR Repositories" section and click "create repository". For visibility settings choose "Private", and for repository name write "my-microservice-repository". Leave "Tag Immutability" disabled.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/8771fe5d-4c82-4d2a-9b5a-bf636c37d7c5" alt="Disk Sanitization Steps"/>
<br />

<p align="center">
Step 2: Next, scroll down to the bottom and click "Create repository".      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/dfefa09e-0918-40cf-b450-1de45906bf87" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 3: Once you see your created repository, select it, and choose "View Push Commands". Here you will see your commands.       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/86209c62-267c-4d26-bdb9-1a52b13c7c42" alt="Disk Sanitization Steps"/>
<br />

<p align="center">
Lab 1.5 Tast Definitions <br/>

<p align="center">
Step 1: Navigate to the "Task Definitions" section and click "create new task definition". Next you will choose a name for your task definition. Use "my-task-definition".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/8cf007fb-469d-422a-8a05-36820f62593a" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 2: Under infrastructure requirements choose "Amazon EC2 instances" for your launch type. For operating system use "Linux/X86_64", network mode use "default", CPU use "1 vCPU", memory use "256GB", and for task role and execution role use "none".        <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/192d248e-16b2-4dfa-bb77-493beef7ceec" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 3: Name your container "my-image", copy and paste your repository URL in the "Image URL" section, change the container port number to "8080", change the "memory hard limit" to 256. Leave the rest of the settings default and click "create".         <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/1ecc3e7b-a0f5-4443-aa0a-2bdbd98553fb"/>
<br />

















  
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
