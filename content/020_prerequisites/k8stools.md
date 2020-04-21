---
title: "Install Kubernetes Tools"
chapter: false
weight: 15
---

Amazon EKS clusters require kubectl and kubelet binaries and the aws-cli or aws-iam-authenticator
binary to allow IAM authentication for your Kubernetes cluster.

{{% notice warning %}}
Set up your cluster name
```
export MY_CLUSTER_NAME='eskworkshop-eksctl-<you-player-numnber>'
```
{{% /notice %}}

#### Install kubectl
```
sudo curl --silent --location -o /usr/local/bin/kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/kubectl
sudo chmod +x /usr/local/bin/kubectl
```
#### Update awscli

Upgrade AWS CLI according to guidance in [AWS documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-linux.html).

```
sudo pip install --upgrade awscli && hash -r
```

#### Install jq, envsubst (from GNU gettext utilities) and bash-completion
```
sudo yum -y install jq gettext bash-completion
```

#### Verify the binaries are in the path and executable
```
for command in kubectl jq envsubst aws
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done
```

#### Enable kubectl bash_completion
```
kubectl completion bash >>  ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
```
