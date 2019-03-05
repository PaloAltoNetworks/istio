# istio
This repository contains the various integrations of the Palo Alto Networks Security adapter for Istio.

# Why is this important? 

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
