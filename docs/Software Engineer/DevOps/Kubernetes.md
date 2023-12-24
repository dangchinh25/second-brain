
1. The outermost layer of Kubernetes(K8s) is **K8s cluster**, which is an abstraction that coordinates and manage all of our deployed application
	- The **k8s cluster** has 2 elements
		- **Control plane**: responsible for managing the cluster
		- **Nodes**: A Vm of an actual physical machine managed by K8s control plane
> The Control Plane coordinates all activities in your cluster, such as scheduling applications, maintaining applications' desired state, scaling applications, and rolling out new updates.

2. To deploy a containerized application to a **k8s cluster**
	- Create a **k8s deployment**
	- The **Deployment** instructs **k8s** how to create and update instances of your application. Once you've created a **Deployment**, the **control plane** schedules the application instances included in that **Deployment** to run on individual **Nodes** in the cluster.
	- Once the application instances are created, a Kubernetes Deployment Controller continuously monitors those instances. If the Node hosting an instance goes down or is deleted, the Deployment controller replaces the instance with an instance on another Node in the cluster.

3. A **Deployment** will create **Pod** with the containerized application code inside it
	- A **Pod** is an abstraction to host the application represent of >=1  application container
	- We have to specify the Docker images that **Deployment** uses to create **Pod**
	- A **Pod** may have multiple container
	- Everything in a **Pod** shares storages, IP address, etc
	- A **Pod** models an application specific "logical host"
	- A **Pod** always live on a **Node**
	- **Deployment** may create **Pods** in different **Nodes** depends on which is available

4. **Service**: To expose a **Pod/ a set of Pods** to the public so that it can receives traffic from the outside world

5. **Scale**
	- **Deployment** only spin 1 **pod** by default
	- Can config to increase the number of **Replica**
	- We can use an integrated **Load Balancer** service to distribute the traffic evenly accross **Pods**


- To create a Cronjob, just need to write a K8s yaml configuration with Kind: Cronjob