# Kubernetes

![](Kubernetes_logo.svg)

Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon 15 years of experience of running production workloads at Google, combined with best-of-breed ideas and practices from the community.

https://kubernetes.io/

![](k8s.png)

# Container Orchestrators

## Amazon Elastic Container Service
Amazon Elastic Container Service (ECS) is a hosted service provided by Amazon Web Services (AWS) to run containers at scale on its infrastructure.

https://aws.amazon.com/ecs/


## Azure Container Instances
Azure Container Instance (ACI) is a basic container orchestration service provided by Microsoft Azure.

https://azure.microsoft.com/en-us/products/container-instances/#overview

## Azure Service Fabric
Azure Service Fabric is an open source container orchestrator provided by Microsoft Azure.

https://azure.microsoft.com/en-us/products/service-fabric/

## Kubernetes
Kubernetes is an open source orchestration tool, originally started by Google, today part of the Cloud Native Computing Foundation (CNCF) project.

https://kubernetes.io/

## Marathon
Marathon is a framework to run containers at scale on Apache Mesos and DC/OS.

https://mesosphere.github.io/marathon/

## Nomad
Nomad is the container and workload orchestrator provided by HashiCorp.

https://www.nomadproject.io/

## Docker Swarm
Docker Swarm is a container orchestrator provided by Docker, Inc. It is part of Docker Engine.

https://docs.docker.com/engine/swarm/

# What is Kubernetes

![](kubernetes.png)

# Borg (Meet Google's BorgMasters and Borglets from 2015)

For more than a decade, Borg has been Google's secret, running its worldwide containerized workloads in production. Services we use from Google, such as Gmail, Drive, Maps, Docs, etc., are all serviced using Borg. 

![](borg.png)

https://storage.googleapis.com/gweb-research2023-media/pubtools/pdf/43438.pdf


# Certified Kubernetes Application Developer (CKAD)

![](ckad.png)

https://github.com/cncf/curriculum/blob/master/CKAD_Curriculum_v1.28.pdf

# Kubernetes Architecture

![](cluster.png)

# kube-apiserver

All the administrative tasks are coordinated by the kube-apiserver, a central control plane component running on the control plane node. The API Server intercepts RESTful calls from users, administrators, developers, operators and external agents, then validates and processes them. During processing the API Server reads the Kubernetes cluster's current state from the key-value store, and after a call's execution, the resulting state of the Kubernetes cluster is saved in the key-value store for persistence. The API Server is the only control plane component to talk to the key-value store, both to read from and to save Kubernetes cluster state information - acting as a middle interface for any other control plane agent inquiring about the cluster's state.

The API Server is highly configurable and customizable. It can scale horizontally, but it also supports the addition of custom secondary API Servers, a configuration that transforms the primary API Server into a proxy to all secondary, custom API Servers, routing all incoming RESTful calls to them based on custom defined rules.

# etcd

![](etcd.png)

# A Pod of Killer Whales (APKW)

A Pod is the smallest scheduling work unit in Kubernetes. It is a logical collection of one or more containers scheduled together, and the collection can be started, stopped, or rescheduled as a single unit of work. 

![](pod.png)

# CRI-O

Although Kubernetes is described as a "container orchestration engine", it lacks the capability to directly handle and run containers. In order to manage a container's lifecycle, Kubernetes requires a container runtime on the node where a Pod and its containers are to be scheduled. Runtimes are required on all nodes of a Kubernetes cluster, both control plane and worker.

![](cri-o.png)

# Kubernetes Configuration

![](config.png)

# Minikube

![](Minikube_logo.png)

https://minikube.sigs.k8s.io/docs/

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
minikube start --driver=virtualbox
```

![](minikube.png)

![](minikube-vb7.png)

```
minikube status
minikube stop
minikube profile list
```

# Minikube Profiles

```
minikube start --kubernetes-version=v1.23.3 --driver=podman --profile minipod

minikube start --nodes=2 --kubernetes-version=v1.24.4 --driver=docker --profile doubledocker

minikube start --driver=virtualbox --nodes=3 --disk-size=10g --cpus=2 --memory=4g --kubernetes-version=v1.25.1 --cni=calico --container-runtime=cri-o -p multivbox

minikube start --driver=docker --cpus=6 --memory=8g --kubernetes-version="1.24.4" -p largedock

