---
title: Kong Operator clean up
description: Learn how to cleanup the Kong Operator
---


### Cleaning Up Operator



**Step 1: Delete the operator's custom resources by using `kubectl delete` commands as below.**

 
 ```execute
 kubectl delete -f kongInstance.yaml
 kubectl delete -f https://bit.ly/echo-service
 kubectl delete -f kongIngress.yaml 
 ```


**Step 2: Delete the operator by using `kubectl delete` command followed by target link as below.**
 
   
 ```execute
 kubectl delete -f https://operatorhub.io/install/kong.yaml
 ```
 

 **Step 3: Delete all the yaml files.**
 
 
  ```execute
  rm -rf kongInstance.yaml
  rm -rf kongIngress.yaml
  ```
  
### Conclusion

**You have successfully cleaned up the Kong Operator resources.**
