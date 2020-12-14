# ImageHost kubernetes deployment guide

![Configuration validator](https://github.com/MSDO-ImageHost/Deployment/workflows/Configuration%20validator/badge.svg)

## Prerequisite
Fetch RabbitMQ erlang cookie
```bash
$ /bin/bash -c bake-rabbit-cookie.sh
```

Install mySQL operator (requires helm)
```bash
$ helm repo add presslabs https://presslabs.github.io/charts
$ helm install presslabs/mysql-operator --generate-name
```

## Deploy in minikube

Start minikube
```bash
$ minicube start
$ minikube addons enable ingress
```

Apply k8s configurations
```bash
$ kubectl apply -f ./kubernetes-flattened
```

Configure MongoDB replicaset synchronization
```bash
$ kubectl exec -it mongo-0 -- mongo
> rs.initiate()
> var cfg = rs.conf()
> cfg.members[0].host="mongo-0.mongo:27017"
> rs.reconfig(cfg)
> rs.add("mongo-1.mongo:27017")
> rs.add("mongo-2.mongo:27017")
> exit
// bye
```

Wait for everything to restart and connect

Open webpage
```bash
$ minikube service webgateway
```



## Optional
Forward RabbitMQ managment UI
```bash
$ kubectl port-forward rabbitmq-0 8080:15672
```
Open http://localhost:8080
