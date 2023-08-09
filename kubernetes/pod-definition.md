```yaml
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
```
