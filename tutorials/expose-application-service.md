---
title: Kong Operator Tutorial to deploy an example Application Service and expose it with Kong Ingress
description: This tutorial explains how to deploy an example Application Service and expose it with Kong Ingress
---

In order to deploy an example Application Service and expose it with Kong Ingress, Please follow following steps:


Step 1. Deploy the Application "echo" service:
        
        ```execute
        kubectl create -f https://bit.ly/echo-service
        ```
        
        Output:
        ```
        
        ```
Step 2. Create an Ingress:
        
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
        
       Output:
       
       ```
       ```
        
Step 3. See that Kong Ingress works and relays requests to the application.
   
    ```execute
    PROXY_IP=$(kubectl get service example-kong-kong-proxy -o jsonpath={.spec.clusterIP})
    ```
    
- Make a request to the application service using curl command:
    
    ```execute
    curl http://$PROXY_IP/foo/
    ```
    
    Output:
    
    ```
    
    ```
