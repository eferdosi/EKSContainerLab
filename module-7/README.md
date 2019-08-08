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

To deploy your image onto EKS, use the "kubectl run" command, passing in the name of your application and the URL to your image in ECR. You can also see the status of the deployment using the "kubectl get deployment" command.


## Next

Please proceed to the next lesson.

**[Proceed to Module 8](/module-8)**


### [AWS Developer Center](https://developer.aws)
