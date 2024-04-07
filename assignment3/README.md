# Kubernetes Deployment Lab

This lab focuses on the scheduling, configmaps, canary and blue-green deployment strategies of Kubernetes. We will use Minikube in GitHub CodeSpaces to deploy an nginx service and two backend apps.
<br>
Use the following command to start minikube:
```bash
minikube start
```

## Environment Setup

This lab is performed in GitHub CodeSpaces, which provides a complete, configurable dev environment on top of a powerful VM in minutes. 

## Deployment

### Nginx Service

1. Deploy the nginx service using the `nginx-dep.yaml` file. This creates a Deployment named `nginx-dep` with 5 replicas using the nginx 1.14.2 base image and exposes port 80.
<br>
```bash
kubectl apply -f nginx-dep.yaml
```
```bash
minikube start
```
<br>
2. Create a ConfigMap using the `nginx-configmap.yaml` file. This ConfigMap contains the configuration for the nginx service.
<br>
```bash
kubectl apply -f nginx-configmap.yaml
```
<br>
3. Create a Service for the nginx Deployment using the `nginx-svc.yaml` file. This Service is of type ClusterIP and exposes port 80.
<br>
```bash
kubectl apply -f nginx-svc.yaml
```

### Backend Apps

1. Deploy the backend apps using the `app-1-dep.yaml` and `app-2-dep.yaml` files. These Deployments use the pre-built Docker images `ghcr.io/denoslab/ensf400-sample-app:v1` and `ghcr.io/denoslab/ensf400-sample-app:v2` respectively.
<br>
```bash
kubectl apply -f app-1-dep.yaml
kubectl apply -f app-2-dep.yaml
```
<br>
3. Create Services for the backend apps using the `app-1-svc.yaml` and `app-2-svc.yaml` files.
<br>
```bash
kubectl apply -f app-1-svc.yaml
kubectl apply -f app-2-svc.yaml
```
<br>
### Ingress

1. Create an Ingress for the nginx service using the `nginx-ingress.yaml` file. This Ingress redirects requests to path `/` to the backend service `nginx-svc`.

2. Create Ingresses for the backend apps using the `app-1-ingress.yaml` and `app-2-ingress.yaml` files. These Ingresses redirect 70% of the traffic to `app-1` and 30% of the traffic to `app-2`.

## Testing

You can test the setup by sending requests to the Minikube IP and checking the responses. For example:

```bash
$ curl http://$(minikube ip)/
Hello World from [app-1-dep-86f67f4f87-2d28z]!
$ curl http://$(minikube ip)/
Hello World from [app-2-dep-7f686c4d8d-lr95c]!
