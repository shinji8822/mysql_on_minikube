# mysql_on_minikube

### minikube 起動
```
minikube start
```

### PersistentVolumeデプロイ
```
kubectl apply -f mysql/mysql-pv.yaml

kubectl get pv mysql-pv-volume
NAME              CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                    STORAGECLASS   REASON   AGE
mysql-pv-volume   1Gi        RWO            Retain           Bound    default/mysql-pv-claim   manual                  27h

kubectl get pvc mysql-pv-claim
NAME             STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    mysql-pv-volume   1Gi        RWO            manual         27h
```

### MySQL5.7デプロイ
```
kubectl apply -f mysql-deployment.yaml

kubectl get pod
NAME                     READY   STATUS    RESTARTS     AGE
mysql-6786c8b4f4-vs8rd   1/1     Running   1 (8h ago)   26h

kubectl get service
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          31h
mysql        NodePort    10.107.59.186   <none>        3306:30000/TCP   26h

minikube service list
|-------------|------------|--------------|----------------------------|
|  NAMESPACE  |    NAME    | TARGET PORT  |            URL             |
|-------------|------------|--------------|----------------------------|
| default     | kubernetes | No node port |
| default     | mysql      |         3306 | http://192.168.39.32:30000 |
| kube-system | kube-dns   | No node port |
|-------------|------------|--------------|----------------------------|

minikube ip
192.168.39.32
```

### connect to MySQL
```
mysql -u root -p -P 30000 -h $(minikube ip)
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 5.7.41 MySQL Community Server (GPL)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

```
