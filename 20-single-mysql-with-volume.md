
```
apiVersion: v1
kind: Pod
metadata:
  name: mysqlvolume
  labels:
    name: mysql
spec:
  containers:
  - name: ps
    image: perconalab/percona-server
    env:
    - name: MYSQL_ROOT_PASSWORD
          # change this
      value: yourpassword
    ports:
    - containerPort: 3306
    volumeMounts:
    - mountPath: /var/lib/mysql
      name: data-volume
  volumes:
  - name: data-volume
    hostPath:
      # directory location on host
      path: /data/flash/volume
```
