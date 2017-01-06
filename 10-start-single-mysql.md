mysql.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    name: mysql
spec:
  containers:
  - name: ps
    image: percona/percona-server
    env:
    - name: MYSQL_ROOT_PASSWORD
          # change this
      value: yourpassword
    ports:
    - containerPort: 3306
```

`kubectl create -f mysql.yaml`
