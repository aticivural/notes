# GENERIC COMMANDS

```
kubectl explain <resource-name>
kubectl explain pod
kubectl explain replicaset
```

```
kubectl [command] [TYPE] [NAME] -o <output_format>

-o json Output a JSON formatted API object.
-o name Print only the resource name and nothing else.
-o wide Output in the plain-text format with any additional information.
-o yaml Output a YAML formatted API object.

kubectl create namespace test-123 --dry-run -o json
kubectl create namespace test-123 --dry-run -o yaml
kubectl get pods -o wide
```

```
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml
kubectl create deployment --image=nginx nginx --dry-run -o yaml
kubectl create deployment nginx --image=nginx --replicas=4
```

```
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
(This will automatically use the pod's labels as selectors)

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
(This will not use the pods' labels as selectors; 
instead it will assume selectors as app=redis. 
You cannot pass in selectors as an option. 
So it does not work well if your pod has a different label set. 
So generate the file and modify the selectors before creating the service)
```

```
Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
(This will automatically use the pod's labels as selectors, but you cannot specify the node port. 
You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
(This will not use the pods' labels as selectors)

Both the above commands have their own challenges. 
While one of it cannot accept a selector the other cannot accept a node port. 
I would recommend going with the `kubectl expose` command. 
If you need to specify a node port, generate a definition file using the same command 
and manually input the nodeport before creating the service.
```

> CMD: image run edğildiğinde default olarak cmd de verilen komut ile çalışır
> 
> CMD sleep 5 or CMD ["sleep", "5"]
> 
> docker run ubuntu -> container çalışır ve exit eder
> 
> Dockerfile
> 
> from ubuntu
> 
> CMD sleep 5
> 
> docker build -t ubuntu-sleeper .
> 
> docker run ubuntu-sleeper -> container çalışır, 5 sn bekler, sonra exit eder
> 
> docker run ubuntu-sleeper sleep 10 -> container çalışır, 10 sn bekler, sonra exit eder, yani CMD yi ezmiş oldu
> 
> Dockerfile
> 
> from ubuntu
> 
> ENTRYPOINT ["sleep"]
> 
> docker build -t ubuntu-sleeper
> 
> docker run ubuntu-sleeper 10 -> bu durumda container sleep executable ını çalıştıracağını bilir ve parametre olarak 10 geçer, yani container 10 sn çalışır, sonra exit eder
> 
> docker run ubuntu-sleeper -> bu durumda hata alırız, çünkü sleep komutu parametre beklemektedir ancak birşey geçmediğimiz için hata verir
> 
> Dockerfile
> 
> from ubuntu
> 
> ENTRYPOINT ["sleep"]
> 
> CMD ["5"]
> 
> docker build -t ubuntu-sleeper
> 
> docker run ubuntu-sleeper -> container 5 sn çalışır, sonra exit eder
> 
> docker run ubuntu-sleeper 10 -> container 10 sn çalışır, sonra exit eder
> 
> Eğer entrypoint i de override etmek istersek **--entrypoint** flag'i ile run etmeliyiz
> 
> docker run --entrypoint sleep2.0 ubuntu-sleeper 15 -> varsayılan olarak sleep2.0 ile container ı başlatır vs 15 sonra exit eder

**CMD, ENTRYPOINT, ENVIRONMENT VARIABLES**

```
version: v1
kind: Pod
metadata:
 labels:
 app: test
spec: 
containers:
- name: ubuntu-sleeper
  image: ubuntu-sleeper
  command: ["sleep2.0"] # docker daki entrypoint karşılığı
  args: ["10"] # docker daki cmd karşılığı
  env:
    - name: APP_COLOR
      value: pink
```

```shell
#print containers inside the pod
kubectl get pods stage-pudo-pudo-as-consumer-c5574b7d5-wp5vn -o jsonpath='{.spec.containers[*].name}'
```
