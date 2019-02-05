# Django Kubernetes Setup

## Sources
```
https://medium.com/google-cloud/deploying-django-postgres-redis-containers-to-kubernetes-9ee28e7a146
```

## Theory Knowledge

![Kubernetes Architecture](images/kubernetes_architecture.jpg)

Nodes
```
Linux machines running Docker containers that make up the cluster
```

Pods
```
Basic unit of orchestration. Pod has one or more containers running inside of it.
Pods can have an arbitrary amount of Labels which are used by Services and
Replication Controllers to match with Pods.
```

Replication Controller
```
Controls how many of a given Pod exist, based on a provided label. ex. 1 or 10 of a given Pod. 
```

Service
```
Communication between Pods even if they're on different Nodes. A Service provides an IP on a virtual private network.
```

Persistent Volume
```
A PV resource is a piece of storage on the cluster analogous to a node (physical device or VM). Captures how storage is implemented.
```

Persistent Volume Claim
```
A PVC is a request for storage by the user and allows for the user to consume abstract storage resources on the cluster.
```

## Plan
- https://medium.com/@markgituma/kubernetes-local-to-production-with-django-1-introduction-d73adc9ce4b4
    - create simple django app in docker container and deploy in lcoal K8 cluster using minikube
    - run postgresql db running as pod in cluster
    - add redis cache and celery for async processing
- GCP setup: https://medium.com/google-cloud/deploying-django-postgres-redis-containers-to-kubernetes-9ee28e7a146
    - gcp project setup
    - redis setup
    - postgresql gcp setup
    - serve static content using CDN
    - local dev using minikube and port forwarding and hot reloading

## Development Notes
- Using DB as a service provides features that can be hard to implement and manage well
    - managed high availability and redundancy
    - easy vertical scaling of computational resources to handle variable load
    - easy horizontal scaling by creating replicated instances to handle high traffic
    - easy snapshots and backup
    - security patches and db monitoring
- Kubernetes allows the use of the service name i.e. postgres-service for domain name resolution to the pod IP.


## Steps
- Step 1
    - setup minikube kubectl
    - create base image for sample app using Docker
    - start deployment of app on minikube
    - start service of app to expose app outside of cluster
- Step 2
    - use persistent volume subsystem for postgresql and setup PVC
    - use Secret resource API to handle credentials used by PostgreSQL and Django pods
    - initialize postgresql pods to use persistent volume
    - expose db as service to allow for access within cluster
    - update django app to access db
    - run migrations using Job API
