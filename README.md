# Installing Minikube on Ubuntu
This guide will help you install Minikube on Ubuntu. Minikube is a tool that allows you to run Kubernetes on your local machine. This guide assumes you are running Ubuntu 20.04 or later.
## Prerequisites
Before we start, make sure your system meets the following requirements:

- Ubuntu 20.04 or later
- 4GB of RAM or more
- 20GB of free disk space or more
- Access to the internet

## Installation
To install Minikube, follow the steps below:

1. Open the terminal and switch to root user by typing
```bash
sudo su -
```
2. Install Docker by running the command
```bash
sudo apt update && apt -y install docker.io
```
3. Install kubectl by running the command
```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
```
4. Install Minikube by running the command
```bash
curl -Lo minikube https://github.com/kubernetes/minikube/releases/download/v1.24.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
5. Install Conntrack by running the command
```bash
apt install conntrack
```
6. Install Cri-Dockerd by running the following commands:
```bash
git clone https://github.com/Mirantis/cri-dockerd.git
wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile
cd cri-dockerd
mkdir bin
go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket
```
7. Install Cri-tools by running the following commands:
```bash
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.24.0/crictl-v1.24.0-linux-amd64.tar.gz
sudo tar zxvf crictl-v1.24.0-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-v1.24.0-linux-amd64.tar.gz
```
8. Finally, start Minikube by running the commands
```bash
minikube start --vm-driver=none 
```
and
```bash
minikube status
```
Congratulations! You have successfully installed Minikube on your Ubuntu machine. You can now start working with Kubernetes on your local environment.