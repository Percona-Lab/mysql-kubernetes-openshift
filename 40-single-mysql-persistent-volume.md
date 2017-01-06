The goal is to use a persistent volume to store database 

pv.json
```json
{                                                                                                                 
    "apiVersion": "v1",                                                                                           
    "kind": "PersistentVolume",
    "metadata": {                                                                                                 
        "name": "default-drive"
    },
    "spec": {                                                                                                     
        "capacity": {
            "storage": "100Gi"                                                                                     
        },      
        "accessModes": [                                                                                          
            "ReadWriteMany"                                                                                       
        ],                                                                                                        
        "persistentVolumeReclaimPolicy": "Recycle",
        "nfs": {                                                                                    
            "path": "/data/flash/pv0001",
            "server": "127.0.0.1"
        }
    }
}
{                                                                                                                 
    "apiVersion": "v1",                                                                                           
    "kind": "PersistentVolumeClaim",                                                                              
    "metadata": {                                                                                                 
        "name": "mysql-pv-claim"                                                                             
    },                                                                                                            
    "spec": {                                                                                                     
        "accessModes": [                                                                                          
            "ReadWriteMany"                                                                                       
        ],                                                                                                        
        "resources": {                                                                                            
            "requests": {                                                                                         
                "storage": "10Gi"                                                                                 
            }                                                                                                     
        }                                                                                                         
    }                                                                                                             
}
```

mysql-pv.yaml
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
  name: mysqlvolumesecret
  labels:
    name: mysql
spec:
  containers:
  - name: ps
    image: percona/percona-server
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: testkube
    ports:
    - containerPort: 3306
    volumeMounts:
    - mountPath: /var/lib/mysql
      name: data-volume
  volumes:
  - name: data-volume
    persistentVolumeClaim:
      claimName: mysql-pv-claim
```
