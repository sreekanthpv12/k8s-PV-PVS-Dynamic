# k8s-PV-PVS-Dynamic

Installing and Configuring the AWS EBS CSI Driver for Kubernetes Cluster with Dynamic Provisioning of EBS Volumes
=================================================================================================================
This guide provides the steps to install and configure the AWS EBS CSI Driver on a Kubernetes cluster to enable the use of Amazon Elastic Block Store (EBS) volumes as persistent volumes.


Prerequisites
Before you begin, ensure that you have the following prerequisites in place:

A running Kubernetes cluster.
Kubernetes version 1.20 or newer.
An AWS account with permissions to create an IAM user and obtain access key and secret key.

Installing Helm
Helm is a package manager for Kubernetes that simplifies the deployment and management of applications.

To install Helm, execute the following commands:

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
===
Installing the AWS EBS CSI Driver
Now, let's install the AWS EBS CSI Driver on your Kubernetes cluster. Follow these steps:

Create a secret to store your AWS access key and secret key. Replace <YourAWSAccessKey> and <YourAWSSecretKey> with your actual AWS credentials:

kubectl create secret generic aws-secret \
    --namespace kube-system \
    --from-literal "key_id=<YourAWSAccessKey>" \
    --from-literal "access_key=<YourAWSSecretKey>"
===
Add the AWS EBS CSI Driver Helm chart repository:

helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm repo update
===
Deploy the AWS EBS CSI Driver using the following command:

helm upgrade --install aws-ebs-csi-driver \
    --namespace kube-system \
    aws-ebs-csi-driver/aws-ebs-csi-driver
===
Verify that the driver has been deployed and the pods are running:

kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-ebs-csi-driver
===
Provisioning EBS Volumes
Now, let's provision EBS volumes for your applications. Follow these steps:

Create a storageclass.yaml file to define your storage class settings and apply it:

kubectl apply -f storageclass.yaml
===
Create a pvc.yaml file to define your Persistent Volume Claims and apply it:

kubectl apply -f pvc.yaml
===
Create a pod.yaml file to define your pods and apply it:

kubectl apply -f pod.yaml
===
Verify that the EBS volume has been provisioned and attached to the pod:

kubectl exec -it app -- df -h /data
  
The output of the above command should show the mounted EBS volume and its available disk space.

Conclusion
In this guide, you've learned how to install and configure the AWS EBS CSI Driver on your Kubernetes cluster and how to use it for dynamic provisioning of EBS volumes.
===================================================================================================================================================================================
