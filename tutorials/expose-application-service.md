---
title: Kong Operator Tutorial to deploy an example Application Service and expose it with Kong Ingress
description: This tutorial explains how to deploy an example Application Service and expose it with Kong Ingress
---

### Deploy an example Application Service and expose it with Kong Ingress


In order to deploy an example Application Service and expose it with Kong Ingress, Please follow below steps:


Step 1. Deploy the Application "echo" service usinge below command:
        
```execute
kubectl create -f https://bit.ly/echo-service
```
       
       
You will see a similar Output as below:

```
service/echo created
deployment.apps/echo created
```

Please wait till Pod STATUS will be "Running" and then proceed further.


Check pods status:

```execute
kubectl get pods 
```
You will see Output similar to this:

```
NAME                                 READY   STATUS    RESTARTS   AGE
echo-85fb7989cc-56bmr                1/1     Running   0          36m
example-kong-kong-86777c7d7b-pq2sk   2/2     Running   0          36m
```

Check all the kubernetes resources:

```execute
kubectl get all 
```


You will see similar to this output:

```
NAME                                     READY   STATUS    RESTARTS   AGE
pod/echo-85fb7989cc-sksnf                1/1     Running   0          13m
pod/example-kong-kong-86777c7d7b-zrmkc   2/2     Running   0          15m

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/echo                      ClusterIP   10.106.166.98   <none>        8080/TCP,80/TCP              13m
service/example-kong-kong-proxy   NodePort    10.99.37.0      <none>        80:32352/TCP,443:32372/TCP   15m
service/kubernetes                ClusterIP   10.96.0.1       <none>        443/TCP                      49m

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/echo                1/1     1            1           13m
deployment.apps/example-kong-kong   1/1     1            1           15m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/echo-85fb7989cc                1         1         1       13m
replicaset.apps/example-kong-kong-86777c7d7b   1         1         1       15m
```


       
Step 2. Create below yaml which will create a Custom Resource for Kong Ingress:
        
```execute
cat <<'EOF' >kongIngress.yaml 
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: demo
  annotations:
    # Note that the annotation below is deprecated as of Kubernetes 1.18
    # in favor of the new spec.ingressClassName field. At the moment of writing
    # (Kong Ingress Controller v0.9.0), Kong Ingress Controller does not support
    # the new format yet.
    kubernetes.io/ingress.class: example-ingress-class
spec:
  rules:
  - http:
      paths:
      - path: /foo
        backend:
          serviceName: echo
          servicePort: 8080
EOF
```
        
- Execute below command to create ingress instance:

```execute
kubectl create -f kongIngress.yaml  
```

You will see Output similar to this:
       
```
ingress.extensions/demo created
```


Check pods status:

```execute
kubectl get pods 
```
You will see Output similar to this:

```
NAME                                 READY   STATUS    RESTARTS   AGE
echo-85fb7989cc-56bmr                1/1     Running   0          36m
example-kong-kong-86777c7d7b-pq2sk   2/2     Running   0          36m
```

Please wait till Pod STATUS will be "Running" and then proceed further.

        
Step 3. Verify that Kong Ingress works and relays requests to the application.
   
```execute
 PROXY_IP=$(kubectl get service example-kong-kong-proxy -o jsonpath={.spec.clusterIP})
```
    
- Make a request to the application service using curl command:
    
```execute
curl http://$PROXY_IP/foo/
```
    
You will see the response as follow:
    
Output:
    
```
Hostname: echo-85fb7989cc-56bmr

Pod Information:
        node name:      event-k8s-ibm-operators-playground-fztmds.softlayer.com
        pod name:       echo-85fb7989cc-56bmr
        pod namespace:  default
        pod IP: 10.244.0.43

Server values:
        server_version=nginx: 1.12.2 - lua: 10010

Request Information:
        client_address=10.244.0.42
        method=GET
        real path=/foo/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://10.109.156.184:8080/foo/

Request Headers:
        accept=*/*  
        connection=keep-alive  
        host=10.109.156.184  
        user-agent=curl/7.29.0  
        x-forwarded-for=10.244.0.1  
        x-forwarded-host=10.109.156.184  
        x-forwarded-path=/foo/  
        x-forwarded-port=80  
        x-forwarded-proto=http  
        x-real-ip=10.244.0.1  

Request Body:
        -no body in request-
```


***Conclusion: From the above response output we can ensure that application service is accessible using this kong ingress controller.***
