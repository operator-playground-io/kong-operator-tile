<h1 align="center">Kong Operator</h1>

![Logo](_images/kong-logo.png)


### Overview:

Kong is a popular open-source cloud-native API gateway. Kong Operator is a Kubernetes operator which manages Kong and Kong Enterprise clusters.
With Kong Operator running in your cluster, you can spin up multiple instances of Kong, each of them configured by a Kong custom resource.

Kong Operator can deploy Kong in various configurations, for example:

- as a Kubernetes ingress controller, enabling you to expose Kubernetes Services via Kong,
- a standalone Kong gateway (without the ingress controller; either DB-enabled or DB-less)
- a standalone Ingress Controller (configuring an external instance of Kong)

### Operator's features are as follows:




### Objective of tutorial

In this tutorial,we are going to cover following topics:

1. Install Kong Operator and verify its successful installation.
2. Create Kong Instance to deploy Kong Ingress Controller and verify status of pods and services.
3. Deploy an example application Service and expose it with kong Ingress.
4. Verify that Kong ingress works and relays requests to the application.
5. Cleanup Operator.
