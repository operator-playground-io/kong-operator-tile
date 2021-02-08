---
title: Kong Operator Installation Verification Tutorial
description: This tutorial explains how to verify that the Kong Operator installed properly in the namespace
---

### Check the Kong Operator

After installation, verify that operator is installed successfully by executing the below command.

```execute
kubectl get csv -n operators
```

You should see a similar output as below:

```output
NAME          DISPLAY         VERSION   REPLACES      PHASE
kong.v0.7.0   Kong Operator   0.7.0     kong.v0.6.0   Succeeded
```

**Please wait till `PHASE` status will be `Succeeded` and then proceed further.**

After the installation is successful , you can check your operator's pod by executing the below command.

```execute
kubectl get pods -n operators
```

You should see a pod starting with 'kong-operator' with Ready value '1/1' and Status 'Running' like the output as below:

```output
NAME                                READY   STATUS    RESTARTS   AGE
pod/kong-operator-f7f5f9f9f-wlx4l   1/1     Running   0          15s
```

