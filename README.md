# Trendify – DevOps CI/CD Deployment Project

## 1. Project Overview

Trendify is a production-ready web application deployed using AWS and an end-to-end DevOps CI/CD workflow.

This project demonstrates:

- Docker containerization
- DockerHub image management
- Terraform Infrastructure as Code
- AWS EC2 with Jenkins
- AWS EKS
- Kubernetes deployment
- Kubernetes LoadBalancer
- AWS Network Load Balancer
- GitHub version control
- GitHub webhook integration
- Jenkins CI/CD
- Prometheus monitoring
- Grafana dashboards

The application runs on port **3000**.

---

## 2. Architecture

```text
Developer
    |
    | git push
    v
GitHub
    |
    | GitHub Webhook
    v
Jenkins EC2
    |
    +----------------------+
    |                      |
    v                      v
Docker Build         DockerHub Push
    |                      |
    +----------+-----------+
               |
               v
            AWS EKS
               |
               v
     Kubernetes Deployment
               |
          +----+----+
          |         |
          v         v
        Pod       Pod
          \         /
           \       /
            v     v
      Kubernetes Service
               |
               v
     AWS Network Load Balancer
               |
               v
         Trendify :3000
```

---

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| React | Application |
| Nginx | Production web server |
| Docker | Containerization |
| DockerHub | Container image registry |
| Terraform | Infrastructure as Code |
| AWS EC2 | Jenkins server |
| AWS VPC | Network infrastructure |
| AWS IAM | Access management |
| AWS EKS | Kubernetes cluster |
| Kubernetes | Application orchestration |
| Jenkins | CI/CD automation |
| GitHub | Version control |
| GitHub Webhook | Automatic pipeline trigger |
| Prometheus | Metrics collection |
| Grafana | Monitoring dashboards |
| Alertmanager | Alert management |
| Node Exporter | Node metrics |
| kube-state-metrics | Kubernetes resource metrics |

---

## 4. Application Deployment

The provided Trend application was deployed as a production-ready static web application.

The application is served through Nginx inside a Docker container.

### Application Port

`3000`

The application was successfully accessed through the AWS Network Load Balancer.

---

## 5. Docker

The application was containerized using Docker and Nginx.

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY dist/ /usr/share/nginx/html/

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
```

### Build Docker Image

```bash
docker build -t anbansiddharthan/trend-app:latest .
```

### Run Docker Container

```bash
docker run -d --name trend-app -p 3000:3000 anbansiddharthan/trend-app:latest
```

The Docker image was successfully built and tested.

---

## 6. DockerHub

The Docker image was pushed to DockerHub.

### Repository

`anbansiddharthan/trend-app`

### Image

`anbansiddharthan/trend-app:latest`

Jenkins automatically builds and pushes the Docker image during the CI/CD pipeline.

---

## 7. Terraform Infrastructure

Terraform was used as Infrastructure as Code to provision the AWS infrastructure.

The Terraform configuration is located in:

`terraform/main.tf`

The infrastructure includes:

- VPC
- Subnet
- Internet Gateway
- Route Table
- Security Group
- IAM Role
- IAM Instance Profile
- Jenkins EC2 instance

### Terraform Commands

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

---

## 8. Jenkins

Jenkins was installed and configured on an AWS EC2 instance.

The Jenkins environment was configured with the tools required for the CI/CD workflow:

- Java
- Jenkins
- Git
- Docker
- AWS CLI
- kubectl
- Helm

Jenkins was integrated with GitHub using a webhook.

---

## 9. AWS EKS

The application was deployed to an AWS EKS Kubernetes cluster.

### AWS Region

`eu-north-1`

The EKS cluster and worker nodes were successfully verified.

The worker nodes were in the `Ready` state.

---

## 10. Kubernetes Deployment

The Kubernetes Deployment configuration is located at:

`k8s/deployment.yaml`

The application runs with **2 replicas**.

The Docker image used by the deployment is:

`anbansiddharthan/trend-app:latest`

### Deploy Application

```bash
kubectl apply -f k8s/deployment.yaml
```

### Check Pods

```bash
kubectl get pods
```

### Verify Deployment

```bash
kubectl rollout status deployment/trend-app
```

The deployment successfully rolled out with two running application pods.

---

## 11. Kubernetes Service

The Kubernetes Service configuration is located at:

`k8s/service.yaml`

The service type is:

`LoadBalancer`

The application is exposed on port `3000`.

The service was configured to use an AWS Network Load Balancer.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: trend-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
spec:
  type: LoadBalancer
  selector:
    app: trend-app
  ports:
    - port: 3000
      targetPort: 3000
      protocol: TCP
```

