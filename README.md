# kubernetes setup on ubuntu 24 step by step
## Prerequisites
1. Two or more machines running Ubuntu 24.04 (1 master, 1+ worker nodes)
2. Each vm (master or node):
   1. Minimum 2 CPU cores
   2. 2 GB+ RAM
   3. Root or sudo access
   4. Internet access
3. Unique hostname, MAC address, and product_uuid on each machine
4. Disable swap

## Step-by-Step Kubernetes Installation
| sl. no. | Description | Master (vm) | Node (vm) |
| --- | :--- | --- | --- | 
| 1 | Set Hostnames | <pre><code>sudo hostnamectl set-hostname master-node</code></pre> | <pre><code>sudo hostnamectl set-hostname worker-01</code></pre> |
| 2 | Update and Install Dependencies | <pre><code> sudo apt update && sudo apt upgrade -y &#10; sudo apt install -y apt-transport-https ca-certificates curl</code></pre> | same as master | 
| 3 | Disable Swap (Kubernetes requires this) | <span> `sudo swapoff -a` <br> <span> `sudo sed -i '/ swap / s/^/#/' /etc/fstab` | same as master | 
| 4 | Load Kernel Modules and Set Sysctl Params | see the below code | see the below code | 
<details>
<summary>Kernel Modules & Sysctl Code</summary>

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
```



- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

