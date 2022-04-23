#           NodeJs-Project
## Table of contents
 - Introduction
 - Project Plan
 - Project Summary


### Introduction
In this project we use Nodejs web application , to bring up the infrastructure we on AWS Cloud  used terraform , to automate and monitoring used Kubernetes , Grafana.

### Project Plan
In this project we used some tools like :
 
 - Terraform
 - Docker
 - Kubernetes 
 - Jenkines
 - Grafana
 - Prometheus
 - fluentd/fluent-bit
 - Kibana
 - ElasticSearch

### Terraform
Terraform is an infrastructure as code (IaC) tool that allows you to build, change, and version infrastructure safely and efficiently. 
Terraform is an open-source infrastructure as code tool, mostly used for managing public cloud infrastructure such as AWS, GCP and Azure. 

### Docker
Docker is an operating system virtualization technology that allows applications to be packaged as containers. This is a very fundamental part of cloud computing, as containerized applications can be run on any type of infrastructure, regardless of the provider.

### Kubernetes
Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

### Jenkines
Jenkins is an open source continuous integration/continuous delivery and deployment (CI/CD) automation software DevOps tool written in the Java programming language. It is used to implement CI/CD workflows, called pipelines.

### Grafana
Grafana allows you to query, visualize, alert on, and understand your metrics no matter where they are stored. Create, explore, and share beautiful dashboards with your team and foster a data-driven culture.
Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources.

### Prometheus
Prometheus is a free software application used for event monitoring and alerting. It records real-time metrics in a time series database built using a HTTP pull model, with flexible queries and real-time alerting.

### Fluentbit
Fluent bit is an open source, light-weight, and multi-platform service created for data collection mainly logs and streams of data. Fluent bit service can be used for collecting CPU metrics for servers, aggregating logs for applications/services.

### Fluentd
Fluentd scraps logs from a given set of sources, processes them (converting into a structured data format) and then forwards them to other services like Elasticsearch, object storage etc. Fluentd is especially flexible when it comes to integrations – it works with 300+ log storage and analytic services.


### Kibana
Kibana is an free and open frontend application that sits on top of the Elastic Stack, providing search and data visualization capabilities for data indexed in Elasticsearch. Commonly known as the charting tool for the Elastic Stack (previously referred to as the ELK Stack after Elasticsearch, Logstash, and Kibana), Kibana also acts as the user interface for monitoring, managing, and securing an Elastic Stack cluster — as well as the centralized hub for built-in solutions developed on the Elastic Stack. 

### Elasticsearch
Elasticsearch is a distributed, free and open search and analytics engine for all types of data, including textual, numerical, geospatial, structured, and unstructured.

### Project Summary
#### Phase-1
Initially we created EC2 instance and installed Terraform . Then we wrote Terraform code by using code we created as follows, VPC , Subnet , IG (internet gateway) , NAT (Network Address Translation), Security group, EC2 instance(Jumpbox), EKS (Elastic Kubernetes Service ) , Nodes ,ECR (Elastic Container Registry) , S3 (Simple Storage Service).

Then we created one organization on Github in that  two different Repository in github and named as Devops , Production.
Terraform code has pushed to Devops Repo , and in Production repo we have Nodejs project .


#### Phase-2
Then we installed default-jdk , awscli and Jenkins in Jumpbox 

#### Phase-3
In this phase we installed Docker, Kubernetes , helmchat in Jumpbox.

In helmchat we do certain changes in values_dev.yaml file , ECR repo url , replicaCount 1 enabled LoadBalancer , port number , cpu and memory limits , enabling auto scaling min-1 to max-5 . And push the code to github repo.
Created three different namespaces in Eks Cluster
- Dev
- Qa
- Prod

> We need three different namespaces beacause:
> Dev environment is for developer Team.
> And Qa environment is for Testing Team.
> And Prod environment is for Production team .

We got the project of Nodejs , We have clone the Project code from the Github and we wrote the Docker file.
The Docker file contants some docker code to bring up the Project up.And we test the docker file by running it ,and it was running good.And we created ECR(Elastic Container Registry)
Then we write three Jenkins pipeline code and three shellscript code .
In that first code was Jenkins pipeline and shellscript, this code is for Dev environment.
This code will clone the docker file from github, login to ECR and build the docker file and tag the Docker file into two different type, 
> first one was tagging the docker images as dev-latest.
> second one was  tagging the docker images as latest commit_id . After tagging we push both tagged images to ECR. 

