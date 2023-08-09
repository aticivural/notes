# yaml Definition

> Bir yaml da 4 alan olmak zorunda. 
> 
> apiVersion
> 
> kind
> 
> metadata
> 
> spec

# PODS

```shell
kubectl run redis --image redis1234 --dry-run=client -o yaml > redis.yaml
```

```shell
kubectl edit pods redis
```

```shell
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
```

```shell
kubectl edit pod <pod-name>
```

```shell
kubectl delete pods -l key=value
```

# Controller - ReplicaSet

```shell
kubectl scale --replicas=6 -f replicaset-definition.yml
```

```shell
kubectl scale --replicase=6 replicaset myapp-replicaset
```

> bu iki kod dosyada değişiklik yapmıyor, sadece scale ediyor

```shell
kubectl edit rs <rs-nama>
```

# Deployment

```shell
kubectl create deployment <deployment-name> --image=nginx
```

```shell
kubectl create deployment httpd-frontend --image=httpd:2.4-alpine --replicas=3
```

# CONFIGMAP

```shell
kubectl get configmaps

# imperative
kubectl create configmap <config-name> --from-literal=<key>=<value>
kubectl create configmap app-config \
 --from-literal=APP_COLOR=blue \
 --from-literal=APP_MOD=prod

kubectl create configmap <config-name> --from-file=<path-to-file>
kubectl create configmap app-config --from-file=app_config.properties
#--------------------------------------------------- 
# declerative
kubectl create -f config-map.yaml

# config-may.yaml
apiVersion: v1
kind: ConfigMap
metadata: 
    name: app-config
data:
    APP_COLOR: blue
    APP_MODE: prod
#--------------------------------------------------- 
#pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      envFrom: # bu şekilde app-config de tanımlı 2 tane config override edilmiş olur
        - configMapRef:
            name: app-config
      env: 
        - name: APP_COLOR # override edilecek env variable, bu şekilde sadece istediğimiz variable ı override etmiş oluyoruz
          valueFrom: 
            configMapKeyRef:
                name: app-config # configMap ismi
                key: APP_COLOR # app-config deki key
```

# SECRETS

```shell
kubectl create secret generic <secret-name> --from-literal=<key>=<value>
kubectl create secret generic app-secret --from-literal=DB_HOST=mysql

kubectl create secret generic <secret-name> --from-file=<path-to-file>
kubectl create secret generic <secret-name> --from-file=app_secret.properties
```

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: app-secret
data:
    DB_HOST: mysql
    DB_USER: user
    DB_PASS: pass
```

```shell
kubectl create -f app_secret.yaml
```

```shell
kubectl get secrets
kubectl describe secrets 
kubectl get secrets app-secret -o yaml
```

```yml
#pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      envFrom: # use to pass all secrets
        - secretRef:
            name: app-secret
      env: # use to pass single env variable
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
                name: app-secret
                key: DB_Password
```

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
    volumes:
    - name: app-secret-volume
      secret: 
        secretName: app-secret      
```

# SECURITY

```yaml
version: v1
kind: Pod
metadata: 
    name: web-pod
spec:
    securityContext: 
        runAsUser: 1000
    containers:
        - name: ubuntu
          image: ubuntu
          command: ["sleep", "3600"]
          securityContext: #burada olursa yukarıda tanımlanmışsa onu ezer
              runAsUser: 1001
              capabilities:
                  add: ["SYS_TIME", "MAC_ADMIN"]  # burada da bu user a istediğimiz şeyler için yetki vermiş oluyoruz
```

```shell
kubectl exec ubuntu-sleeper -- whoami # returns the user
```

# SERVICE ACCOUNT

Uygulamalar tarafından API ile konuşmak için ya da image registry den image çekmek için kullanılıyor. Eski versiyonlarda service account oluşturunca otomatik secret ı da oluşturup, service account a bağlıyordu ancak yeni versiyonlarda böyle olmuyor

```shell
kubectl create serviceaccount dashboard-sa # önce sa oluşturuyoruz
kubectl create token dashboard-sa # sonra oluşturduğumuz sa için token oluşturuyoruz
# sonra da secret oluşturup token ı sa ya bağlıyoruz
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: dashboard-secret
  annotations:
    kubernetes.io/service-account.name: dashboard-sa
type: kubernetes.io/service-account-token
EOF
```

# TAINTS & TOLERATIONS

scheduling ile ilgilidir. taint ile node'u işaretliyoruz/filtreliyoruz bu filtreyi geçemeyen podlar bu node a schedule edilemez, eğer bir pod un bu node schedule edilmesine izin vermek istersek de pod definition da tolerations kısmında taint te belirttiğimiz filtreyi eklememiz gerekiyor

```shell
kubectl taint nodes <node-name> key=value:taint-effect
taint-effect = NoSchedule | PreferNoSchedule | NoExecute
NoSchedule: bundan sonra buraya pod atama ancak mevcutlar devam etsin
PreferNoSchedule: pod atanmamasını garanti etmez
NoExecute: filtreye uymayan podlar varsa öldürür, yeni pod atanmaz

kubectl taint nodes node1 app=blue:NoSchedule # bu komut ile node u işaretledik, artık burada pod oluşturulmayacak


#pod-defition.yaml
apiVersion: v1
kind: Pod
metadata: 
    name: myapp-pod
spec:
    containers:
        - name: nginx-container
          image: nginx
    tolerations: # bu komut ile bu pod un node1 e atanmasına izin verdik ancak bu illa node1 de oluşturulacağı anlamına gelmez.
        - key: "app"
          operator: "Equal"
          value: "blue"
          effect: "NoSchedule"

#sondaki eksi taint i kaldırmaya yarıyor
k taint node controlplane node-role.kubernetes.io/control-plane-
```
