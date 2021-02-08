---
title: Jaeger Operator Tutorial to create an instance of Jaeger Operator
description: This tutorial explains how to create an instance of Jaeger Operator
---

### Create Instance of Jaeger Operator

Jaeger Operator Instance can be created using the Custom Resource Definition YAML files.

**step 1:** Create a custom resource YAML file

```execute
cat <<'EOF' >jaegerInstance.yaml 
apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: jaeger
EOF
```

**Step 2:** create Jaeger Operator instance in the namespace "operators"

```execute
kubectl create -f jaegerInstance.yaml 
```

You will see the following output:

```
jaeger.jaegertracing.io/jaeger created
```

Check the Pods status:

```execute
kubectl get pods
```

You will see similar to this output:

```
NAME                          READY   STATUS    RESTARTS   AGE
pod/jaeger-788f55ddc9-ztglr   1/1     Running   0          5s
```

Check all the kubernetes resources:

```execute
kubectl get all
```


You will see similar to this output:

```
NAME                          READY   STATUS    RESTARTS   AGE
pod/jaeger-788f55ddc9-ztglr   1/1     Running   0          5s

NAME                                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                  AGE
service/jaeger-agent                ClusterIP   None            <none>        5775/UDP,5778/TCP,6831/UDP,6832/UDP      5s
service/jaeger-collector            ClusterIP   10.100.20.172   <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   5s
service/jaeger-collector-headless   ClusterIP   None            <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   5s
service/jaeger-query                ClusterIP   10.105.144.48   <none>        16686/TCP                                5s
service/kubernetes                  ClusterIP   10.96.0.1       <none>        443/TCP                                  17h

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/jaeger   1/1     1            1           5s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/jaeger-788f55ddc9   1         1         1       5s
```


### Access Jaeger's dashboard

Execute below command to edit "service/jaeger-query" Port Type from ClusterIP to LoadBalancer  :

```execute
kubectl edit service/jaeger-query
```

In the vi editor, change the type of service from ClusterIP to "LoadBalancer" as shown in below snapshot:

![](_images/change-clustertype-to-load-balancer.png)

Check all the services:

```execute
kubectl get svc
```

Output will be similar to below output:

```
NAME                                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                  AGE
service/jaeger-agent                    ClusterIP      None            <none>        5775/UDP,5778/TCP,6831/UDP,6832/UDP      4m19s
service/jaeger-collector                ClusterIP      10.111.142.2    <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   4m19s
service/jaeger-collector-headless       ClusterIP      None            <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   4m19s
service/jaeger-query                    LoadBalancer   10.109.253.83   <pending>     16686:30709/TCP                          4m19s
service/kubernetes                      ClusterIP      10.96.0.1       <none>        443/TCP                                  130m
```

The Port value to access Jaeger UI using "service/jaeger-query" is :30709

In your case you can get this Port number using below command:

  
```execute
 export PORT=$(kubectl get svc|grep jaeger-query|tr -s ' ' | cut -d ' ' -f 5|cut -d ':' -f 2|cut -d "/" -f 1)
 echo $PORT
```


We will use this PORT value to access Jaeger UI using below URL: 

```copycommand
http://##DNS.ip##:PORT
```
  

You will see the Jaeger UI as below :


 ![](_images/jaeger-ui.PNG)




