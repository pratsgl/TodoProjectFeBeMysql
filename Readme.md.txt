https://medium.com/better-programming/kubernetes-a-detailed-example-of-deployment-of-a-stateful-application-de3de33c8632

https://github.com/shri-kanth/kuberenetes-demo-manifests


https://hub.docker.com/r/kubernetesdemo/to-do-app-frontend/
https://hub.docker.com/r/kubernetesdemo/to-do-app-backend/

[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl get secrets --all-namespaces | grep db
default           db-credentials                                   Opaque                                2      6m44s
default           db-root-credentials                              Opaque                                1      7m38s

[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl get configmap --all-namespaces | grep db
default       db-conf                              2      9m45s

[g702892@da3q-gen-imn-app100 ~]$ kubectl get pods --all-namespaces | grep mysql
NAMESPACE     NAME                                             READY   STATUS    RESTARTS   AGE     IP               NODE                  NOMINATED NODE   READINESS GATES
default       mysql-656c77d597-qfld4                           1/1     Running   0          12m

[g702892@da3q-gen-imn-app100 ~]$ kubectl get pv,pvc --all-namespaces | grep mysql
persistentvolume/pvc-001b9d0e-3817-4175-9940-6249571250b1   1Gi        RWO            Delete           Bound    default/mysql-pv-claim               nfs-client              13m
default      persistentvolumeclaim/mysql-pv-claim            Bound    pvc-001b9d0e-3817-4175-9940-6249571250b1   1Gi        RWO            nfs-client     13m

[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl get deployments --all-namespaces | egrep -i "to-do|mysql"
default       mysql                           1/1     1            1           20m
default       to-do-app-backend               2/2     2            2           73s

[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl get svc  --all-namespaces | egrep -i "to-do|mysql"
default       mysql                           ClusterIP      None             <none>        3306/TCP                 22m
default       to-do-app-backend               LoadBalancer   10.100.179.111   <pending>     80:31352/TCP             2m48s

[g702892@da3q-gen-imn-app100 TodoProject]$  kubectl get replicasets
NAME                           DESIRED   CURRENT   READY   AGE
mysql-656c77d597               1         1         1       4h53m
to-do-app-backend-5b9496bf96   2         2         2       4h34m

[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl describe services to-do-app-backend
Name:                     to-do-app-backend
Namespace:                default
Labels:                   <none>
Annotations:              Selector:  app=to-do-app,tier=backend
Type:                     LoadBalancer
IP:                       10.100.179.111
Port:                     <unset>  80/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31352/TCP
Endpoints:                10.40.185.18:8080,10.42.150.82:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>



[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl get replicasets
NAME                            DESIRED   CURRENT   READY   AGE
mysql-656c77d597                1         1         1       5h1m
to-do-app-backend-5b9496bf96    2         2         2       4h42m
to-do-app-frontend-76666776fc   2         2         0       44s


[g702892@da3d-gen-imn-svr002 ~]$ kubectl get svc
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP        11d
mysql                ClusterIP   None             <none>        3306/TCP       5h17m
to-do-app-backend    NodePort    10.100.179.111   <none>        80:31352/TCP   4h58m
to-do-app-frontend   NodePort    10.101.186.233   <none>        80:31212/TCP   17m


[g702892@da3d-gen-imn-svr002 ~]$ kubectl get pods --all-namespaces -o wide
NAMESPACE     NAME                                             READY   STATUS    RESTARTS   AGE     IP               NODE                  NOMINATED NODE   READINESS GATES
default       mysql-656c77d597-qfld4                           1/1     Running   0          5h18m   10.40.185.17     da3d-gen-imn-svr003   <none>           <none>
default       to-do-app-backend-5b9496bf96-g52mm               1/1     Running   0          4h59m   10.42.150.82     da3d-gen-imn-svr004   <none>           <none>
default       to-do-app-backend-5b9496bf96-lnldd               1/1     Running   0          4h59m   10.40.185.18     da3d-gen-imn-svr003   <none>           <none>
default       to-do-app-frontend-76666776fc-bj4wt              1/1     Running   0          17m     10.42.150.83     da3d-gen-imn-svr004   <none>           <none>
default       to-do-app-frontend-76666776fc-pc6lt              1/1     Running   0          17m     10.40.185.19     da3d-gen-imn-svr003   <none>           <none>
jenkins       jenkins-6ff46686cd-6ds8p                         1/1     Running   0          6d2h    10.42.150.81     da3d-gen-imn-svr004   <none>           <none>
kube-system   calico-kube-controllers-76d4774d89-rmvzj         1/1     Running   0          11d     10.41.47.193     da3d-gen-imn-svr002   <none>           <none>
kube-system   calico-node-5dl48                                1/1     Running   0          10d     10.165.209.219   da3d-gen-imn-svr004   <none>           <none>
kube-system   calico-node-bgj4j                                1/1     Running   0          11d     10.165.209.218   da3d-gen-imn-svr003   <none>           <none>
kube-system   calico-node-cw54h                                1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   coredns-66bff467f8-bg9kc                         1/1     Running   0          11d     10.41.47.195     da3d-gen-imn-svr002   <none>           <none>
kube-system   coredns-66bff467f8-bsr5r                         1/1     Running   0          11d     10.41.47.194     da3d-gen-imn-svr002   <none>           <none>
kube-system   etcd-da3d-gen-imn-svr002                         1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   kube-apiserver-da3d-gen-imn-svr002               1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   kube-controller-manager-da3d-gen-imn-svr002      1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   kube-proxy-8p25x                                 1/1     Running   0          11d     10.165.209.218   da3d-gen-imn-svr003   <none>           <none>
kube-system   kube-proxy-8x2r4                                 1/1     Running   0          10d     10.165.209.219   da3d-gen-imn-svr004   <none>           <none>
kube-system   kube-proxy-vh6mb                                 1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   kube-scheduler-da3d-gen-imn-svr002               1/1     Running   0          11d     10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
kube-system   metrics-server-7b5f965dd7-l5x6p                  1/1     Running   0          9d      10.165.209.218   da3d-gen-imn-svr003   <none>           <none>
kube-system   nfs-client-provisioner-56b4cc9d-8gt7b            1/1     Running   0          9d      10.42.150.71     da3d-gen-imn-svr004   <none>           <none>
monitoring    prometheus-alertmanager-59b97b8b6f-tmrls         2/2     Running   0          9d      10.42.150.73     da3d-gen-imn-svr004   <none>           <none>
monitoring    prometheus-kube-state-metrics-55c864f698-lmwgj   1/1     Running   0          9d      10.40.185.11     da3d-gen-imn-svr003   <none>           <none>
monitoring    prometheus-node-exporter-n5llg                   1/1     Running   0          9d      10.165.209.220   da3d-gen-imn-svr002   <none>           <none>
monitoring    prometheus-node-exporter-nvjlp                   1/1     Running   0          9d      10.165.209.218   da3d-gen-imn-svr003   <none>           <none>
monitoring    prometheus-node-exporter-s298r                   1/1     Running   0          9d      10.165.209.219   da3d-gen-imn-svr004   <none>           <none>
monitoring    prometheus-pushgateway-98d8dc9b-pcdfq            1/1     Running   0          9d      10.42.150.72     da3d-gen-imn-svr004   <none>           <none>
monitoring    prometheus-server-79b77d8899-bgjpc               2/2     Running   0          9d      10.40.185.12     da3d-gen-imn-svr003   <none>           <none>

[g702892@da3d-gen-imn-svr002 ~]$  kubectl port-forward --address localhost,10.165.209.220  -n default pod/to-do-app-frontend-76666776fc-bj4wt 8080:8080
Forwarding from 10.165.209.220:8080 -> 8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080





[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl exec -it -n default mysql-656c77d597-qfld4 bash

root@mysql-656c77d597-qfld4:/# mysql -u root -p
Enter password:magic
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 289
Server version: 5.7.30 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| to-do-app-db       |
+--------------------+
5 rows in set (0.00 sec)

mysql> use to-do-app-db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------------+
| Tables_in_to-do-app-db |
+------------------------+
| ITEM                   |
| LIST_ENTITY            |
| schema_version         |
+------------------------+
3 rows in set (0.00 sec)

mysql> select * from ITEM;
Empty set (0.00 sec)

mysql>
mysql> exit
Bye
root@mysql-656c77d597-qfld4:/# exit
exit



[g702892@da3q-gen-imn-app100 ~]$ kubectl get all
NAME                                      READY   STATUS    RESTARTS   AGE
pod/mysql-656c77d597-qfld4                1/1     Running   0          20h
pod/to-do-app-backend-5b9496bf96-g52mm    1/1     Running   0          19h
pod/to-do-app-backend-5b9496bf96-lnldd    1/1     Running   0          19h
pod/to-do-app-frontend-76666776fc-d6bwh   1/1     Running   0          13h
pod/to-do-app-frontend-76666776fc-l9x24   1/1     Running   0          13h

NAME                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP        11d
service/mysql                ClusterIP   None             <none>        3306/TCP       20h
service/to-do-app-backend    NodePort    10.100.179.111   <none>        80:31352/TCP   19h
service/to-do-app-frontend   NodePort    10.101.186.233   <none>        80:31212/TCP   15h

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql                1/1     1            1           20h
deployment.apps/to-do-app-backend    2/2     2            2           19h
deployment.apps/to-do-app-frontend   2/2     2            2           13h

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-656c77d597                1         1         1       20h
replicaset.apps/to-do-app-backend-5b9496bf96    2         2         2       19h
replicaset.apps/to-do-app-frontend-76666776fc   2         2         2       13h
[g702892@da3q-gen-imn-app100 ~]$
[g702892@da3q-gen-imn-app100 ~]$



[g702892@da3d-gen-imn-svr002 ~]$ kubectl get svc -n default -o wide
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE   SELECTOR
kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP        12d   <none>
mysql                ClusterIP   None             <none>        3306/TCP       31h   app=mysql,tier=database
to-do-app-backend    NodePort    10.100.179.111   <none>        80:31352/TCP   31h   app=to-do-app,tier=backend
to-do-app-frontend   NodePort    10.101.186.233   <none>        80:31212/TCP   26h   app=to-do-app,tier=frontend


[g702892@da3q-gen-imn-app100 TodoProject]$ cat backend-configMap.yaml
# ConfigMap to expose configuration related to backend application
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-conf # name of configMap
data:
  server-uri: 10.165.209.220:31212 # enternal ip of backend application 'Service'



[g702892@da3d-gen-imn-svr002 ~]$ kubectl get nodes -o wide
NAME                  STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE                                      KERNEL-VERSION               CONTAINER-RUNTIME
da3d-gen-imn-svr002   Ready    master   13d   v1.18.2   10.165.209.220   <none>        Red Hat Enterprise Linux Server 7.6 (Maipo)   3.10.0-957.10.1.el7.x86_64   docker://19.3.8
da3d-gen-imn-svr003   Ready    <none>   13d   v1.18.2   10.165.209.218   <none>        Red Hat Enterprise Linux Server 7.6 (Maipo)   3.10.0-957.10.1.el7.x86_64   docker://19.3.8
da3d-gen-imn-svr004   Ready    <none>   11d   v1.18.2   10.165.209.219   <none>        Red Hat Enterprise Linux Server 7.6 (Maipo)   3.10.0-957.10.1.el7.x86_64   docker://19.3.8

[g702892@da3d-gen-imn-svr002 ~]$ kubectl get svc -n default -o wide
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE    SELECTOR
kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP        13d    <none>
mysql                ClusterIP   None             <none>        3306/TCP       2d4h   app=mysql,tier=database
to-do-app-backend    NodePort    10.100.179.111   <none>        80:31352/TCP   2d3h   app=to-do-app,tier=backend
to-do-app-frontend   NodePort    10.101.186.233   <none>        80:31212/TCP   47h    app=to-do-app,tier=frontend


You can access the application using :: MasternodeIP:to-do-app-frontend-port-number 
Example : http://10.165.209.220:31212



[g702892@da3q-gen-imn-app100 TodoProject]$ kubectl exec -it -n default mysql-656c77d597-qfld4 bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl kubectl exec [POD] -- [COMMAND] instead.
root@mysql-656c77d597-qfld4:/#
root@mysql-656c77d597-qfld4:/#
root@mysql-656c77d597-qfld4:/# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1278
Server version: 5.7.30 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| to-do-app-db       |
+--------------------+
5 rows in set (0.00 sec)

mysql> use to-do-app-db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------------+
| Tables_in_to-do-app-db |
+------------------------+
| ITEM                   |
| LIST_ENTITY            |
| schema_version         |
+------------------------+
3 rows in set (0.00 sec)

mysql> select * from ITEM;
+----+---------------------+---------+---------------------+-------------+---------+----------+
| ID | CREATED_ON          | DELETED | MODIFIED_ON         | NAME        | LIST_ID | POSITION |
+----+---------------------+---------+---------------------+-------------+---------+----------+
|  1 | 2020-07-13 14:36:10 |       0 | 2020-07-13 14:36:10 | Rice/Curry  |       3 |        0 |
|  2 | 2020-07-13 14:36:20 |       0 | 2020-07-13 14:36:20 | Barista     |       1 |        0 |
|  3 | 2020-07-13 14:36:28 |       0 | 2020-07-13 14:36:28 | Continental |       2 |        0 |
|  4 | 2020-07-13 14:36:54 |       0 | 2020-07-13 14:36:54 | Green Salad |       4 |        0 |
|  5 | 2020-07-13 14:37:10 |       0 | 2020-07-13 14:37:10 | Fruit Bowl  |       5 |        0 |
+----+---------------------+---------+---------------------+-------------+---------+----------+
5 rows in set (0.00 sec)

mysql> select * from LIST_ENTITY;
+----+---------------------+---------+---------------------+----------------+----------+
| ID | CREATED_ON          | DELETED | MODIFIED_ON         | NAME           | POSITION |
+----+---------------------+---------+---------------------+----------------+----------+
|  1 | 2020-07-13 14:28:42 |       0 | 2020-07-13 14:28:42 | Morning Coffee |        0 |
|  2 | 2020-07-13 14:28:58 |       0 | 2020-07-13 14:28:58 | Breakfast      |        1 |
|  3 | 2020-07-13 14:29:08 |       0 | 2020-07-13 14:29:08 | Lunch          |        2 |
|  4 | 2020-07-13 14:29:21 |       0 | 2020-07-13 14:29:21 | Snacks         |        3 |
|  5 | 2020-07-13 14:37:01 |       0 | 2020-07-13 14:37:01 | Dinner         |        4 |
+----+---------------------+---------+---------------------+----------------+----------+
5 rows in set (0.00 sec)

mysql>













