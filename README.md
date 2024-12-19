# 🚀 Kubernetes End-to-End Project on Amazon EKS (Elastic Kubernetes Service)

![main-images](https://github.com/user-attachments/assets/37441828-8272-4f37-839c-54600fa37ef3)

## **Prerequisites ✅**
To complete this project, ensure you have the following tools installed and configured:

- **kubectl 🕹️**: Command-line tool for interacting with Kubernetes clusters. [Install or update kubectl](https://kubernetes.io/docs/tasks/tools/).
- **eksctl 🏫**: Command-line utility for managing Amazon EKS clusters. [Install eksctl](https://eksctl.io/introduction/).
- **AWS CLI 🌐**: Command-line interface for working with AWS services. [Install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) and configure with `aws configure`.

---

## **✨ Project Overview**

### **🎮 Project Title**: Deploying the 2048 Game Application on Amazon EKS

### **Objective 🔧**
This project focuses on deploying the popular 2048 game app on a Kubernetes cluster hosted on Amazon EKS. It demonstrates how to:
1. 🔒 Containerize a web application using Docker.
2. 🌎 Configure and deploy an EKS cluster.
3. ✉️ Manage the application with Kubernetes YAML manifests.
4. 💡 Enable scaling for performance.
5. ☁️ Expose the application to users via a Load Balancer.

---

## **Step-by-Step Implementation ➕**

### **Step 1: Set Up the Amazon EKS Cluster**

1. **Create IAM Roles 🔒**:
   - **EKS Cluster Role**:
     - Attach `AmazonEKSClusterPolicy`.
   - **EKS Node Group Role**:
     - Attach the following policies:
       - `AmazonEKSWorkerNodePolicy`
       - `AmazonEC2ContainerRegistryReadOnly`
       - `AmazonEKS_CNI_Policy`

2. **Cluster Configuration 🗒**:
   - Use `eksctl` or AWS Management Console to set up:
     - 🟩 Default VPC with 2-3 subnets.
     - ⚡️ Security group allowing ports **22**, **80**, and **8080**.
     - Endpoint access set to **Public**.

3. **⏳ Cluster Activation**:
   Wait for 10-12 minutes and verify the status.

![EKS Cluster](https://github.com/user-attachments/assets/bfdce8ca-6572-4537-a116-6b9acf12ffa7)
![image-1](https://github.com/user-attachments/assets/1e24989c-316a-4e55-b869-6677c6726999)

---

### **Step 2: Add Node Groups ➕**

1. **Worker Node Setup 💻**:
   - Name: `<your-name>-eks-nodegrp-1`
   - AMI: 🗂 Amazon Linux 2.
   - Desired, Minimum, Maximum Instances: Set to **1** each.
   - Enable SSH Access: Use the security group configured for ports **22**, **80**, **8080**.

3. **Node Creation ⏳**:
   Wait for 2-3 minutes for completion.

![Node Group](https://github.com/user-attachments/assets/619cfa1f-3afd-4f91-a094-b2abf4a8c89c)


---

### **Step 3: Authenticate the Cluster 🔓**

1. **IAM Verification(1 policy attached) ✨**:
![image-2](https://github.com/user-attachments/assets/406bbd52-d37a-401b-9d74-bb9d456561e1)
   ```bash
   aws sts get-caller-identity
   

   ```

3. **kubeconfig Setup ⚖️**:
   ```bash
   aws eks update-kubeconfig --region <region-code> --name <cluster-name>
   ```
   Example:
   ```bash
   aws eks update-kubeconfig --region us-east-1 --name my-eks-cluster
   ```

4. **Cluster Validation 🔍**:
   ```bash
   kubectl get nodes
   ```

---

### **Step 4: Deploy the 2048 Game Application 🎮**

1. **Pod Manifest 🗎**:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: 2048-pod
     labels:
       app: 2048-ws
   spec:
     containers:
     - name: 2048-container
       image: blackicebird/2048
       ports:
       - containerPort: 80
   ```

2. **Pod Deployment 🚪**:
   ```bash
   kubectl apply -f 2048-pod.yaml
   ```

3. **Verify Deployment 🆑**:
   ```bash
   kubectl get pods
   ```

---

### **Step 5: Expose the Application ☁️**

1. **LoadBalancer Service Manifest**:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: mygame-svc
   spec:
     selector:
       app: 2048-ws
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: LoadBalancer
   ```

2. **Apply Service Configuration ✨**:
   ```bash
   kubectl apply -f mygame-svc.yaml
   ```

3. **Service Validation 🔒**:
   ```bash
   kubectl describe svc mygame-svc
   ```

4. **Game Access 🎮**:
   Retrieve the LoadBalancer DNS from EC2 🚪 and access via browser.

![2048 Game](https://github.com/user-attachments/assets/4f060d20-6b74-4292-9113-7cfa3d12dae6)

---

## **✨ Conclusion**

🚀 Congratulations! You’ve successfully deployed the 2048 🎮 on EKS. In this project, you’ve learned to:
- 🔧 Containerize an application.
- ☁️ Deploy and scale Kubernetes clusters.
- 🛠 Expose apps to the internet via LoadBalancer Services.

---

## **✏️ Feedback**
Thank you! Don’t forget to ⭐ and share this repository.

#### **Author**: [✨ Arunesh Dwivedi](https://github.com/AruneshDwivedi)

---

