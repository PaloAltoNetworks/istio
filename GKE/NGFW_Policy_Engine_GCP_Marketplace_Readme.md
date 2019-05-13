# Overview

The Palo Alto Networks ```NGFW Policy Engine``` security adapter for Istio, gives operators:
- the ability to have full visibility into traffic patterns between microservice workloads
  running within the service mesh
- and the ability to enforce policy on the micro-services workloads running inside the mesh

# Deployment Models for the NGFW Policy Adapter

The NGFW Policy Engine is designed to retrieve security policies from either Panorama or 
a Policy Simulator.

The choice of either using Panorama or the Policy Simulator is specified while providing 
the deployment configuration for the Security Policy Adapter. 

The following figure illustrates the complete deployment options for the Security Policy 
Adapter along with the options for either Panorama or Policy Simulator. 


## Important Sections

- Theory of Operation
- Specification of the Security Policy API Endpoint 
  (Recommended: Panorama; Alternatively: Security Policy Management Service) 
  - Configuring the ```NGFW Policy Engine``` to communicate with Panorama
  - Configuring the ```NGFW Policy Engine``` to communicate with the Security Policy Management Service 

## Theory of Operation 

The ```NGFW Policy Engine``` adapter is designed to retrieve security policies defined 
on the Palo Alto Networks Policy Management device, called Panorama.

Once the policies have been retreived, they are translated from Palo Alto Networks security
policies into policies that can be applied on the Kubernetes and Istio systems. 

## Specification of the Security Policy API Endpoint

### Configure the ```NGFW Policy Engine``` to communicate with Panorama 

TODO

### Configure the ```NGFW Policy Engine``` to communicate with the Security Policy Management Service

(1) Clone the Security Policy Management Service from the git repo 
	- git clone https://github.com/vinayvenkat/pan_istio_policy_simulator.git

(2) Deploy the Palo Alto Networks Policy Management Simulator Service

Execute: cd <path to directory>/pan_istio_policy_simulator

Execute : kubectl create configmap panw-security-policies --from-file sec_policy.json

Execute : kubectl apply -f panw_policy.yaml

Validate that the policy simulator container and service are running
To validate the service execute: kubectl get svc panw-policy You should see the panw-policy service listed.

To validate the deployment of the container execute: kubectl get deployment panw-policy You should see the panw-policy deployment listed and running.

## Important: Extract the podIP of the panw-policy simulator service 

Execute: ``` kubectl get po ``` 

Output should resemble: ```
NAME                           READY   STATUS    RESTARTS   AGE
curl-775f9567b5-8clc8          1/1     Running   1          53m
panw-policy-5ff6c586c7-44bvm   1/1     Running   0          7d
```
Copy the name of the panw-policy container in its entirety. 

Execute: ``` kubectl get panw-policy-5ff6c586c7-44bvm -o yaml | grep podIP ``` 

Output will resemble: ```
	podIP: 10.28.3.59
```

Note: You will need this IP when editing the yaml file pertaining to the Palo Alto Networks Security Adapter deployment config.


