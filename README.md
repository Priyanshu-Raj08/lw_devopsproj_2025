# DevOps project
## Overview
### This project showcases the deployment and management of a containerized application using AWS Elastic Kubernetes Service (EKS), ArgoCD, and Istio Service Mesh. It offers practical experience in setting up a comprehensive DevOps pipeline, managing Kubernetes clusters, and facilitating seamless application delivery through GitOps practices.

## Prerequisites

Ensure the following tools are installed and configured on your system:

### AWS Account: Ensure your account is active.  
Git Bash: [Download Git Bash](https://git-scm.com/downloads).  
AWS CLI: [Download AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).  
kubectl: [Download kubectl](https://kubernetes.io/docs/tasks/tools/).  
eksctl: [Download eksctl](https://eksctl.io/installation/).  
Istio CLI: [Download Istio CLI](https://eksctl.io/installation/).  
ArgoCD: [Install ArgoCD (Releases)](https://eksctl.io/installation/).  

**Important: Set up the path as environment variables for AWS cli, kubectl, eksctl, Istioctl

## Deployment

## 1. Configure Aws cli
### Enter your aws credentials like private access key and security key.
```
aws configure
```
## 2. Creating kubernetes cluster
### Execute the command below to start a cluster

```
eksctl create cluster --name devopsprojectcluster
```
**Note: Creating cluster will result in bill addonsRefer to [AWS pricing details](https://aws.amazon.com/pricing/)**

## 3. Installing Argocd
- Creating namespace for ArgoCd
```
kubectl create namespace argocd
```
- Apply ArgoCD installation
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- Verifying the pods
```
kubectl get pods -n argocd
```
## 4. Access the ArgoCD dashboard
- Port forward the ArgoCD service:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
- Access the dashboard using the public ip
  . Username should be admin.  
  . Password would be obtained using:  
```
kubectl config set-context --current --namespace=argocd
```
```
argocd login –-core
```
```
argocd admin initial-password -n argocd
```
## 5. Deploy the application
1. Clone the repository
 ```
git clone https://github.com/your-repo/project.git
cd project
```
2. Add and commit the files for your application
```
git add .
git commit -m "Initial commit"
git push
```
3. In the ArgoCD dashboard, configure a new application:
○ Name: Your application name  
○ Source: Your Git repository URL and file path  
○ Destination: Kubernetes cluster and namespace (default by default)  
○ Policy: Automatic synchronization  

## 6. Installing istio
- Installing demofile:
```
istioctl install --set profile=demo
```
- istioctl install --set profile=demo:
```
kubectl label namespace default istio-injection=enabled
```
- Deleting and restarting the pods:
```
kubectl delete pods --all -n default
```
- Deploy istio addons:
  ○ Do locate to the samples folder inside the istio file
```
kubectl apply -f addons/
```
- Generate traffic for testing:
```
while :; do curl -s -o /dev/null -w "%{http_code}" http://<your-service-url>/productpage; done
```

## 8. Monitoring and managing the traffics

- Using istioctl open the dashboards
```
istioctl dashboard kiali
```
```
istioctl dashboard prometheus
```
```
istioctl dashboard grafana
```
## 9. Security analysis
- Use Trivy or SonarQube for scanning vulnerabilities in your Kubernetes setup and application.

## Conclusion
### This project helps you practice cloud-native application deployment and management using industry-standard tools. With ArgoCD, you can maintain GitOps workflows, and Istio enhances observability, traffic control, and security of your services. This hands-on guide empowers you to build and manage scalable, resilient systems effectively.

## App resource
- https://github.com/vimallinuxworld13/eks_istio_bookinfo_app/tree/master