### Apply Service

```bash
kubectl apply -f k8s/service.yaml
```

### Check Service

```bash
kubectl get service trend-service
```

---

## 12. AWS Network Load Balancer

The Kubernetes `LoadBalancer` service created an AWS Network Load Balancer.

### Load Balancer DNS

`a1692eb15631e4b478af0da9abc66a1d-9ced16a22069ad18.elb.eu-north-1.amazonaws.com`

### Application Port

`3000`

### Load Balancer ARN

```text
arn:aws:elasticloadbalancing:eu-north-1:858656722878:loadbalancer/net/a1692eb15631e4b478af0da9abc66a1d/9ced16a22069ad18
```

The application was successfully accessed through the Load Balancer.

---

## 13. Jenkins CI/CD Pipeline

The declarative Jenkins pipeline is defined in:

`Jenkinsfile`

The pipeline performs the following stages:

1. Checkout
2. Build Docker Image
3. Push to DockerHub
4. Deploy to EKS
5. Verify Deployment

### Pipeline Flow

```text
GitHub
   |
   | Webhook
   v
Jenkins
   |
   v
Checkout
   |
   v
Build Docker Image
   |
   v
Push to DockerHub
   |
   v
Deploy to EKS
   |
   v
Verify Deployment
```

The Jenkins pipeline completed successfully.

---

## 14. GitHub Integration

The project is maintained in GitHub.

### Repository

`https://github.com/AnbanSiddharthan/trend-devops-project`

Git was used through the command line to commit and push the project.

```bash
git status
git add .
git commit -m "Update project"
git push origin main
```

---

## 15. GitHub Webhook

A GitHub webhook was configured to automatically trigger the Jenkins pipeline whenever a commit is pushed.

The workflow is:

```text
git push
    |
    v
GitHub
    |
    v
GitHub Webhook
    |
    v
Jenkins
    |
    v
CI/CD Pipeline
```

The webhook was successfully tested.

Jenkins displayed:

```text
Started by GitHub push by AnbanSiddharthan
```

This confirms that GitHub push events automatically trigger the Jenkins pipeline.

---

## 16. Monitoring

An open-source monitoring stack was configured for the EKS cluster.

The monitoring stack consists of:

- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- kube-state-metrics

The monitoring components were deployed in the Kubernetes namespace:

`monitoring`

### Verify Monitoring

```bash
kubectl get pods -n monitoring
```

The monitoring components were successfully verified as running.

### Prometheus

Prometheus collects metrics from the Kubernetes cluster and nodes.

### Grafana

Grafana provides dashboards for visualizing Kubernetes cluster metrics.

The Grafana Kubernetes dashboard was successfully verified with live metrics.

### Alertmanager

Alertmanager handles alerts generated by Prometheus.

### Node Exporter

Node Exporter collects system-level metrics from Kubernetes nodes.

### kube-state-metrics

kube-state-metrics provides Kubernetes resource and object state metrics to Prometheus.

---

## 17. Repository Structure

```text
Trend/
│
├── dist/
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── terraform/
│   └── main.tf
│
├── Dockerfile
├── nginx.conf
├── Jenkinsfile
├── .gitignore
├── .dockerignore
└── README.md
```

---

## 18. Verification

The complete CI/CD deployment was successfully verified.

### Docker

Docker image successfully built:

`anbansiddharthan/trend-app:latest`

### DockerHub

Docker image successfully pushed to DockerHub.

### Jenkins

Pipeline completed successfully:

```text
Finished: SUCCESS
```

### Kubernetes

Two application replicas were running successfully.

```text
1/1 Running
1/1 Running
```

### EKS

Worker nodes were successfully verified as:

`Ready`

### Load Balancer

AWS Network Load Balancer was successfully created.

### GitHub Webhook

Jenkins successfully received a GitHub push event:

```text
Started by GitHub push by AnbanSiddharthan
```

### Monitoring

Prometheus and Grafana were successfully deployed.

Grafana successfully displayed live Kubernetes cluster metrics.

---

# 19. Screenshots / Evidence

