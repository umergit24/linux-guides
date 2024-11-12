---
tags:
  - guides
---
https://www.youtube.com/watch?v=gd7BXuUQ91w

# install docker


```bash
sudo apt update
sudo apt upgrade -y
sudo apt-get update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce -y
sudo systemctl status docker
sudo systemctl enable docker
sudo usermod -aG docker vboxuser

docker run hello-world

```

# install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client
```

## adding user to sudoers file

```linux
$ su root

root@ubuntunew:/home/vboxuser# apt-get install sudo -y

root@ubuntunew:/home/vboxuser# adduser vboxuser sudo

root@ubuntunew:/home/vboxuser# chmod 0440 /etc/sudoers

root@ubuntunew:/home/vboxuser# exit

```
then restart
## set up GO lang in ubuntu

```
sudo tar -C /usr/local -xzf go1.22.5.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
```

In `~/.profile`, add the following:
```
sudo nano ~/.profile
# set PATH so it includes GOPATH/bin if it exists  
if [ -x "$(command -v go)" ] && [ -d "$(go env GOPATH)/bin" ]; then  
    PATH="$(go env GOPATH)/bin:$PATH"  
fi


go version
```

## install OperatorSDK

```
export ARCH=$(case $(uname -m) in x86_64) echo -n amd64 ;; aarch64) echo -n arm64 ;; *) echo -n $(uname -m) ;; esac)

export OS=$(uname | awk '{print tolower($0)}')

export OPERATOR_SDK_DL_URL=https://github.com/operator-framework/operator-sdk/releases/download/v1.36.0

curl -LO ${OPERATOR_SDK_DL_URL}/operator-sdk_${OS}_${ARCH}

chmod +x operator-sdk_${OS}_${ARCH} && sudo mv operator-sdk_${OS}_${ARCH} /usr/local/bin/operator-sdk

operator-sdk version

```

for verification: 
```

gpg --keyserver keyserver.ubuntu.com --recv-keys 052996E2A20B5C7E

curl -LO ${OPERATOR_SDK_DL_URL}/checksums.txt
curl -LO ${OPERATOR_SDK_DL_URL}/checksums.txt.asc
gpg -u "Operator SDK (release) <cncf-operator-sdk@cncf.io>" --verify checksums.txt.asc

grep operator-sdk_${OS}_${ARCH} checksums.txt | sha256sum -c -


```


uninstall operator-sdk
```
which operator-sdk
sudo rm /usr/local/bin/operator-sdk
```


# delete everything kubernetes related

```yaml
#!/bin/sh
# Kube Admin Reset
kubeadm reset

# Remove all packages related to Kubernetes
apt remove -y kubeadm kubectl kubelet kubernetes-cni 
apt purge -y kube*

# Remove docker containers/ images ( optional if using docker)
docker image prune -a
systemctl restart docker
apt purge -y docker-engine docker docker.io docker-ce docker-ce-cli containerd containerd.io runc --allow-change-held-packages

# Remove parts

apt autoremove -y

# Remove all folder associated to kubernetes, etcd, and docker
rm -rf ~/.kube
rm -rf /etc/cni /etc/kubernetes /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/lib/etcd2/ /var/run/kubernetes ~/.kube/* 
rm -rf /var/lib/docker /etc/docker /var/run/docker.sock
rm -f /etc/apparmor.d/docker /etc/systemd/system/etcd* 

# Delete docker group (optional)
groupdel docker

# Clear the iptables
iptables -F && iptables -X
iptables -t nat -F && iptables -t nat -X
iptables -t raw -F && iptables -t raw -X
iptables -t mangle -F && iptables -t mangle -X
```




# vbox guest addition
```
sudo apt install build-essential dkms linux-headers-generic 
```

```
sudo rcvboxadd setup
```

# ssh into vm

```
sudo apt install openssh-server -y
sudo systemctl enable ssh.service
sudo systemctl start ssh.service
sudo systemctl status ssh.service
```

		get ip from  'ip a'
		username from 'whoami'
make sure firewall is disabled

		sudo ufw diable
# install kind
pre-req: go, docker

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /bin/kind
vi kindconfig.yaml
```

create multi node cluster using kindconfig file
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

```
kind create cluster --config kindconfig.yaml
```

# install metallb in kind
# install helm
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
# install vs code

 ```shell
sudo apt update
sudo snap install --classic code
code
 ```

# install kubebuilder
```shell
curl -L -o kubebuilder "https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)"
chmod +x kubebuilder 
sudo mv kubebuilder /usr/local/bin/


```

# uninstall go
```
sudo apt-get remove golang-go
sudo apt-get remove --auto-remove golang-go
```

# deploying kubernetes dashboard


# autocompletion for ubuntu 
```
sudo apt update
sudo apt info bash-completion
sudo apt install bash-completion
```


# installing node

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
source ~/.bashrc
nvm install node
```


# installing docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```
