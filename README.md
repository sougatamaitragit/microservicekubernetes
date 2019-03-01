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
  
 
 