The following screenshots provide evidence of the implementation and successful deployment.

## 19.1 Application

<img width="1915" height="965" alt="image" src="https://github.com/user-attachments/assets/89b3f580-1dc4-4f09-a421-b03552379e02" />


---

## 19.2 Docker Build

<img width="1477" height="577" alt="image" src="https://github.com/user-attachments/assets/abf1ca72-0345-400c-a8ce-18acf5f3f6df" />

---

## 19.3 DockerHub

<img width="1912" height="910" alt="image" src="https://github.com/user-attachments/assets/0ef7e1a6-8a51-422e-80f1-707d3e9b33ce" />

---

## 19.4 Jenkins

<img width="1917" height="960" alt="image" src="https://github.com/user-attachments/assets/016d3316-52b7-4136-9011-6e0dd06df8c5" />

---

## 19.5 Jenkins Successful CI/CD Pipeline

<img width="1917" height="912" alt="image" src="https://github.com/user-attachments/assets/fc764928-03d3-4459-b594-ec0b597bf955" />

---

## 19.6 GitHub Repository

<img width="1915" height="907" alt="image" src="https://github.com/user-attachments/assets/881a129f-6df0-4da3-8f4f-4a6df8bfbe21" />

---

## 19.7 GitHub Webhook

<img width="1913" height="918" alt="image" src="https://github.com/user-attachments/assets/4e7361b8-ec3c-46b7-87d4-6aab0c8506a8" />

---

## 19.8 AWS EKS Cluster

<img width="1917" height="861" alt="image" src="https://github.com/user-attachments/assets/062ce91a-62e5-4b05-8564-aacf98fd836f" />

---

## 19.9 Kubernetes Pods

<img width="1472" height="140" alt="image" src="https://github.com/user-attachments/assets/3cb5e5ea-3b6f-4437-a0b0-94ab1bbca4e3" />

---

## 19.1 Kubernetes Service

<img width="1478" height="172" alt="image" src="https://github.com/user-attachments/assets/0c8abc52-629f-47db-bef5-a8f5cdde0500" />

---

## 19.11 AWS Network Load Balancer

<img width="1917" height="806" alt="image" src="https://github.com/user-attachments/assets/94258f5b-d5a5-4700-8a13-e198184ac68b" />

---

## 19.12 Prometheus

<img width="1473" height="130" alt="image" src="https://github.com/user-attachments/assets/eb0aff8a-cc16-4e79-a2b6-6d87f5784812" />

---

## 19.13 Grafana Monitoring Dashboard

<img width="1916" height="897" alt="image" src="https://github.com/user-attachments/assets/c2806d74-8e57-43bf-8919-6fcfe134ff56" />

---

# 20. Final Deployment Details

| Item | Details |
|---|---|
| Application | Trendify |
| Application Port | 3000 |
| AWS Region | eu-north-1 |
| DockerHub Image | anbansiddharthan/trend-app:latest |
| Kubernetes Service | trend-service |
| Kubernetes Platform | AWS EKS |
| Load Balancer | AWS Network Load Balancer |
| Monitoring | Prometheus + Grafana |
| CI/CD | Jenkins |
| Version Control | GitHub |

### GitHub Repository

`https://github.com/AnbanSiddharthan/trend-devops-project`

### DockerHub Image

`anbansiddharthan/trend-app:latest`

### Load Balancer DNS

`a1692eb15631e4b478af0da9abc66a1d-9ced16a22069ad18.elb.eu-north-1.amazonaws.com`

### Load Balancer ARN

```text
arn:aws:elasticloadbalancing:eu-north-1:858656722878:loadbalancer/net/a1692eb15631e4b478af0da9abc66a1d/9ced16a22069ad18
```

---

# 21. Conclusion

This project demonstrates an end-to-end DevOps workflow for deploying a production-ready application.

The implementation includes:

- GitHub source control
- Docker containerization
- DockerHub image management
- Terraform infrastructure provisioning
- Jenkins CI/CD
- GitHub webhook automation
- AWS EKS
- Kubernetes deployment
- AWS Network Load Balancer
- Prometheus monitoring
- Grafana dashboards

A GitHub push automatically triggers Jenkins. Jenkins builds the Docker image, pushes it to DockerHub, deploys the application to AWS EKS, verifies the Kubernetes rollout, and provides monitoring through Prometheus and Grafana.
