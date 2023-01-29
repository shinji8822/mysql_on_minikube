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

```
