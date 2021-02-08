

1. Deploy an example Service and expose it with an Ingress:
    1. Deploy the echo service:
        ```
        kubectl apply -f https://bit.ly/echo-service
        ```
    1. Create an Ingress:
        ```
        kubectl create -f - <<EOF
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
1. See that Kong works and relays requests to the application!
    ```
    PROXY_IP=$(kubectl get service example-kong-kong-proxy -o jsonpath={.spec.clusterIP})
    ```
    ```
    curl http://$PROXY_IP/foo/
    ```
