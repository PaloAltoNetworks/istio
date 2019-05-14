# Overview

The Palo Alto Networks ```NGFW Policy Engine``` security adapter for Istio, gives operators:
- the ability to have full visibility into traffic patterns between microservice workloads
  running within the service mesh
- and the ability to enforce policy on the micro-services workloads running inside the mesh

# Detailed steps to deploy the NGFW Policy Engine onto a Kubernetes cluster from the GCP Market place

The following figure shows the user interface associated with deploying 
the adapter from the Google Marketplace. 

![Alt text](https://github.com/PaloAltoNetworks/istio/blob/master/GKE/NGFW-Policy-inteface.png "Deploy NGFW Policy Engine from the Google marketplace")

## Important Note: There are two options with regard to specifying the Security Policy Endpoint

The following sections discuss the options as well the the steps to configure and deploy the security policy 
adapter along with the security policy endpoint.

# Deployment Models for the NGFW Policy Adapter

The NGFW Policy Engine is designed to retrieve security policies from **either** Panorama **or**
a Policy Simulator.

The choice of either using Panorama or the Policy Simulator is specified while providing 
the deployment configuration for the Security Policy Adapter (as shown in the figure above). 

The following figure illustrates the complete deployment options for the Security Policy 
Adapter along with the options for either Panorama or Policy Simulator. 

![Alt text](https://github.com/PaloAltoNetworks/istio/blob/master/GKE/NGFW-Policy-deployment.png  "NGFW Policy Adapter Deployment Options")

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

1. Specify the type of the policy endpoint to be "panorama" 
2. Obtain the public IP of the Panorama device.  
3. Specify the port number of the policy endpoint as: "443"
4. Input the username of the Panorama device in base64 encoding: echo "<username>" | base64
   - Copy and paste the output into the "username for Panorama" text input field.
5. Input the password of the Panorama device in base64 encoding: echo "<password> " | base64 
   - Copy and paste the output into the "password for Panorama" text input field.

Once the fields have been input, press the "Deploy" button at the bottom of the page. 
This will then deploy the NGFW Policy Engine onto the selected kubernetes cluster. 

### Configure the ```NGFW Policy Engine``` to communicate with the Security Policy Management Service

The deployment and configuration of the Security Policy Simulator Management service is a 2-step process. 

#### Step 1 Deploy the Simulator Service onto the Kubernetes cluster

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

#### Step 2 Input the Security Policy Simulator service information into the market place deployment fields. 

1. In the "Specify the type of the Policy endpoint" field: Enter "simulator"

2. In the "Specify the IP Address or the DNS name of the security policy endpoint" field, input the 
   IP address retrieved from the step above. This will be the value of the podIP field as shown in the example above. 

3. In the "Specify the port number on which the security policy API endpoint is accessed on" input: 9080 

4. There is currently no authentication associated with the use with the policy simulator. 
   Hence leave the username and password fields empty. 

Once the fields have been input, press the "Deploy" button at the bottom of the page. 
This will then deploy the NGFW Policy Engine onto the selected kubernetes cluster. 
