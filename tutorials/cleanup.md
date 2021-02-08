---
title: Jaeger Operator cleanup Tutorial
description: This tutorial explains how to cleanup Operator
---


### Cleaning Up Operator



***Delete the operator's custom resource by kubectl delete commands :***

Example:
 
 ```execute
 kubectl delete -f kongInstance.yaml
 kubectl delete -f https://bit.ly/echo-service
 kubectl delete -f kongIngress.yaml 
 ```



***Delete the operator by kubectl delete command:***
 
 
 Example:
 
 ```execute
 kubectl delete -f https://operatorhub.io/install/kong.yaml
 ```
 

 
***Delete all the yaml files:***
 
 Example:
 
  ```execute
  rm -rf kongInstance.yaml
  rm -rf kongIngress.yaml
  ```
  

