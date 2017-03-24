# spark on k8s

 Create a spark cluster on k8s, then run a Zeppelin note book to lunch a task to spark cluster and show result

### 1.  Create spark master
``` bash
[root@xhucfc02 spark]# kubectl create -f spark-master-controller.yaml
replicationcontroller "spark-master-controller" created
[root@xhucfc02 spark]# kubectl get rc
NAME                      DESIRED   CURRENT   READY     AGE
spark-master-controller   1         1         1         41s
[root@xhucfc02 spark]# kubectl get po
NAME                              READY     STATUS    RESTARTS   AGE
nginx-yn-nginx-3419328037-sw0lh   1/1       Running   0          31m
spark-master-controller-kvp23     1/1       Running   0          42s
[root@xhucfc02 spark]#
```

### 2.  Create spark master service so that spark work and Zeppelin access to by service name
``` bash
[root@xhucfc02 spark]# kubectl create -f spark-master-service.yaml 
service "spark-master" created
[root@xhucfc02 spark]# kubectl get svc
NAME             CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
kubernetes       10.0.0.1     <none>        443/TCP          46m
nginx-yn-nginx   10.0.0.237   <nodes>       80:31746/TCP     32m
spark-master     10.0.0.93    <nodes>       7077:31584/TCP   10s
[root@xhucfc02 spark]# 
```
### 3.  Create spark web gui
``` bash
[root@xhucfc02 spark]# kubectl create -f spark-webui.yaml 
service "spark-webui" created
[root@xhucfc02 spark]# kubectl get svc
NAME             CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
kubernetes       10.0.0.1     <none>        443/TCP          58m
nginx-yn-nginx   10.0.0.237   <nodes>       80:31746/TCP     44m
spark-master     10.0.0.93    <nodes>       7077:31584/TCP   11m
spark-webui      10.0.0.99    <nodes>       8080:31764/TCP   3s
[root@xhucfc02 spark]# 
```
### 4.  Create spark worker
``` bash
[root@xhucfc02 spark]# kubectl create -f spark-worker-controller.yaml 
replicationcontroller "spark-worker-controller" created
[root@xhucfc02 spark]# kubectl get rc
NAME                      DESIRED   CURRENT   READY     AGE
spark-master-controller   1         1         1         15m
spark-worker-controller   2         2         2         12s
[root@xhucfc02 spark]# 
[root@xhucfc02 spark]# kubectl get po
NAME                              READY     STATUS    RESTARTS   AGE
nginx-yn-nginx-3419328037-sw0lh   1/1       Running   0          46m
spark-master-controller-kvp23     1/1       Running   0          15m
spark-worker-controller-dpdr9     1/1       Running   0          10s
spark-worker-controller-nggtm     1/1       Running   0          10s
[root@xhucfc02 spark]# 
```
### 5.  Create Zeppelin notebook
``` bash
[root@xhucfc02 spark]# kubectl create -f zeppelin-controller.yaml
replicationcontroller "zeppelin-controller" created

[root@xhucfc02 spark]# kubectl get rc
NAME                      DESIRED   CURRENT   READY     AGE
spark-master-controller   1         1         1         18m
spark-worker-controller   2         2         2         3m
zeppelin-controller       1         1         1         16s
[root@xhucfc02 spark]# kubectl get po
NAME                              READY     STATUS    RESTARTS   AGE
nginx-yn-nginx-3419328037-sw0lh   1/1       Running   0          49m
spark-master-controller-kvp23     1/1       Running   0          19m
spark-worker-controller-dpdr9     1/1       Running   0          3m
spark-worker-controller-nggtm     1/1       Running   0          3m
zeppelin-controller-dhs9x         1/1       Running   0          19s
[root@xhucfc02 spark]#
```

### 6.  Create Zeppelin notebook service
``` bash
[root@xhucfc02 spark]# kubectl create -f zeppelin-service.yaml 
service "zeppelin" created
[root@xhucfc02 spark]# kubectl get svc
NAME             CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
kubernetes       10.0.0.1     <none>        443/TCP          3h
nginx-yn-nginx   10.0.0.237   <nodes>       80:31746/TCP     2h
spark-master     10.0.0.93    <nodes>       7077:31584/TCP   2h
spark-webui      10.0.0.99    <nodes>       8080:31764/TCP   2h
zeppelin         10.0.0.141   <nodes>       8080:30980/TCP   2s
[root@xhucfc02 spark]# 
```

## Reference

[1] https://github.com/kubernetes/kubernetes/tree/release-1.4/examples/spark
[2] https://zeppelin.apache.org/docs/0.5.5-incubating/tutorial/tutorial.html Zeppelin


