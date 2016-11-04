The goal is to have a persistent (local) volume for the database, which would survive POD restarts

mysql-volume.yaml:

```
apiVersion: v1
kind: Service
metadata:
  labels:
    name: mysql
  name: mysqlvolumeservice
spec:
  ports:
    # the port that this service should serve on
    - port: 3306
  # label keys and values that must match in order to receive traffic for this service
  selector:
    name: mysql
---
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

Start POD and service
`kubectl create -f mysql-volume.yaml`

See attached IP address
`kubectl describe -f mysql-volume.yaml`

Teardown:
`kubectl delete -f mysql-volume.yaml`
