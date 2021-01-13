## Kubernetes cluster with Minikube

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

### SSH access into the main pod

It is not advised to go inside your containers but if yoy need to (for instance, to create the ENABLE_INSTALL_TOOL in the /var/www/public/typo3conf directory - access with the famous joh316 password), here is how you can do:

```
kubectl exec --stdin --tty typo3cms -- /bin/bash
```

The working dir is /var/www.