---
title: "Deploying ALB Ingress Controller"
date: 2019-04-09T00:00:00-03:00
weight: 14
draft: false
---

### Helm

We will use **Helm** to install the ALB Ingress Controller.
```
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```


Check to see if `helm` is installed:

```bash
helm version
```

```sh
helm completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
source <(helm completion bash)
```

Add helm incubator repository

```bash
helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
```

```bash
# Get the VPC ID
export VPC_ID=$(aws eks describe-cluster --name ${MY_CLUSTER_NAME} --query "cluster.resourcesVpcConfig.vpcId" --output text)


helm --namespace 2048-game install 2048-game \
  incubator/aws-alb-ingress-controller \
  --set image.tag=${ALB_INGRESS_VERSION} \
  --set awsRegion=${AWS_REGION} \
  --set awsVpcID=${VPC_ID} \
  --set rbac.create=false \
  --set rbac.serviceAccount.name=alb-ingress-controller \
  --set clusterName=${MY_CLUSTER_NAME}
```

Execute the following command to watch the progress by looking at the deployment status:

```bash
kubectl -n 2048-game rollout status \
  deployment 2048-game-aws-alb-ingress-controller
```

Output:
{{< output >}}
Waiting for deployment "2048-game-aws-alb-ingress-controller" rollout to finish: 0 of 1 updated replicas are available...
deployment "2048-game-aws-alb-ingress-controller" successfully rolled out
{{< /output >}}

```bash
kubectl get pods -n 2048-game
```

Output:
{{< output >}}
NAME                                                    READY   STATUS    RESTARTS   AGE
2048-deployment-7f77b47f7-fvrqj                         1/1     Running   0          17m
2048-deployment-7f77b47f7-pvjpk                         1/1     Running   0          17m
2048-deployment-7f77b47f7-pvlmv                         1/1     Running   0          17m
2048-deployment-7f77b47f7-q66n9                         1/1     Running   0          17m
2048-deployment-7f77b47f7-x6sjc                         1/1     Running   0          17m
2048-game-aws-alb-ingress-controller-569d6cbfc8-nfh6c   1/1     Running   0          69s
{{< /output >}}