minikube start --driver=virtualbox -n 3 --container-runtime=containerd --cni=calico -p minibox
```

# Minbox

![](minibox.png)

```
minikube start --driver=virtualbox -n 3 --container-runtime=containerd --cni=calico -p minibox
minikube stop -p minibox
minikube delete -p minibox
```

![](minibox-info.png)


# 4docker

```
minikube start -n 4 -p 4docker
minikube remove -p 4docker

```

![](4docker.png)

# Kubectl

https://kubectl.docs.kubernetes.io/

![](kubectl.png)

![](pods.png)

# Kubernetes Dashboard

```
minikube addons list
minikube addons enable metrics-server
minikube addons enable dashboard
minikube dashboard
```

![](dashboard.png)

# Pods

nginx.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

```
kubectl apply -f nginx.yaml
```

![](nginx.png)

```
kubectl run firstrun --image=nginx
kubectl get pods -o wide

kubectl replace --force -f secondrun.yaml
kubectl delete pods firstrun
```

# Labels

Labels are key-value pairs attached to Kubernetes objects (e.g. Pods, ReplicaSets, Nodes, Namespaces, Persistent Volumes). Labels are used to organize and select a subset of objects, based on the requirements in place. Many objects can have the same Label(s). Labels do not provide uniqueness to objects. Controllers use Labels to logically group together decoupled objects, rather than using objects' names or IDs.

![](labels.png)

# Deployments

```
kubectl create deployment mynginx --image=nginx:1.15-alpine
kubectl get deploy,rs,po -l app=mynginx
kubectl scale deploy mynginx --replicas=3

kubectl rollout history deploy mynginx
kubectl rollout history deploy mynginx --revision=1

kubectl set image deployment mynginx nginx=nginx:1.16-alpine
kubectl rollout undo deployment mynginx --to-revision=1
```

![](deployments.png)

# Service

![](ssh-minikube.png)


```
kubectl run pod-hello --image=pbitty/hello-from:latest --port=80 --expose=true
kubectl get po,svc,ep --show-labels
minikube service --all
minikube ssh
curl <pod ip>
curl <srv ip>

kubectl edit svc pod-hello
edit and change to NodePort from ClusterIP
kubectl get po,svc,ep --show-labels
minikube service --all
```

```
kubectl create deployment deploy-hello --image=pbitty/hello-from:latest --port=80 --replicas=3
kubectl expose deployment deploy-hello --type=NodePort
kubectl get deploy,po,svc,ep -l app=deploy-hello --show-labels
```

![](deploy-from.png)
![](deploy-dash.png)

## Service Load Balancing (Default)

![](load-balance.png)

```
kubectl delete deployments web-dash
```

webserver.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

```
kubectl create -f webserver.yaml
kubectl get replicasets
kubectl get pods
kubectl create deployment webserver --image=nginx:alpine --replicas=3 --port=80
```

webserver-svc.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: web-service
  labels:
    app: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```

```
kubectl create -f webserver-svc.yaml
OR
kubectl expose deployment webserver --name=web-service --type=NodePort
kubectl describe service web-service
minikube ip
```
![](minikube-ip.png)

```
OR
minikube service web-service
OR
minikube service web-service --url
```

# Config Map

```
kubectl create configmap my-config \
  --from-literal=key1=value1 \
  --from-literal=key2=value2


kubectl get configmaps my-config -o yaml  
```

![](configmap.png)

# Ingress API Resource

"An Ingress is a collection of rules that allow inbound connections to reach the cluster Services".

![](Ingress.png)

```
minikube addons enable ingress
```


virtual-host-ingress.yaml

```
apiVersion: networking.k8s.io/v1 
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: "nginx"
  name: virtual-host-ingress
  namespace: default
spec:
  rules:
  - host: blue.io
    http:
      paths:
      - backend:
          service:
            name: webserver-blue-svc
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
  - host: green.io
    http:
      paths:
      - backend:
          service:
            name: webserver-green-svc
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
```

```
sudo bash -c "echo $(minikube ip) blue.io green.io >> /etc/hosts"
```

![](complete.png)


# Demo

https://youtu.be/SwFg6WED2uw

![](Kubernetes%20Youtube%20Thumbnail.png)