Next we will run helm chat and use set command to pull the latest commit_id image from ECR.

In helmchat we do certain changes in values-Qa.yaml file , ECR repo url , replicaCount 1 enabled LoadBalancer , port number , cpu and memory limits , enabling auto scaling min-1 to max-5 . And push the code to github repo.


In that second type , the Jenkins pipeline and shellscript, this code is for Qa environment.
And the code will  pull the latest images from ECR and build the docker file and tag the Docker file into two different type, 
> first one was tagging the docker images as Qa-latest.
> second one was  tagging the docker images as latest commit_id . After tagging we push both tagged images to ECR. 

Next we will run helm chat and use set command to pull the latest commit_id image from ECR.

In helmchat we do certain changes in values-Prod.yaml file , ECR repo url , replicaCount 1 enabled LoadBalancer , port number , cpu and memory limits , enabling auto scaling min-1 to max-5 . And push the code to github repo.
In that second type , the Jenkins pipeline and shellscript, this code is for Prod environment.
And the code will  pull the latest images from ECR and build the docker file and tag the Docker file into two different type, 
> first one was tagging the docker images as prod-latest.
> second one was  tagging the docker images as latest commit_id . After tagging we push both tagged images to ECR. 

Next we will run helm chat and use set command to pull the latest commit_id image from ECR.

Metrics server:
After deploying in prod next we will move on to Metrics server . 
Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines.
The Metrics server role is it will frequently checking the metrics of every running pods in EKS cluster . The main role of this server is it will help to Horizontal Pod Autoscaler and Vertical Pod Autoscaler.

> https://www.eksworkshop.com/beginner/080_scaling/deploy_hpa/

Cluster autoscaler:
The Kubernetes Cluster Autoscaler automatically adjusts the number of nodes in your cluster when pods fail or are rescheduled onto other nodes. The Cluster Autoscaler is typically installed as a Deployment in your cluster. It uses leader election to ensure high availability, but scaling is done by only one replica at a time.
We have installed Cluster autoscaler by refering below link .

> https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html

HPA:
Horizontal scaling means that the response to increased load is to deploy more Pods. This is different from vertical scaling, which for Kubernetes would mean assigning more resources.
If the load decreases, and the number of Pods is above the configured minimum, the HorizontalPodAutoscaler instructs the workload resource (the Deployment, StatefulSet, or other similar resource) to scale back down.
This configuration we have do in values_prod.yml file , there we will enable hpa .
Min pods - 1 and max pods - 5 . In that range only it will autoscale the pods .
This will be enabled only in prod environment .

> https://www.eksworkshop.com/beginner/080_scaling/deploy_hpa/

ALB [ Application loadbalancer ] using ingress :
Kubernetes Ingress is an API resource that allows you manage external or internal HTTP(S) access to Kubernetes services running in a cluster. Amazon Elastic Load Balancing Application Load Balancer (ALB) is a popular AWS service that load balances incoming traffic at the application layer (layer 7) across multiple targets, such as Amazon EC2 instances, in a region.

Following the steps 

- The controller watches for Ingress events from the API server. When it finds Ingress resources that satisfy its requirements, it starts the creation of AWS resources.

- An ALB is created for the Ingress resource.
- TargetGroups are created for each backend specified in the Ingress resource.
- Listeners are created for every port specified as Ingress resource annotation. If no port is specified, sensible defaults (80 or 443) are used.
- Rules are created for each path specified in your Ingress resource. This ensures that traffic to a specific path is routed to the correct TargetGroup created.

> https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/
> https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html

The above link is refered to deploy alb-ingress-controller.

- First we  create an IAM policy named ALBIngressControllerIAMPolicy to allow the ALB Ingress controller to make AWS API calls on your behalf. 

- Next, create a Kubernetes service account and an IAM role (for the pod running the AWS ALB Ingress controller) by substituting PolicyARN with the recorded value from the previous step.
- we have created aws-load-balancer-controller-service-account.yaml and make changes like role arn ,  name space.
-  This will be deployed in kube-system namespace.
- Finaly we will  deploy the AWS ALB Ingress controller.


