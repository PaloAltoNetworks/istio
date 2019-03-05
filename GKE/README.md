# Installing and Running the Palo Alto Networks Security Adapter for Istio 

## Overview
Microservices applications are deployed on container orchestration platforms such as Kubernetes in large numbers. Consequently, 
there is a lot data traffic between micro-services as well as from the internet and to the internet. Operators have found that 
it becomes difficult to keep track and reason about all this traffic as the number of services keeps increasing. 

## Problem

The explosion of the number of services continues, there is a need to be able to have a record of all the container 
traffic patterns. This becomes even more important as containers are highly ephemeral, which means they are in 
existence for a very short period of time. Hence, if and when exploits happen, it becomes very hard to know the 
circumstances around the exploit / vulnerability. 

## Solution 

The solution to this problem, especially for security teams is to have a very light weight footprint 
which will allow them to have a perfect record of all containers that have existed in the cluster at any point,
along with all the traffic access patters of these containers. 

## Palo Alto Security adapter for Istio 

The Palo Alto Security adapter for Istio generically gives operators, especially security teams the ability 
to have access to this perfect record of container activity. It provides information such as:

  - container name
  - destination IP
  - destination service 
  - protocol 
  - request path or URL 
  - method 
  - requestSize 
  - destinationUid 
  - responseCode 
  - latency 
  - responseSize 
  - Timestamp
  - among others

All data output by the Palo Alto Networks adapter will be prefixed by a custom `prefix` which 
can be specified by the user. This allows for the easy searching of pertinent information. 

### Use Cases

- full visibility into container activity from a network perspective for container 
  workloads running on Kubernetes environments with Istio.
- Ability to use the logs and information to perform forensics to understand 
  exploits and vulnerabilities.


## Installation and setup 

#### kubectl 

Please ensure that the `kubectl` tool in installed and available on your system. 

Instructions to install kubectl can be found at: ```https://kubernetes.io/docs/tasks/tools/install-kubectl/```

#### Install gcloud skd and tools 

Instructions to install the Google cloud SKD can be found at: ```https://cloud.google.com/sdk/install```


#### Download the kubernetes (GKE) credentials to your local system. 

 - You will need the GKE cluster name, the zone name and the project id 
 - Execute ```gcloud container clusters get-credentials <cluster name> --zone <zone name> --project <project name>

#### Create a kubernetes cluster role binding 

Execute the following command:

```kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
```

#### Ensure Stackdriver Logging is enabled for the cluster

This deployement is only applicable on GKE. 

Please follow the instructions at the following to enable stack driver logging:
```https://cloud.google.com/monitoring/kubernetes-engine/legacy-stackdriver/logging```

#### Download the Istio code base if you don't have Istio already installed 

Instructions can be found at: ```https://istio.io/docs/setup/kubernetes/download-release/```

#### Setup and deploy the Istio Custom Resource Definitions 

   Note: the following paths are dependent the `pwd` directory being the directory 
         into which Istio has been deployed into. 
		 For example: ```$PWD/istio-1.0.11/```

   Execute the following command: 
   ``` kubectl apply -f install/kubernetes/istio-demo-auth.yaml ```


#### Verify the Istio installation 

 - Ensure that Istio and all its components have been successfully installed. 
 - Execute: ```kubectl get po -n istio-system```

   The output should resemble: 
   ```
	vinay@vv15-ubuntu18:~/go/src/$ kubectl get po -n istio-system
		NAME                                      READY   STATUS      RESTARTS   AGE
		grafana-9cfc9d4c9-dqp9f                   1/1     Running     0          20d
		istio-citadel-74df865579-9qd2s            1/1     Running     0          20d
		istio-cleanup-secrets-mjlhq               0/1     Completed   0          20d
		istio-egressgateway-96fb7964f-jdn6x       1/1     Running     0          20d
		istio-galley-8487989b9b-zhclr             1/1     Running     0          20d
		istio-grafana-post-install-2p6hw          0/1     Completed   0          20d
		istio-ingressgateway-75d6486b96-q762r     1/1     Running     0          20d
		istio-pilot-78776dd4bd-tkpsn              2/2     Running     0          20d
		istio-policy-67f87dd-zjnvp                2/2     Running     0          20d
		istio-security-post-install-j9wzc         0/1     Completed   0          20d
		istio-sidecar-injector-5cfcf6dd86-7bfd6   1/1     Running     0          20d
		istio-telemetry-9b566b9c7-c9phj           2/2     Running     0          20d
		istio-tracing-ff94688bb-5thvh             1/1     Running     0          20d
		prometheus-f556886b8-754zr                1/1     Running     0          20d
		servicegraph-55d57f69f5-k7vt2             1/1     Running     0          20d
   ```

## Install the Palo Alto Networks Security Adapter

Note: This assumes that you have checked out the `github.com/PaloAltoNetworks/istio` 
      repo.
      Your working directory = $PWD/istio/GKE/

Execute: ```kubectl apply -f operatorconfig```

This will install all the components required to run the Palo Alto Networks 
Security adapter on the kubernetes cluster. 

### Verify the installation of the Palo Alto Networks Adapter 


   The output should resemble: 
   ``` 
	vinay@vv15-ubuntu18:~/go/src/$ kubectl get po -n istio-system
		NAME                                      READY   STATUS      RESTARTS   AGE
		grafana-9cfc9d4c9-dqp9f                   1/1     Running     0          20d
		istio-citadel-74df865579-9qd2s            1/1     Running     0          20d
		istio-cleanup-secrets-mjlhq               0/1     Completed   0          20d
		istio-egressgateway-96fb7964f-jdn6x       1/1     Running     0          20d
		istio-galley-8487989b9b-zhclr             1/1     Running     0          20d
		istio-grafana-post-install-2p6hw          0/1     Completed   0          20d
		istio-ingressgateway-75d6486b96-q762r     1/1     Running     0          20d
		istio-pilot-78776dd4bd-tkpsn              2/2     Running     0          20d
		istio-policy-67f87dd-zjnvp                2/2     Running     0          20d
		istio-security-post-install-j9wzc         0/1     Completed   0          20d
		istio-sidecar-injector-5cfcf6dd86-7bfd6   1/1     Running     0          20d
		istio-telemetry-9b566b9c7-c9phj           2/2     Running     0          20d
		istio-tracing-ff94688bb-5thvh             1/1     Running     0          20d
		panstackdriver-6789d7d88f-49tmk           1/1     Running     0          24s
		prometheus-f556886b8-754zr                1/1     Running     0          20d
		servicegraph-55d57f69f5-k7vt2             1/1     Running     0          20d
``` 

You will see the Palo Alto Networks security adapter's container. In this case it is: 
`panstackdriver-6789d7d88f-49tmk`


## Run some traffic to any applications deployed on your cluster


## View the logs on the Stackdriver Console on GCP

- Login to the GCP console
- Navigate to Stackdriver logging tab on the left 
- Filter to the following logs: `GKE Container -> <cluster name> -> istio-system (namespace)` 

You should be able to see the logs pertaining to intercontainer traffic on your kubernetes cluster
running with the Istio Service Mesh.  


