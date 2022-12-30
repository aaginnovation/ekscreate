<<<<<<< HEAD

# export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account) 
# export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
# export AZS=($(aws ec2 describe-availability-zones --query 'AvailabilityZones[].ZoneName' --output text --region $AWS_REGION))



# sudo curl --silent --location -o /usr/local/bin/kubectl \
#    https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.5/2022-01-21/bin/linux/amd64/kubectl

# sudo chmod +x /usr/local/bin/kubectl


# curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
# unzip awscliv2.zip
# sudo ./aws/install

# sudo apt -y install jq gettext bash-completion moreutils
=======
#export constant variable to be able to reuse in the process
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account) 
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
export AZS=($(aws ec2 describe-availability-zones --query 'AvailabilityZones[].ZoneName' --output text --region $AWS_REGION))


#Install Kubectl
sudo curl --silent --location -o /usr/local/bin/kubectl \
   https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.5/2022-01-21/bin/linux/amd64/kubectl
#Change mode of kubectl
sudo chmod +x /usr/local/bin/kubectl

#Install aws cli 
#Skip this step if you have your aws cli configured already
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

#Install jq into your server to help for bash completion
sudo apt -y install jq gettext bash-completion moreutils
>>>>>>> da488f41673d3b8bcf1463aca6fe1e8d80c3337b

# echo 'yq() {
#   docker run --rm -i -v "${PWD}":/workdir mikefarah/yq "$@"
# }' | tee -a ~/.bashrc && source ~/.bashrc


<<<<<<< HEAD
# Create admin iam role.


# curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
=======
#Create admin iam role.
#Create Admin Role for your cluster and name it.
#Attach this role to your server.


#Install eksctl

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
>>>>>>> da488f41673d3b8bcf1463aca6fe1e8d80c3337b

# sudo mv -v /tmp/eksctl /usr/local/bin

# eksctl version

#Deploy clusterconfig

<<<<<<< HEAD
# apiVersion: eksctl.io/v1alpha5
# kind: ClusterConfig
# metadata:
#   name: ekspractice
#   region: eu-west-3
#   version: "1.22"
# availabilityZones: ["eu-west-3a", "eu-west-3b", "eu-west-3c"]
# managedNodeGroups:
# - name: mynodegroup
#   desiredCapacity: 2
#   instanceType: t2.micro
#   ssh:
#     enableSsm: true
# secretsEncryption:
#   keyARN: arn:aws:kms:eu-west-3:135155369967:key/48e0e2bc-429b-40c6-ad64-fbec67ef2127
=======
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: ekspractice
  region: eu-west-3
  version: "1.22"
availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]
managedNodeGroups:
- name: mynodegroup
  desiredCapacity: 2
  instanceType: t2.micro
  ssh:
    enableSsm: true
secretsEncryption:
  keyARN: ${MASTER_ARN}
>>>>>>> da488f41673d3b8bcf1463aca6fe1e8d80c3337b

# eksctl version

#NOTE: You can deploy this if you want your cluster with kapenter autoscaler
# eksctl with karpenter

<<<<<<< HEAD
# apiVersion: eksctl.io/v1alpha5
# kind: ClusterConfig
# metadata:
#   name: ekspractice
#   region: eu-west-3
#   version: "1.22"
# availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]
# managedNodeGroups:
# - name: nodegroup
#   desiredCapacity: 3
#   instanceType: t3.small
#   ssh:
#     enableSsm: true
# iam:
#   withOIDC: true
# karpenter:
#   version: '0.15.0'
# secretsEncryption:
#   keyARN: ${MASTER_ARN}


# eksctl create cluster -f ekspractice.yaml




# Creating and AWS CUSTOM MANAGED KEY {CMK}
=======
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: ekspractice
  region: ${region}
  version: "1.22"
availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]
managedNodeGroups:
- name: nodegroup
  desiredCapacity: 3
  instanceType: t3.small
  ssh:
    enableSsm: true
iam:
  withOIDC: true
karpenter:
  version: '0.15.0'
secretsEncryption:
  keyARN: ${MASTER_ARN}

#create your cluster
eksctl create cluster -f ekspractice.yaml



#Create a CMK
Creating and AWS CUSTOM MANAGED KEY {CMK}
>>>>>>> da488f41673d3b8bcf1463aca6fe1e8d80c3337b

# aws kms create-alias --alias-name alias/ekspractice --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)

# export MASTER_ARN=$(aws kms describe-key --key-id alias/ekspractice --query KeyMetadata.Arn --output text)

# echo "export MASTER_ARN=${MASTER_ARN}" | tee -a ~/.bash_profile

<<<<<<< HEAD

# aws kms create-alias --alias-name alias/ekspractice --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)


# eksctl delete cluster --name=ekspractice


# helm repo add stable https://charts.helm.sh/stable
# helm repo add bitnami https://charts.bitnami.com/bitnami
=======
aws kms create-alias --alias-name alias/ekspractice --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)

#Clean your cluster
eksctl delete cluster --name=ekspractice

#Helm charts
helm repo add stable https://charts.helm.sh/stable
helm repo add bitnami https://charts.bitnami.com/bitnami
>>>>>>> da488f41673d3b8bcf1463aca6fe1e8d80c3337b