#### Phase-4
## Monitoring tools installation n setup
Prometheus:
Prometheus is an open source tool for monitoring and alerting applications

- Prometheus Server: This component is the central component that collects the metrics from    multiple nodes. Prometheus uses the concept of scraping, where target systems’ metric endpoints  are contacted to fetch data at regular intervals.
- Node Exporter: This is called a monitoring agent which we installed on all the target machines so that Prometheus can fetch the data from all the metrics endpoints

- Alert Manager: Alert Manager is used to send the various alerts based upon the metrics data collected in Prometheus.
- Web UI: The web UI layer of Prometheus provides the end user with an interface to visualize data collected by Prometheus. In this, we will use Grafana to visualize the data.

Installation steps to Prometheus
- Now we will install the Prometheus on one of the EC2 Instance.

- Create a new user and add new directories
- Now we will configure Prometheus to monitor itself using yaml file. 
- We will give alert-manager target private-ip address in  Prometheus.yml file and we give refer to  alert.rules.yml
- In the alert.rules.yml file we will declare what and all the alert should be send to alert manager like HostOutOfMemory , HostHighCpuLoad , etc...
-  Now we will configure the service and start it . You can see the meterics in the browser by putting public-ip with the port number 9090
-  The meterices will be viewed oly of local machine because we didn't insall Node Exporter in the target machine 
> https://devops4solutions.com/monitoring-using-prometheus-and-grafana-on-aws-ec2/

### Install Node Exporter

Now to monitor your servers you need to install the node exporter on all your target machine which is like a monitoring agent on all the servers.
The  Node Exporter will collect all the metrices from the EC2 instance.

- we installed  Node Exporter by refering below link.

- After installing , we enable node-exporter and started node-exporter.
- The port to access the metrices is public-ip:9100.
- Need to give EC2ReadyOnlyAccess iam permission.
- Now you can see tagert machine metrices .

> https://devops4solutions.com/monitoring-using-prometheus-and-grafana-on-aws-ec2/

### Install Grafana

Once Prometheus is installed successfully then we can install the Grafana and configure Prometheus as a datasource.
Grafana is an opensource tool which is used to provide the visualization of your metrics.

- To install Grafana we refered below link .
- After installing Grafana , you can login to grafana by using port 3000.
- And going to Datasources and setting the ip in grafana.
- Then we will create Dashboard and import nessary dashboard in grafana.com
- Now we are importing 1860 to view the all the metrics from the node.


> https://devops4solutions.com/monitoring-using-prometheus-and-grafana-on-aws-ec2/

### Phase- 5 
### EFK [ Elasticsearch, Fluent-bit , and Kibana ]

### Elasticsearch:
   The  EFK stack is a collection of three open-source products Elasticsearch Fluent-bit and kibana. EFK stack provides centerallized logging in order to idetify problems with servers or applications. It will allow you to search all the logs in a single place. It also help to find issues in multiple servers by connecting  logs during a specific time frame.
  Installation of Elasticseach:
 - Elasticsearch is nothing but DB [ Database ] where the logs will be stored .
 - We reffered below link to install and congigure the elasticsearch .

-  After installing we make necessary changes like network.host , port address ,   discovery.seed_hosts in elasticsearch.yml file.
 - And allowed the port and started.
- This installation and configuration will be done in seperate EC2 instance [ should not install on jumpbox ]
   
   
>  https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-20-04
> https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-20-04
   
##### Fluentd:
Fluentd is will collect all the logs of pods in the cluster. Fluentd is deployed as a daemonset since it has to stream logs from all the nodes in the clusters.

Deploying Fluentd in Kubernetes as followes:
- By referring the below link we have make necessary changes in fluentd.yml file .
- Then we deployed it EKS [ Jumpbox with is connected to EKS ]
- It will be having the loggs of all the pods in EKS cluster , and it will be send to ElasticSearch .



### Kibana
Kibana is web application where we can see all the logs of pods.

We install kibana by reffering below link
After installing we can see in browser public-ip:5601

> https://www.elastic.co/guide/en/kibana/current/deb.html

