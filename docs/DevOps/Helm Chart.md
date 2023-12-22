
- [[Kubernetes]] is an open-source container orchestration system. The containers that make up an application are grouped into logical units. To set up a single application, you have to create multiple independent resources (pods, services, and deployments). Each requires you to write a YAML manifest file.

- **Helm** is a **K8s** package manager designed to easily package, configure, and deploy applications and services onto **K8s cluster** - it's **homebrew** for **k8s**

# Resource
- Helm Chart resource and actual provisioned resource
	- In Helm Chart, whenever we want to provision (spin up) a pod, we need to tell how much resource that we gonna give to that Pod
	- The way this work is that we have a Node group, which is basically a pool of multiple node with predefined instance class (let’s say t3.medium)
	- Whenever a pod get spun up, K8s will scan through the pool of Node to see if any of the Node has the amount of resource that we request, if there is it just gonna use that Node or if not it will wait for something to free up or just spin up a new Node.
	- This means that the maximum value that we can give to Helm Chart is the maximum value of the predefined instance class (in this case, t3.medium)