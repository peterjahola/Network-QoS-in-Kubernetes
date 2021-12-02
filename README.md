# Network QoS in Kubernetes
A study about is it worth to use QoS in Kubernetes and if so then examine it in some special cases.

## Introduction
As you might know QoS (Quality of Service) used in plenty of fields but not in the public network because everyone would be able to change their packet priority and also some routers are not supporting QoS. Therefore, if we want to speed up our connection or simply just improve our system, QoS doesn't seems to be a possible solution. However, we can implement it in the sender side and while the interface of a `Node` is overloaded the most important packets can be sent earlier than the ones which are not delay and bandwidth sensitive. In Kubernetes it can help so much because one interface usually handles more containers. This could also help in private networks and maybe standards like TSN would be able to use it as well. There are a lot of interesting usage where Kubernetes and QoS could cause surprising results. The focus in my work is on a basic Pod-Pod communication measured in Minikube and localhost. Results will tell if this idea has any opportunities or the solution for these problems are somewhere else such as in Docker, maybe Kubernetes is optimized and fast enough so it’s not even necessary to deal with it.

## Tools and Environment setup
First of all, I added two deployments, both with the same image from Docker Hub called `retvari/net-debug`, the sender named as `iperf-client` and the receiver as `net-debug`. It has a lot of useful tools for tests like this, here `tcpdump` and `iperf` was the most helpful. To make the traffic priority based one of the easiest ways is to apply `tc-pfifo_fast`. This is a basic utility which queues the packets in three different `pfifo`. The queuing is based on what TOS (Type of Service) value has the packet. While a lower band has traffic higher bands are never dequeued. Net debug Pod configuration file should be changed to use `tc` on it.

## The Measurement
After the setup I started an `iperf` server on the receiver side. Before the first measure I checked with `tcpdump` if packets successfully got the TOS value. Overall, three type of TOS value have been used: 0x10 for the highest priority, 0x8 for the lower priority and 0x0 when no priority has given. To simulate real circumstances there was a “background” traffic and through the different measures I gave different type of priority to it, sometimes higher than the main traffic. Parameter of the `iperf` were these:
```bash
kubectl exec -it <iperf-client-pod-name> -- iperf -c <net-debug-pod-ip> -u -b 100g -M 100 -t <time in sec to transmit for> -S <tos value>
```
As you could see, the protocol was UDP. For the main traffic I never set a `-t` parameter which means that it was always lasts ten seconds while I set the background traffic for fifteen seconds so I could comfortably start the main traffic while the background was running. Results will contain the different type of QoS, Minikube and localhost combinations I used.

## Results

## Conclusion
