## Kubernetes cluster with Minikube

### History

2021-13-01: initial version 1.0.0

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

### Deploy yaml configuration

1-typo3cms-pod.yaml: this file is used for the webserver and the CMS. There is also the MySQL database server definition inside this manifest.
1-cms-service.yaml: service exposed outside to access the TYPO3 CMS.

```
kubectl apply -f 1-typo3cms-pod.yaml --namespace=typo3-playground
kubectl apply -f 1-cms-service.yaml --namespace=typo3-playground
```

### Get endpoint for exposed service

```
minikube --namespace=typo3-playground service cmsservice --url
```
![Get endpoint for exposed service](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/kubectl-service-endpoint.png)

### SSH access into the main pod

It is not advised to go inside your containers but if yoy need to (for instance, to create the ENABLE_INSTALL_TOOL in the /var/www/public/typo3conf directory - access with the famous joh316 password), here is how you can do:

```
kubectl exec --stdin --tty typo3cms -- /bin/bash
```

The working dir is /var/www.

Here are some screenshots:

Minikube Daskboard:

![Minikube Daskboard](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/minikube-dashboard.png)

TYPO3 Install Tool:

![TYPO3 Install Tool](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/typo3-install-tool.png)

TYPO3 Install Tool - Update Database schema:

![TYPO3 Install Tool - Update Database schema](https://raw.githubusercontent.com/christi4n/kubernetes-typo3/master/assets/typo3-install-tool-update-database-schema.png)


### What's next?

This is just a start. I'll make some effort to improve it from time to time.

### Source code qnd main Docker image

The Docker image used is entirely mine.
You can check my [GitHub repo](https://github.com/christi4n/docker-multistage).