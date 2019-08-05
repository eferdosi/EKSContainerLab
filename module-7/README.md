# Module 7 - Deploy the Sample Application to your EKS Cluster


**Time to complete:** 20 minutes

**Services used:**

- Elastic Container Registry (ECR)

- Elastic Kubernetes Service (EKS)

### Overview

From the previous module, you should have obtained a Docker image that you are ready to push to ECR (Elastic Container Registry). [Amazon ECR](https://aws.amazon.com/ecr/) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images.

### Push Docker Image to ECR

First, to be able to access the ECR from the command line, you need to authenticate Docker to the ECR registry. There are 2 methods for authentication that can be found [here](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth). We use the 'get-login' method to obtain the login command. Run the following in the Command Line window and replace the <region> with the region that you want to create ECR repository:

```shell
aws ecr get-login --region <region> --no-include-email
```
The output of this command will give you the login command that you need to run to get authenticated. copy and paste the docker login command into the terminal and run it. You should now be authenticated!

Run the following command to find the image that you'll be pushing to ECR. It should have the name 'vlanimage':
```shell
docker images
```
_Tip:
First, If your image repository does not exist in the registry you intend to push to yet, create it. For more information, see [Creating a Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html)._

What you need from the list, is the "IMAGE ID" column of the vlanimage. Next, tag your image with the ECR repository name you've created:
```shell
docker tag <imageid> aws_account_id.dkr.ecr.region.amazonaws.com/<repository>
```

Now, push the image to ECR using docker push command. You need to replace the aws_account_id and the region with your own values.
```shell
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/<repository>
```

_Tip:
To find the Account ID, run the following command and the "Account" attribute value, is what you need to copy and paste into the above command:_
```shell
aws sts get-caller-identity 
```

Follow [instructions here](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) for full detail on how to login, tag and push your Docker image to ECR.

### Deploy Docker Image to EKS
Once you have pushed your image to ECR, you can deploy it to your EKS cluster that you setup in module 2. Kubernetes supports ECR as a private repository, [as detailed here](https://kubernetes.io/docs/concepts/containers/images/#using-aws-ec2-container-registry).

To deploy your image onto EKS, use the "kubectl run" command, passing in the name of your application and the URL to your image in ECR. You can also see the status of the deployment using the "kubectl get deployment" command.


## Next

Please proceed to the next lesson.

**[Proceed to Module 8](/module-8)**


### [AWS Developer Center](https://developer.aws)
