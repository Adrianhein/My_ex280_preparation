
## Helm is a package manager of Kubernetes, which helps in managing Kubernetes applications. 
### To fix this error, we'll need to install Helm.
### On Linux
### Download the Helm binary:
    curl https://get.helm.sh/helm-v3.10.1-linux-amd64.tar.gz -o helm.tar.gz
    tar -zxvf helm.tar.gz
    cp -p ~/linux-amd64/helm ~/local/bin/
    helm version

### On Windows (chocolaty need to be installed first)
    choco install kubernetes-helm
    helm version

---

### New project for helm
    oc new-project helm-project

### Add helm repo
    helm repo add bitnami <image registry url>
    helm repo add bitnami https://charts.bitnami.com/bitnami

### Install application
    helm install wordpress-app bitnami/wordpress
    #>> help install wordpress-app bitnami/wordpress --set wordpressUsernam=admin --set wordpressPassword=wpadmin 

### Verifying after installing 
    helm list

### Repo update
    help repo update

### Uninstall application
    helm install wordpress-app 
---

