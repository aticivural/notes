# COLIMA

```shell
colima start --cpu 8 --memory 10240
colime stop
colima list
colima delete list
colima status
```

# MINIKUBE

```shell
minikube config set memory 8192
minikube config set cpus 8
minikube start
minikube stop
minikube list
minikube status

minikube start -p demo --cpus 7 --memory 9216
minikube stop -p demo
minikube delete -p demo
minikube ip -p test
minikube image load kafka-kraft:latest -p test
```
