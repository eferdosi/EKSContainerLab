
# Module 2: Configure your Amazon EKS Cluster 

**Time to complete:** 15 minutes

**Services used:**
* [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
* [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/)


## Overview

In this module, we will use `eksctl` to launch and configure our EKS cluster and worker nodes.

**[Eksctl](https://eksctl.io/)** is a tool jointly developed by AWS and [Weaveworks](https://www.weave.works/) that automates much of the experience of creating EKS clusters.

Alternative approaches for EKS cluster creation include using CloudFormation, the AWS CLI, the AWS Console, or several third-party automation tools.  A complete reference architecture is available [here](https://github.com/thestacks-io/eks-cluster).


## Client Setup

Kubernetes uses the `kubectl` command-line utility for communicating with the EKS Cluster API Server.  Recall that you previously installed `kubectl` in module-1 of this lab. 

1. Using your local Terminal application, login to your remote Linux Dev Environment (configured in module-1).

``` shell
ssh -i MYKEYPAIR.pem ec2-user@myEc2IpAddress
```
```
login examples:
ssh -i ~\downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
ssh -i %HOME%\Downloads\mykeypair.pem ec2-user@ec2-34-219-242-224.us-west-2.compute.amazonaws.com
```

2. Verify that `kubectl` is installed and review the commands available.

``` shell
kubectl
```

3. Amazon EKS uses IAM to provide authentication to your Kubernetes cluster via the [AWS IAM Authenticator for Kubernetes](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html).  Download and install the `aws-iam-authenticator` as follows.

``` shell
sudo curl -L https://amazon-eks.s3-us-west-2.amazonaws.com/1.13.7/2019-06-11/bin/linux/amd64/aws-iam-authenticator -o /usr/local/bin/aws-iam-authenticator
```
``` shell
sudo chmod +x /usr/local/bin/aws-iam-authenticator
```

4. Test that the `aws-iam-authenticator` works.

``` shell
aws-iam-authenticator help
```


## Create Cluster

Now you can create your Amazon EKS cluster and a worker node group using the `eksctl` utility.

Many cluster configuration options are available as `eksctl` [documentation](https://github.com/weaveworks/eksctl/blob/master/README.md) illustrates.  However, for our purposes, the default configuration is adequate.

1. Create your cluster as follows.

``` shell
eksctl create cluster
```

2. You may check the progress of your cluster creation using the events tab of the [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/home).

3. The above procedure will take several minutes to complete. In the interim, you may [proceed to Module 3](/module-3) or just wait here. Remember to return to the next step herein. 


## Connect to your Cluster

1. When your cluster is ready, test that your `kubectl` configuration is correct.

``` shell
kubectl get svc
```

2. You should see output similar to the following. If not, refer to the [AWS documentation](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html) for getting started with `eksctl`.

``` 
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
svc/kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   1m
```

That concludes Module 2.

**[Proceed to Module 3](../module-3/README.MD)**


### [AWS Developer Center](https://developer.aws)
