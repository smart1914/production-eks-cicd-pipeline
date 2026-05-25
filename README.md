
# 🚀 End-to-End CI/CD Pipeline for Node.js App Deployment on EKS using GitHub Actions

A robust, enterprise-grade continuous integration and continuous deployment (CI/CD) automation architecture that builds, tests, containerizes, and deploys a Node.js web application straight down into an Amazon Elastic Kubernetes Service (EKS) cluster using GitHub Actions pipelines and Kustomize manifest management configurations.

---

## **📌 Table of Contents** - [📂 Repository Structure](#-repository-structure)
- [🔧 Prerequisites](#-prerequisites)
- [⚙️ CI/CD Workflow](#️-cicd-workflow)
  - [🔨 Build Job](#-build-job)
  - [🚀 Deployment Job](#-deployment-job)
- [🛠️ Real-World Engineering Troubleshooting](#️-real-world-engineering-troubleshooting)
- [📊 Monitoring \& Verification Health Logs](#-monitoring--verification-health-logs)
- [🖼️ Live Application Proof](#️-live-application-proof)
- [📜 Infrastructure Clean-up](#-infrastructure-clean-up)

---

## **📂 Repository Structure** The repository is structured for modularity and maintainability:

```tree
📂 root  
├── 📂 .github/workflows/      # GitHub Actions CI/CD workflows
│   ├── ci.yml                 # CI pipeline (testing & linting validations)
│   └── cd-production.yml      # Live production deployment automated runner
│
├── 📂 app                     # Application source code  
│   ├── calculator.js          # Business logic for calculations  
│   ├── calculator.test.js     # Unit tests for calculator functions  
│   ├── Dockerfile             # Optimized Dockerfile configuration
│   ├── index.js               # Main entry point of the Node.js application  
│   └── package.json           # Project dependencies and test scripts  
│  
├── 📂 kustomize               # Kubernetes manifests managed with Kustomize  
│   └── 📂 base                # Base configurations common for all environments  
│       ├── deployment.yaml    # Pod layout deployment with health checks
│       ├── kustomization.yaml # Kustomize configuration file
│       └── service.yaml       # Kubernetes LoadBalancer service mapping tracking
└── README.md                  # Project documentation and deployment details

```

---

## **🔧 Prerequisites** Before executing the pipelines, ensure your local and remote environments contain:

* 🛠 **Node.js (>=20.x)** - 🐳 **Docker Container Runtimes** - ☸ **kubectl CLI engine link tool** - ☁ **AWS Access Credentials** configured securely inside GitHub Secrets (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`)
* 🔑 **AWS IAM privileges** to administer active EKS Clusters

---

## **⚙️ CI/CD Workflow Architecture** The automation delivery pipeline is split into logical jobs handling compilation through delivery within `.github/workflows/cd-production.yml`:

### **🔨 Build Job** 1️⃣ **Code Quality Verification** - Instantiates code checkout actions and attaches local `Node.js 20` instances.

* Resolves local dependencies via clean `npm install` tasks.
* Runs comprehensive code unit checks using automated execution matrices (`npm test`).

2️⃣ **Dynamic ECR Containerization** - Uses secure programmatic keys to run authenticated handshakes into **Amazon Elastic Container Registry (ECR)**.

* Compiles application source paths into secure Docker image layers.
* Tags builds with both unique structural `git-sha` markers alongside mutable `:latest` tags before pushing directly to the cloud registry.

### **🚀 Deployment Job** 1️⃣ **Kubernetes Infrastructure Authentication** - Updates `kubeconfig` references dynamically targeting the active remote **Amazon EKS cluster** (`e2ecicd-eks-dev`).

2️⃣ **Streaming Configuration Manipulation** - Automatically updates mismatched manifest strings inside the pipeline runner using custom stream text modifiers (`sed`).

* Transforms internal mapping indicators from generic `ClusterIP` layouts into live elastic `LoadBalancer` modes.

3️⃣ **State Application Orchestration** - Triggers `kubectl apply -k` flags against Kustomize files to initiate a live zero-downtime rolling rollout within the cluster nodes.

---

## **🛠️ Real-World Engineering Troubleshooting**

In cloud-native environments, static delivery files often suffer from integration drift. During this project's initial deployment lifecycle, an alignment error surfaced: the deployment manifests referenced a generic container target (`nodejs-app:latest`), causing clusters to log critical `ImagePullBackOff` faults.

Instead of introducing hardcoded configuration rewrites inside the base repository code, the delivery pipeline was optimized to execute **dynamic inline stream mutations**:

```yaml
- name: Hard-Replace Image String & Service Type
  run: |
    cd kustomize/base/
    # 1. Hard replace image tracking to target our actual calculator-app ECR path
    sed -i 's|[623035187793.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:latest](https://623035187793.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:latest)|[623035187793.dkr.ecr.us-east-1.amazonaws.com/calculator-app:latest](https://623035187793.dkr.ecr.us-east-1.amazonaws.com/calculator-app:latest)|g' *.yaml
    
    # 2. Upgrade service layout dynamically from private ClusterIP to public LoadBalancer
    sed -i 's/type: ClusterIP/type: LoadBalancer/g' *.yaml

```

This strategy ensures a decoupled, immutable codebase while keeping production deployment paths entirely automated.

---

## **📊 Monitoring & Verification Health Logs** Upon automated runner completion, the cluster resources verify a healthy operational state:

```bash
# Tracking application controller pod runtime
[root@eks-node]# kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
web-deployment-6bc49fc6-gmlrr     1/1     Running   0          33s

# Tracking live network ingress exposure endpoints
[root@eks-node]# kubectl get svc
NAME          TYPE           CLUSTER-IP       EXTERNAL-IP                                                             PORT(S)
web-service   LoadBalancer   172.20.232.197   a0fadebfacc33475db1c691c1c3849cd-1841414744.us-east-1.elb.amazonaws.com 80:32561/TCP

```

---

## **🖼️ Live Application Proof** The browser captures the successfully routed node-application rendering live via an AWS Classic Elastic Load Balancer endpoint:

---

## **📜 Infrastructure Clean-up** To cleanly dismantle active elastic hardware configurations and prevent ongoing cloud resource accumulation, trigger the following clean-up command locally:

```bash
kubectl delete -k kustomize/base/

```

---

## 🛠️ **Author & Community** This project was built and optimized as a hands-on exploration of production GitOps delivery practices by **[Your GitHub Name](https://github.com/smart1914)** 💡.

Feel free to reach out, review my configuration parameters, or drop a line to discuss all things Kubernetes and DevOps!

```

---
