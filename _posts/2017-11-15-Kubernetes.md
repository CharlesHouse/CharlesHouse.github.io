---
layout:     post
title:      "Kubernetes Configuration"
subtitle:   "Kubernetes"
date:       2017-11-13
author:     "Charles"
tags:
    - Kubernetes
    - Container
    - Monitoring
---

[Kubernetes](https://kubernetes.io)是Google主导的自动化容器管理的开源平台，用于容器的部署、自动化调度和集群管理。本文将致力于kubernetes的配置和相关插件安装。另外为方便我之后的整理编辑，本文主体部分将用英文书写，反正也并没有几个人会真的研究这个 ：）

Kubernetes official website provides instruction for installing kubernetes and setting up a kubernetes cluster based on varies cloud platforms and different OS.
Since kubernetes 1.4, ‘kubeadm’ is introduced to reduce the workload to install kubernetes cluster. Here we will bootstrap Clusters with kubeadm instead of installing each components manually.  
Now let's start!


###Prerequirement
* One or more machines running Ubuntu 16.04+, Debian 9, CentOS 7 (Here I use Ubuntu 16.04)
>       swapoff -a
* Disable Swap in order for kubelet to work propery
* Disable Firewalld Service
>     - systemctl status firewalld.service
>     - systemctl stop firewalld.service
>     - systemctl disable firewalld.service
* Remote server preparation: Here we deploy all the nodes on POLIMICLOUD virtual machine which provide the following nodes.

|   IP Address   |  Role  | CPU | Memory |
|----------------|--------|-----|--------|
| 131.175.21.210 | master |   2 | 2G     |
| 192.168.1.34   | node1  |   1 | 2G     |
| 192.168.1.31   | node2  |   1 | 2g     |

*note: Due to the port limitation of PoliCloud, the IP Address of the node can only be accessed from the master node. That means, 192.168.1.34 is the inner IP.*

### Installing Docker
Install Docker directly from Ubuntu repositories:
>   apt-get update
>   apt-get install -y docker.io  

Install Docker CE from Docker official repositories. Here we recommend to install Docker Version v1.12, v1.13 or v17.03, for the rest version, there are not yet tested by Kubernetes node team.
>  apt-get update && apt-get install -y  
>  curl apt-transport-https  
>  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
  cat <<EOF >/etc/apt/sources.list.d/docker.list  
>  deb https://download.docker.com/linux/$(lsb_release -si | tr '[:upper:]' '[:lower:]') $(lsb_release -cs) stable  
>  EOF  
>  apt-get update  
>  apt-get install -y kubelet kubeadm kubectl

### Installing kubeadm, kubelet and kubectl
All the packages should be installed on each machine.  
>   apt-get update && apt-get install -y apt-transport-https  
>   curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
>   cat <<EOF >/etc/apt/sources.list.d/kubernetes.list  
>   deb http://apt.kubernetes.io/ kubernetes-xenial main  
>   EOF  
>   apt-get update  
>   apt-get install -y kubelet kubeadm kubectl  

 
### Bootstrap Cluster
Now we can start to initialize the cluster with kubeadm. We choose 131.175.21.210 as the Master Node and the following commands are executed on this machie. In order to connect Master node on POLIMICLOUD, we use ssh channel to connect.
>   ssh -i id_rsa.key ubuntu@131.175.21.210  
>   sudo su  
>   apt-get update && apt-get upgrade  
>   kubeadm init     
 
The output should look like:
> [kubeadm] WARNING: kubeadm is in beta, please do not use it for production clusters.  
> [init] Using Kubernetes version: v1.8.0  
> [init] Using Authorization modes: [Node RBAC]  
> [preflight] Running pre-flight checks  
> [kubeadm] WARNING: starting in 1.8, tokens expire after 24 hours by default (if you require a non-expiring token use --token-ttl 0)  
> [certificates] Generated ca certificate and key.  
> [certificates] Generated apiserver certificate and key.  
> [certificates] apiserver serving cert is signed for DNS names [kubeadm-master kubernetes kubernetes.default kubernetes.default.svc   kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.138.0.4]  
> [certificates] Generated apiserver-kubelet-client certificate and key.  
> [certificates] Generated sa key and public key.  
> [certificates] Generated front-proxy-ca certificate and key.  
> [certificates] Generated front-proxy-client certificate and key.  
> [certificates] Valid certificates and keys now exist in "/etc/kubernetes/pki"  
> [kubeconfig] Wrote KubeConfig file to disk: "admin.conf"  
> [kubeconfig] Wrote KubeConfig file to disk: "kubelet.conf"  
> [kubeconfig] Wrote KubeConfig file to disk: "controller-manager.conf"  
> [kubeconfig] Wrote KubeConfig file to disk: "scheduler.conf"  
> [controlplane] Wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"  
> [controlplane] Wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"  
> [controlplane] Wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"  
> [etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"  
> [init] Waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests"  
> [init] This often takes around a minute; or longer if the control plane images have to be pulled.  
> [apiclient] All control plane components are healthy after 39.511972 seconds
> [uploadconfig] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace  
> [markmaster] Will mark node master as master by adding a label and a taint
> [markmaster] Master master tainted and labelled with key/value: node-role.kubernetes.io/master=""  
> [bootstraptoken] Using token: <token>  
> [bootstraptoken] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials  
> [bootstraptoken] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token  
> [bootstraptoken] Creating the "cluster-info" ConfigMap in the "kube-public" namespace  
> [addons] Applied essential addon: kube-dns  
> [addons] Applied essential addon: kube-proxy  
> 
> Your Kubernetes master has initialized successfully!  

To make kubectl work, we should set Kubernetes Environment for the machine:
>   cp /etc/kubernetes/admin.conf $HOME/  
>   chown $(id -u):$(id -g) $HOME/admin.conf  
>   export KUBECONFIG=$HOME/admin.conf  

After rebooting or relogining the remote server, we might have to reset the environment for Kubernetes with the same commands.
Till now, Kubernetes Master has successfully installed and we still need to install some nodes to run workloads (pods and containers, etc). As there is only one external IP for me to use on POLIMICLOUD, I have to SSH to the master node and then reload to the other nodes through internal IP.

>   ssh -i id_rsa.key ubuntu@131.175.21.210  
>   ssh -i /tmp/id_rsa.key ubuntu@192.168.1.34  
>   sudo su  
>   kubeadm join --token <token> <master-ip>:<master-port>   --discovery-token-ca-cert-hash sha256:<hash>  


Run *kubectl get nodes* on master, we can only see the master node is ready and all the nodes are not ready. Because there is no pod network to make all pods communicate with each other. Now I will install a pod network add-on to make all nodes ready. What should be attention is that the network must be deployed before any applications.  
Here I choose **Weave Net**
> export kubever=$(kubectl version | base64 | tr -d '\n')  
> kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"  
 
### Kubernetes Dashboard add-on
Kubernetes Dashboard is a general purpose, web-based UI for Kubernetes clusters. It allows users to manage applications running in the cluster and troubleshoot them, as well as manage the cluster itself.

Since kubernetes v1.7, Dashboard uses more secure setup. It means, that by default it has minimal set of privileges and can only be accessed over HTTPS. When accessing Dashboard, there is a login page to confirm privileges.
>   mkdir -p ~/k8s/  
>   wget https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml  
>   kubectl create -f kubernetes-dashboard.yaml  
>   kubectl proxy  

Here I choose to use **Bearer Token** as an authentication method
> $ kubectl -n kube-system get secret  
> kubernetes-dashboard-token-hxmwl  
> $ kubectl -n kube-system describe secret kubernetes-dashboard-token-hxmwl  

The name of the dashboard (kubernetes-dashboard-token-hxmwl) changes on different machines and you should replace it by yourself. Use the token derived by above commands when we meet the login page of Dashboard. At the same time, I choose Putty to do port forwarding (in order to make a mapping from port 7575 of my local machine to port 8081 of the remote server).   

The dashboard address: *http://localhost:7575/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login*

### Heapster add-on
Heapster enables Container Cluster Monitoring and Performance Analysis for Kubernetes, and platforms which include it. It is used for collecting and interpreting various signals like compute resource usage, lifecycle events, etc, and exports cluster metrics via REST endpoints.

Here is the instructions about how to add it into Kubernetes Cluster:  
> mkdir -p ~/k8s/heapster   
> cd ~/k8s/heapster  
> wget https://https://github.com/kubernetes/heapster/tree/master/deploy/kube-config/influxdb/grafana.yaml  
> wget https://https://github.com/kubernetes/heapster/tree/master/deploy/kube-config/rbac/heapster-rbac.yaml  
> wget https://https://github.com/kubernetes/heapster/tree/master/deploy/kube-config/influxdb/heapster.yaml  
> wget https://https://github.com/kubernetes/heapster/tree/master/deploy/kube-config/influxdb/influxdb.yaml  
> kubectl create -f ./  

We can see the metrics through REST API at
*Http://localhost:7575/api/v1/proxy/namespaces/kube-system/services/heapster/api/v1/model/debug/allkeys*















>    
>  
>  
> 


