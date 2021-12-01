# Network QoS in Kubernetes
A study about is it worth to use QoS in Kubernetes and if so then examine it in some special cases.
## Introduction
As you might know QoS used in plenty of fields but not in the public network because everyone would be able to change their packet priority and also some routers are not supporting QoS. Therefore, if we want to speed up our connection or simply just improve our system, QoS doesn't seems to be a possible solution however, we can implement it in the sender and receiver side where it can lead to a faster process. This could also help in private networks and maybe standards like TSN would be able to use it as well. There are a lot of interesting usage where Kubernetes and QoS could cause surprising results. The focus in my work is on a basic Pod-Pod communication measured in Minikube and localhost. Results will tell if this idea has really huge opportunities or the solution for these problems are somewhere else such as in Docker, maybe Kubernetes is optimized and fast enough so it’s not even necessary to deal with it.
## Tools and Eniroment setup
First of all I added two deployments, one of them was 

## The Measurement

## Results

## Conclusion
