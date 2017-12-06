# kubernetes-install-bare-metal

Kubernetes is docker orchestration tool developed by google.
For Installing kubernetes we required 4 packages to be installed on machine.

Install Docker for Ubuntu:
$ apt-get update

$ apt-get install -y docker.io

Install Docker for CentOS/RHEL:

$ yum install -y docker

$ systemctl enable docker && systemctl start docker

Install kubeadm, kubelet and kubectl.

kubeadm: The command to bootstrap the cluster.

kubelet: The component that runs on all of the machines in your cluster and does things like starting pods and containers.

kubectl: The command line util to talk to your cluster.
Install kubelet kubeadm kubectl kubernetes-cni for Ubuntu:

$ apt-get update && apt-get install -y apt-transport-https

$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

$ cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

$ apt-get update
$ apt-get install -y kubelet kubeadm kubectl kubernetes-cni

Install kubelet kubeadm kubectl kubernetes-cni for CentOS/RHEL:

$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

$ setenforce 0

$ yum install -y kubelet kubeadm kubectl

$ systemctl enable kubelet && systemctl start kubelet

Initializing cluster with kubeadm
On master node

$ kubeadm init — pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=172.20.0.10

--apiserver-advertise-address determines which IP address Kubernetes should advertise its API server on.
You will get this when you done with Kubadm init.
Your Kubernetes master has initialized successfully!
To start using your cluster, you need to run (as a regular user):

$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run “kubectl apply -f [podnetwork].yaml” with one of the options listed at:
http://kubernetes.io/docs/admin/addons/
You can now join any number of machines by running the following on each node
as root:

$ kubeadm join — token 7ed3b7.ae447f51edc87df2 172.20.0.10:6443 — discovery-token-ca-cert-hash sha256:7b19036bd9cfc7e4e09721448e0a0467cf7fc67c982abbf36f478e0c2eec32cb

Via this token you can connect new nodes to master.
For installing nodes you need to do all the steps as given above but only $kubeadm init not required.

$ kubectl apply --filename https://git.io/weave-kube-1.6

For running kube-dns POD’s
Now you can check nodes for kubernetes cluster

$ kubectl get nodes
For checking which pods are running on kubernetes cluster

$ kubectl get pods --all-namespaces -o wide

Kubernets cluster running..!!!!
