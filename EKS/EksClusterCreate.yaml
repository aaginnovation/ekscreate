#install aws-cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

#Install Kubectl
sudo curl --silent --location -o /usr/local/bin/kubectl \
   https://s3.us-west-2.amazonaws.com/amazon-EKS/1.21.5/2022-01-21/bin/linux/amd64/kubectl
   
#Change mode of kubectl
sudo chmod +x /usr/local/bin/kubectl

#Export Account variable
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account) 

#install jq bash
sudo apt -y install jq gettext bash-completion moreutils

#Install EKSctl
curl --silent --location "https://github.com/weaveworks/EKSctl/releases/latest/download/EKSctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv -v /tmp/eksctl /usr/local/bin
eksctl version

#Create a CMK
#Creating and AWS CUSTOM MANAGED KEY 
aws kms create-alias --alias-name alias/my-eks-cluster --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)
export MASTER_ARN=$(aws kms describe-key --key-id alias/my-eks-cluster --query KeyMetadata.Arn --output text)

#export MasterARN
echo "export MASTER_ARN=${MASTER_ARN}" | tee -a ~/.bash_profile

#create EKS cluster config file 
vi eks-cluster-config.yaml

apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: my-eks-cluster
  region: us-east-1
  version: "1.28"
availabilityZones: ["us-east-1a", "us-east-1b"]
managedNodeGroups:
- name: mynodegroup
  desiredCapacity: 2
  instanceType: t2.medium
  ssh:
    enableSsm: true
  iam:
    withAddonPolicies:
      imageBuilder: true
      autoscaler: true
      externalDNS: true
      certManager: true
      ebs: true
      albIngress: true
      xRay: true
iam:
  withOIDC: true
secretsEncryption:
  keyARN: ${MASTER_ARN} #Replace ${MASTER_ARN} with exported value

#Deploy your EKS cluster

eksctl create cluster -f eks-config.yaml
