# Module 7 - Deploy the Sample Application to your EKS Cluster


**Time to complete:** 20 minutes

**Services used:**

- Elastic Container Registry (ECR)

- Elastic Kubernetes Service (EKS)

### Overview

From the previous module, you should have obtained a Docker image that you are ready to push to ECR (Elastic Container Registry). [Amazon ECR](https://aws.amazon.com/ecr/) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images.

### Push Docker Image to ECR

Accessing the ECR and pushing images into the repository can be done in two different methods. AWS CLI via Command Line or AWS Toolkit for Visual Studio. In this module we use the AWS Toolkit for Visual Studio.

In Visual Studio, right click on the VLanMigrate project and select 'Publish Container to AWS':

![Publish Container to AWS](/images/module-7/PublishContainer1.jpg)

This will open the publish window where you need to enter a few parameters as below:
1. __Region__: the region that your image repository will be created.
2. __Docker Repository__: the repository name
3. __Tag__: Docker Image Tag (Optional). If you leave it blank, it will be set to 'latest'
4. __Deployment Target__: click on the list and choose 'Push only docker image to the Amazon Elastic Container Registry'

![Publish Container to AWS](/images/module-7/PublishContainer2.jpg)

Now, click on the 'Publish' command and the process should be completed quickly. Once finished, open the 'AWS Toolkit' window from the 'View > AWS Toolkit' menu and on the tree, expand 'Amazon Elastic Container Service > Repositories' and you should see your newly created image. 

![AWS Toolkit](/images/module-7/PublishContainer3.jpg)

Double-click on the repository name and in the new window you can see the image detail including the URL that we will use it later to push to EKS.

![Repository Detail](/images/module-7/PublishContainer4.jpg)

_Tip:_

_All of the above steps can be done via AWS CLI in Command Line. Follow [instructions here](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) for full detail on how to login, tag and push your Docker image to ECR._

### Deploy Docker Image to EKS
Once you have pushed your image to ECR, you can deploy it to your EKS cluster that you setup in module 2. Kubernetes supports ECR as a private repository, [as detailed here](https://kubernetes.io/docs/concepts/containers/images/#using-aws-ec2-container-registry).

To deploy the image onto EKS, we need to first create the YAML file that contains the Pod configuration, then upload that file into a location that can be accessed by EKS cluster and finally run the application in EKS cluster.

To start, create a yaml file with following lines and save it into your local machine as 'eks-config.yaml'. Please note that you need to replace the 'image' attribute with your image URL in ECR. 

```shell
apiVersion: v1
kind: Pod
metadata:
  name: vlanmigrateapp
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: vlan-test-container
    image: <Image URL in ECR>
    imagePullPolicy: Always
    args: ["-f","TEST-DATA/SampleVLANs.txt","-e",TEST-DATA/Exceptions0001.log","-o","TEST-DATA/Router001.txt","-p","TEST-DATA/Router002.txt","-r","false"]
  restartPolicy: OnFailure
```

Next, upload the file into a s3 bucket and make sure that the file is available to public. This is not a recommended method to be used in production environment, but for the purpose of this workshop, we make the file public.

Connect to Linux workstation that you setup in very first module, using the ssh command. You already have configured the EKS cluster and nodes on this instance which can be listed using these commands:

```shell
kubectl get svc
kubectl get nodes
```

Now, using the kubectl apply command and given yaml file, we will run the application in the cluster:

```shell
kubectl apply -f <Image url in ECR>
```
By running this command, a deployment will start and new Pod with the given container image will be run. To see the pods run the following command:

```shell
kubectl get pods
```
in the output, you should see the 'vlanmigrateapp' pod with status 'Completed'. That means that app has run and completed successfully. Please note that this is a console application and will terminate upon completion. For other application types, such as web services, the status will be 'Running'.

To see the output of the console application, run the following command:

```shell
kubectl log vlanmigrateapp
```
The output should be similar to what you saw whe run the application in Visual Studio. 

![Log Outputs](/images/module-7/Output.jpg)

## Congratulations, You have successfully completed the migration of a .NET application and deployed it onto EKS.##

**[Proceed to Module 8](../module-8/README.md)**

### [AWS Developer Center](https://developer.aws)
