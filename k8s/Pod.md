هر پاد یک محیط اجرایی اشتراکی (shared) برای اجرای کانیتر ها می باشد که موارد اشتراکی شامل موارد زیر هست:
* network stack
* volums
* shared memory
* IP address

## Pods as the unit of scaling
Pods are the minimum unit of scheduling in Kubernetes. As such, scaling an application
up adds more Pods and scaling it down deletes Pods. 

Pods are immutable. This means you never change them once they’re running.

You also learned about Pods, Deployments, and Services. Pods allow containers and
other workloads to run on Kubernetes. Deployments add self-healing, scaling, and
rollouts. Services add reliable networking and basic load-balancing.