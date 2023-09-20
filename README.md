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

<p align="center">
Lab 1.6 ECS Cluster Service <br/>

<p align="center">
Step 1: Navigate to the Navigate to the "Clusters" section and click on your cluster you previously created. Click on "services" and click "create". Choose "capacity provider strategy", then select "Use cluster default".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/1f7e45c2-b033-40ba-b194-10b10838add4" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 2: For application type select "service". On "Family", select your task definition that you created, service name use "my-microservice-task", and for service type select "Replica".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/fb067f86-3e21-428e-b1f9-994914231291" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 3: For "desired tasks" select "2", Min running tasks % select "100" and for Max running tasks select "200".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/aa808569-ef08-4f48-879d-829ed929f733" alt="Disk Sanitization Steps"/>
<br />  

<p align="center">
Step 4: Next select "Application Load Balancer" for your "Load balancer type". Under load balancer select the load balancer you created. Next choose your container to load balance. Select "Use an existing listener".       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/c2445b08-f981-4754-94e6-6de2275823a0" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 5: Next you will select "Use an existing target group" and name your target group "my-target-group". Next use "/health" for the health check path and use HTTP for your Health check protocol.       <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/89c8e5b0-20e1-4295-aa21-b9c2bec40180" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 6: Select "Use service auto scaling and select "2" for Minimum number of tasks and select "10" for Maximum number of tasks. Then select "Target tracking" for your Scaling policy type.      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/f978a3f1-4c45-4f27-b42a-e0ce09b8acbc" alt="Disk Sanitization Steps"/>
<br /> 

<p align="center">
Step 7: For policy name choose "cpu-tracking-policy", ECS Service metric select "ECSServiceAverageCPUUtilization, Target value select "50", and for Scale-out cooldown period and Scale-in cooldown period select "300". Then scroll down to the bottom and select "create".      <br/>
<img src="https://github.com/brycehallcloud/Microservices-Architecture-with-AWS-ECS/assets/144934324/1318f7bd-778e-4345-b623-8a9b065d9988" alt="Disk Sanitization Steps"/>
<br />

<p align="center">
Conclusion <br/>

<p align="center">
After following these steps you should have created an ECS Cluster with two instances which will contain the services deployed. Created a Load Balancer to have a single entry point and redirect all the request to all your existing services. Create a Task Definition for your microservice. Deploy a microservice in the cluster specifying the rollout strategy. And last, created Autoscaling Rules for your cluster and your service. <br/>







 



















  
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
