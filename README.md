# Kubernetes_Practice
This is a Practice repo for Kubernetes 

👉 We will start with the installation of Kubelet and eksctl to create cluster and pods.

1. 🧩 Install kubelet (Kubernetes) on Amazon Linux 2023

Amazon Linux 2023 doesn’t support old yum repos the same way, so we use the official Kubernetes repo.

1. Add Kubernetes repo

cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/repodata/repomd.xml.key
EOF

2. Install kubelet, kubeadm, kubectl
sudo dnf install -y kubelet kubeadm kubectl

3. Enable & start kubelet
sudo systemctl enable kubelet
sudo systemctl start kubelet

4. Verify
kubelet --version
kubectl version --client

💡 Note: kubelet alone won’t run pods until you configure a cluster (kubeadm / EKS / etc.)

2. 🚀 Install eksctl (for EKS cluster management)

This one is your magic wand for AWS Kubernetes ☁️

1. Download latest eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" -o eksctl.tar.gz

2. Extract
tar -xzf eksctl.tar.gz

3. Move to bin
sudo mv eksctl /usr/local/bin

4. Verify
eksctl version

🚀 Create EKS Cluster + Node Group (single command way)

This is the easiest and most used approach:

eksctl create cluster \
--name my-cluster \
--region ap-south-1 \
--nodegroup-name my-nodes \
--node-type t2.medium \
--nodes 2 \
--nodes-min 1 \
--nodes-max 3


🔍 What each flag means (important for interviews)
--name → Cluster name
--region → AWS region (use closest = ap-south-1 for India 🇮🇳)
--nodegroup-name → Name of worker nodes
--node-type → EC2 type
--nodes → Desired nodes
--nodes-min → Minimum autoscaling
--nodes-max → Maximum autoscaling

⏳ This will take ~10–15 minutes
During this:

VPC is created
Subnets are created
EC2 instances launched
Kubernetes control plane set up
✅ After creation (verify everything)
1. Check cluster
eksctl get cluster
2. Update kubeconfig (very important)
aws eks --region ap-south-1 update-kubeconfig --name my-cluster
3. Check nodes
kubectl get nodes

👉 If you see nodes = 🎉 success

4. Check pods
kubectl get pods -A
🧪 Create Node Group separately (advanced way)


📦 Simple pod.yaml (Nginx example)
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
        
🚀 Apply this pod
kubectl apply -f pod.yaml

🔍 Check pod status
kubectl get pods

📡 Get more details
kubectl describe pod nginx-pod

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
