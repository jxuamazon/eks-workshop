---
title: "* Prepare the kubectl"
chapter: true
weight: 210
draft: false
tags:
  - advanced
  - operations
  - ci/cd
---

# Prepare your kubectl 

In this Chapter, we will deploy Jenkins using the `helm` package manager we
installed in the last module.

First setup some tools and environment variables , change the player number and namespace. 


{{% notice warning %}}
Please set your namespace with your player  number 
{{% /notice %}}

```
export MY_NAMESPACE=ns-<MY player number>
```

Now, let's install some tools ... 

```
sudo yum -y install jq gettext bash-completion


export MY_CLUSTER_NAME='eksworkshop-eksctl'
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')

#
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
echo "export MY_NAMESPACE=${MY_NAMESPACE}" | tee -a ~/.bash_profile
echo "export MY_CLUSTER_NAME=${MY_CLUSTER_NAME}" | tee -a ~/.bash_profile

# install kubectl
sudo curl --silent --location -o /usr/local/bin/kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/kubectl
sudo chmod +x /usr/local/bin/kubectl

#update awscli
sudo pip install --upgrade awscli && hash -r

#install jq, gettext
sudo yum -y install jq gettext bash-completion

# setup completion for kubectl
kubectl completion bash >>  ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion

# switch the instance profile to one with more powerful permission
curl -s http://169.254.169.254/latest/meta-data/instance-id | gawk '{print $1}' | xargs aws ec2 associate-iam-instance-profile --iam-instance-profile Name=eksworkshop-admin --instance-id "$@" --region=us-east-1
```


{{% notice warning%}}
Now turn of the "AWS managed temporary credentials" from Cloud9/Preference/AWS SETTINGS. 
{{% /notice %}}


```
aws sts get-caller-identity --query Arn | grep eksworkshop-admin -q && echo "IAM role valid" || echo "IAM role NOT valid"
```
{{% notice warning %}}
Only proceed with your see "IAM role valid"
{{% /notice %}}

Install eksctl
``` 
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv -v /tmp/eksctl /usr/local/bin
eksctl completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
```


Then setup kubectl with an eks cluster with the name "eksworkshop-eksctl"



```
aws eks --region $AWS_REGION update-kubeconfig --name $MY_CLUSTER_NAME
kubectl create namespace $MY_NAMESPACE
kubectl config set-context --current --namespace $MY_NAMESPACE

# ROLE_NAME is used everywhere in the workshop. 
STACK_NAME=$(eksctl get nodegroup --cluster ${MY_CLUSTER_NAME} -o json | jq -r '.[].StackName')
ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME --region=$AWS_REGION| jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')
echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile

```


Install helm


```sh
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

helm version --short

helm repo add stable https://kubernetes-charts.storage.googleapis.com/

helm search repo stable

helm completion bash >> ~/.bash_completion

. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
source <(helm completion bash)
```


