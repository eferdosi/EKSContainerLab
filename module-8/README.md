# Module 8 - Deploy Application to Elastic Kubernetes Service (EKS) 

**Time to complete:** 20 minutes

**Services used:**

- Elastic Kubernetes Service (EKS)

- Simple Storage Service (S3)

### Overview

In this module, we will deploy our application to the EKS Cluster and test it to check if it run properly on our EKS cluster.

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
kubectl apply -f <S3 Url for Yaml file>
```
By running this command, a deployment will start and new Pod with the given container image will be run. Now, Let's test the application and see the output.

### Test the Application

Previous command will create a Pod in our cluster. A Pod is a group of one or more containers (such as Docker containers), with shared storage/network, and a specification for how to run the containers. You can find more details [here](https://kubernetes.io/docs/concepts/workloads/pods/pod/#what-is-a-pod).

To see the pods created from the previous command, run the following command:

```shell
kubectl get pods
```
in the output, you should see the 'vlanmigrateapp' pod with status 'Completed'. That means that app has run and completed successfully. Note that this is a console application and will terminate upon completion. For other application types, such as web services, the status will be 'Running', that means the app has been deployed successfully and running.

To see the output of the console application, run the following command:

```shell
kubectl log vlanmigrateapp
```
The output should be similar to what you saw whe run the application in Visual Studio. 

![Log Outputs](/images/module-7/Output.jpg)

## Congratulations, You have successfully completed the migration of a .NET application and deployed it onto EKS.##

### [AWS Developer Center](https://developer.aws)
