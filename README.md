## Kubernetes cluster with Minikube

### History

2021-11-17 - 1.1.0: use chrisnt5/php-webdevops:2.x.x instead of 1.x.x
2021-10-02 - 1.0.2: add kustomize for Argo CD deployment
2021-13-01 - 1.0.1: improve documentation
2021-13-01 - 1.0.0: first repo version 

### What can you expect?

You can have a fully functionnal TYPO3 CMS running!

![Backend running](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/typo3-running-kubernetes.png)

### How-to
This repository contains two manifest files in order to create a running TYPO3 CMS instance under Kubernetes.
You can use [Minikube](https://minikube.sigs.k8s.io/) locally.

You are advised to use Virtualbox as a virtual environment for Minikube.
To launch Minikube the first time:

```
minikube start --memory=4096 --driver=virtualbox
```

```
eval $(minikube docker-env)
```

### Deploy with Argo CD

This step is not a documentation to explain how to install Argo CD.

First step: access the Argo CD backend. If you use Minikube, you can get the port the Argo CD server is using with:

```
kubectl get svc argocd-server -n argocd -o=jsonpath="{$.spec.ports[0].nodePort}{'\n'}"
```

You can edit the imge you need in the kustomization.yaml file and then, generate the manifests.yaml file again with the command below. If you cannot see any change, delete the file, it will be generated again.

```
kustomize build ./manifests > ./kustomization/manifests.yaml
```

### Argo CD

Deploy the first release of your application:

```
argocd app create kubernetes-typo3 --repo https://github.com/christi4n/kubernetes-typo3.git --revision kustomize --path ./kustomization --dest-server https://kubernetes.default.svc --dest-namespace kubernetes-typo3-app
```

Sync the app:

```
argocd app sync kubernetes-typo3
```

TODO: to be completed

The working dir is /var/www.

Here are some screenshots:

Minikube Daskboard:

![Minikube Daskboard](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/minikube-dashboard.png)

TYPO3 Install Tool:

![TYPO3 Install Tool](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/typo3-install-tool.png)

TYPO3 Install Tool - Update Database schema:

![TYPO3 Install Tool - Update Database schema](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/typo3-install-tool-update-database-schema.png)

### Kustomization
    $ kubectl create ns kubernetes-typo3-app
    $ kustomize build ./manifests > ./kustomization/manifests.yaml
    $ argocd app create kubernetes-typo3 --repo https://github.com/christi4n/kubernetes-typo3.git  \ 
    --revision kustomize --path ./kustomization --dest-server https://kubernetes.default.svc  \ 
    --dest-namespace kubernetes-typo3-app
    $ argocd app sync kubernetes-typo3

Last step is to proceed to http://${CLUSTER_URL}:${PORT}/typo3/install.php

### What's next?

This is just a start. I'll make some effort to improve it from time to time.

### Source code and main Docker image

The Docker image used is entirely mine.
You can check my [GitHub repo](https://github.com/christi4n/docker-multistage).