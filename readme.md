# Kubernetes lab setup guide

## 1. VirtualBox and environment

### a. Virtualization

- VirtualBox: https://www.virtualbox.org/wiki/Downloads
- CentOS 7 image: http://isoredirect.centos.org/centos/7/isos/x86_64/
(you only need minimal image as this one have the least bloated app)

### b. Environment

- Setup 3 Centos 7 via VirtualBox. You can do it by setup 1 and clone 2 others
- Configuration for 3 VMs:
	+ k8s-master01.server: 4GB RAM, 2vCPU, 8GB disk
	+ k8s-worker01.server: 4GB RAM, 2vCPU, 8GB disk
	+ k8s-worker02.server: 4GB RAM, 2vCPU, 8GB disk
- Setup a NAT Network via VirtualBox (as this one allow all VMs to join single network domain and also allow outbound traffic to Internet)
- For the 1st VMs aka k8s-master01, enable network adapter. Join above NAT Network. The NAT Network will assign IPv4 address for VM by default via DHCP (10.0.2.0/24 subnet)
- One required step is that Centos minimal image do not have name-server configured by default. You must manually assign one via /etc/resolv.conf
- Install net-utils & bind-utils package. Test network connection by ping x.x.x.x and ping domain
- Once successful, take snapshot of this VMs. Using it as source for 2 new VMs via cloning. However, remember to make VirtualBox generate new MAC address for clone 
- One VMs clonned, you can consider make some port-forwardings for conveniently using terminal from host machine.
- Remember to take new snapshot when complete. Call it Infrastructure completed

## 2. Kubernetes, Kuberadm

- Copy kubernetes.repo file to all 3 VMs at /etc/yum.repos.d/kubernetes.repo
- Run these command to install required package:
		sudo yum clean all && sudo yum -y makecache
		sudo yum -y install epel-release vim git curl wget kubelet kubeadm kubectl --disableexcludes=kubernetes
- Recheck the installation by checking kuberadm version:
		kubectl version --client
		kubeadm  version
