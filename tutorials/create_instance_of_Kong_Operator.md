---
title: Jaeger Operator Tutorial to create an instance of Kong Operator
description: This tutorial explains how to create an instance of Kong Operator
---



### Create Kong Ingress Controller 

Kong Ingress Controller  can be created using below Custom Resource Definition YAML file:

**Step 1:** Create a custom resource YAML file

```execute
cat <<'EOF' >kongInstance.yaml 
apiVersion: charts.helm.k8s.io/v1alpha1
kind: Kong
metadata:
  name: example-kong
spec:
  proxy:
    type: NodePort
  env:
    prefix: /kong_prefix/
  resources:
    limits:
      cpu: 500m
      memory: 2G
    requests:
      cpu: 100m
      memory: 512Mi
  ingressController:
    enabled: true
    ingressClass: example-ingress-class
    installCRDs: false
EOF
```

**Step 2:** Deploy the Kong Ingress Controller with "example-ingress-class" Ingress class:

```execute
kubectl create -f kongInstance.yaml 
```

You will see the following resources created:

```
kong.charts.helm.k8s.io/example-kong created
```

Please wait till Pod STATUS will be "Running" and then proceed further.


Check pods status:


```execute
kubectl get pods
```

You will see a similar Output as below:

```
NAME                                 READY   STATUS    RESTARTS   AGE
example-kong-kong-86777c7d7b-pq2sk   2/2     Running   0          36m
```

Check all the kubernetes resources:

```execute
kubectl get all
```


You will see similar to this output:

```
NAME                                     READY   STATUS    RESTARTS   AGE
pod/example-kong-kong-86777c7d7b-pq2sk   2/2     Running   0          37m

NAME                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/example-kong-kong-proxy   NodePort    10.109.156.184   <none>        80:30285/TCP,443:32470/TCP   37m
service/kubernetes                ClusterIP   10.96.0.1        <none>        443/TCP                      23h

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/example-kong-kong   1/1     1            1           37m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/example-kong-kong-86777c7d7b   1         1         1       37m
```




