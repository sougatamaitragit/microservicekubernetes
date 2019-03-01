# Spring Microservice Using Kubernetes
Spring Boot Kubernetes Microservice Example

In oreder to implement microservice using kubernetes we first need to create a kubernetes cluster ( coniating Master and Worker Nodes) . In currrent POC I have used AWS Cloud as the cloud provider where i have 2 worker nodes and 1 master node. In oreder to create Kubernetes cluster in AWS , we need to have KOPS utility . So we first install KOPS in a simple node and then use appropriate KOPS commands to create kubernetes cluster in AWS. Following steps are follwed to create Kubernetes Clusters using KOPS.

1. Take a AWS EC2 instance and install kubectl . Issue following commands to install kubectl in an AWS EC2 ubuntu instance - 
  
      apt-get update && apt-get install -y apt-transport-https
      
      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
      
      echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
      
      apt-get update
      
      apt-get install -y kubectl
  
 2. Install KOPS in that EC2 instance - In order to install KOPS issue following commands - 
 
    wget https://github.com/kubernetes/kops/releases/download/1.10.0/kops-linux-amd64
    
    chmod +x kops-linux-amd64 [ required based on your permission ]

    mv kops-linux-amd64 /usr/local/bin/kops

3. Install AWS CLI - to install AWS CLI issue following command 
    
    apt install python-pip 
    
    pip install awscli
    
    
4. Create a AWS User and give the user following permissions 
  
  AmazonEC2FullAccess
  
  AmazonRoute53FullAccess
  
  AmazonS3FullAccess
  
  IAMFullAccess
  
  AmazonVPCFullAccess
  
  Here I create an user called kops and assign the required accessess. 
  
 
5. Create AWS Route 53 - DNS is optional in case , kops version is 1.6.2 or more . Kubernetes uses DNS name to lookup etcd and nodes uses DNS to communicate with masters . Issue command below to create a Route hosted Zone 

aws route53 create-hosted-zone --name sample.microservice.com --caller-reference 1

6. Create AWS S3 bucket - Kubernetes clustser's uses AWS S3 bucket's to store information about cluster like Number of Nodes , instance type of each node , kubernetes version . During cluster formation these states are stored and any subsequent changes in cluster formation are also stored in the bucket. To create S3 bucket issue below command - 

    aws s3 mb s3://bucket.sample.microservice.com

7. Prepare environment variable - 

export followings 

export KOPS_STATE_STORE=s3://bucket.sample.microservice.com

export KOPS_CLUSTER_NAME=cluster.sample.microservice.com

8. If you are using ubuntu please use ssh-keygen commmands to generate rsa key 

9.  Create cluster configuration - Issue following commands to create cluster definition . This command will not create cluster physcially, instead this will prepare a cluster definition .

 kops create cluster --node-count=2 --node-size=t2.medium --zones=us-east-1a --name=${KOPS_CLUSTER_NAME}  --ssh-public-key ~/.ssh/id_rsa.pub

10. Form Physical CLuster - Issue below commands to create physical cluster . 

kops upgrade cluster --name cluster.sample.microservice.com --state s3://bucket.sample.microservice.com --yes

11. Validate Cluster formation and check connection of kubectl 
