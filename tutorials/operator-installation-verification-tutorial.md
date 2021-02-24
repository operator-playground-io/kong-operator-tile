---
title: Kong Operator Installation Verification
description: Learn how to verify that the Kong Operator installed properly in the namespace
---

### Kong Operator Installation verification

After installation, verify that Kong operator has been installed successfully.

To do so, run the following command.

**Step 1: Execute `kubectl get csv` command.**

```execute
kubectl get csv -n operators
```

This should produce the output as below.

```output
NAME          DISPLAY         VERSION   REPLACES      PHASE
kong.v0.7.0   Kong Operator   0.7.0     kong.v0.6.0   Succeeded
```

Please wait until the PHASE status is "Succeeded", then proceed.

**Step 2: Check the pods.**

After the installation is successful, you can check your operator's pod by executing the following command.

```execute
kubectl get pods -n operators
```

You should see the pod details as:
-	Name starting with 'kong-operator'
-	Ready value as '1/1' 
-	Status as 'Running'.

See the sample output below.

```
NAME                                READY   STATUS    RESTARTS   AGE
pod/kong-operator-f7f5f9f9f-wlx4l   1/1     Running   0          15s
```

