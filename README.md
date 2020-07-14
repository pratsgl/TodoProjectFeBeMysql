# TodoProjectFeBeMysql

https://medium.com/better-programming/kubernetes-a-detailed-example-of-deployment-of-a-stateful-application-de3de33c8632
https://github.com/shri-kanth/kuberenetes-demo-manifests
https://hub.docker.com/r/kubernetesdemo/to-do-app-frontend/
https://hub.docker.com/r/kubernetesdemo/to-do-app-backend/


![Alt](1*78_FSXLxWFY8eOQS-zJ8qA.png)

user@pradeep-lab-system:~/projects/kubernetes/vagrant-provisioning$ kubectl get secrets -n default
NAME                  TYPE                                  DATA   AGE
db-credentials        Opaque                                2      6h30m
db-root-credentials   Opaque                                1      6h33m
default-token-97qmt   kubernetes.io/service-account-token   3      7h42m

user@pradeep-lab-system:~/projects/kubernetes/vagrant-provisioning$ kubectl get configmaps -n default
NAME           DATA   AGE
backend-conf   1      5h45m
db-conf        2      6h42m

user@pradeep-lab-system:~/projects/kubernetes/vagrant-provisioning$ kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
mysql-656c77d597-82rsm                1/1     Running   0          4h50m
to-do-app-backend-5b9496bf96-4bsqp    1/1     Running   1          4h50m
to-do-app-backend-5b9496bf96-cbfcq    1/1     Running   1          5h58m
to-do-app-frontend-76666776fc-bcr4d   1/1     Running   1          5h44m
to-do-app-frontend-76666776fc-tzmsq   1/1     Running   0          4h50m


user@pradeep-lab-system:~/projects/kubernetes/vagrant-provisioning$ kubectl get pv,pvc,storageclass -n default
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                STORAGECLASS   REASON   AGE
persistentvolume/pvc-19c2b98e-7be7-4ad1-a9e7-03cb0b01fdc7   1Gi        RWO            Delete           Bound    default/mysql-pv-claim               nfs-client              6h3m
persistentvolume/pvc-7030e9f2-6469-4687-ab3b-c5a240d6570d   8Gi        RWO            Delete           Bound    monitoring/prometheus-server         nfs-client              3h50m
persistentvolume/pvc-9b75e9c3-c8db-41b4-9ffa-40dd390d2ccc   2Gi        RWO            Delete           Bound    monitoring/prometheus-alertmanager   nfs-client              3h50m

NAME                                   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/mysql-pv-claim   Bound    pvc-19c2b98e-7be7-4ad1-a9e7-03cb0b01fdc7   1Gi        RWO            nfs-client     6h3m

NAME                                               PROVISIONER              RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
storageclass.storage.k8s.io/nfs-client (default)   nfs-client-provisioner   Delete          Immediate           true                   6h10m



user@pradeep-lab-system:~/projects/kubernetes/vagrant-provisioning$ kubectl get deployments -n default
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
mysql                1/1     1            1           6h4m
