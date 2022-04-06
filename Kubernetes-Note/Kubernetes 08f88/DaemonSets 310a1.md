# DaemonSets | StatefulSets

# DaemonSets

---

A Kubernetes DaemonSet is a container tool that ensures that all nodes are running exactly one copy of a pod. DaemonSets will even create pods on new nodes that are added to your cluster! This essentially runs a copy of the desired pod across all nodes.

A *DaemonSet* ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node monitoring daemon on every node

In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types.

![Untitled](DaemonSets%20310a1/Untitled.png)

## **Communicating with Daemon Pods**

---

Some possible patterns for communicating with Pods in a DaemonSet are:

- **Push**: Pods in the DaemonSet are configured to send updates to another service, such as a stats database. They do not have clients.
- **NodeIP and Known Port**: Pods in the DaemonSet can use a `hostPort`, so that the pods are reachable via the node IPs. Clients know the list of node IPs somehow, and know the port by convention.
- **DNS**: Create a [headless service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services) with the same pod selector, and then discover DaemonSets using the `endpoints` resource or retrieve multiple A records from DNS.
- **Service**: Create a service with the same Pod selector, and use the service to reach a daemon on a random node. (No way to reach specific node.)

Refernces: 

[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

[https://blog.knoldus.com/daemonset-in-kubernetes/](https://blog.knoldus.com/daemonset-in-kubernetes/)

[Handling PersistentVolumeClaim in DaemonSet](https://stackoverflow.com/questions/55165517/handling-persistentvolumeclaim-in-daemonset)

# StatefulSets

---

### **Stateless and Stateful application**

the term “stateless” means that no past data nor state is stored or needs to be persistent when a new container is created. Stateless applications tend to include containerized microservices apps or any short-term workers, whereas Stateful applications typically involve some database, such as Cassandra, MongoDB, or MySQL, and process a read and/or write to it. the server must access and hold onto state information generated during the processing of the earlier request, which is why it is called stateful.

## StatefulSets

---

StatefulSet is a Kubernetes resource used to manage stateful applications. It manages the deployment and scaling of a set of Pods and provides guarantees about the ordering and uniqueness of these Pods. StatefulSet is also a Controller but unlike deployments, it doesn’t create ReplicaSet rather it creates the [Pod](https://blog.knoldus.com/pods-lifecycle/) with a unique naming convention.

for e.g. If you create a StatefulSet with name **counter,** it will create a pod with name **counter-0, counter-1**, **counter-2, etc**

Every replica of a stateful set will have its own state, and each of the pods will be creating its own PVC. So a statefulset with 3 replicas will create 3 pods, each having its own Volume, i.e 3 PVCs.

![Untitled](DaemonSets%20310a1/Untitled%201.png)

### **Statefulset Component in kubernetes**

The following are the major components of *StatefulSet*s :

- StatefulSet
- PersistentVolume
- Headless Service

## **Headless Services**

---

Sometimes you don't need load-balancing and a single Service IP. In this case, you can create what are termed "headless" Services, by explicitly specifying `"None"` for the cluster IP (`.spec.clusterIP`).

You can use a headless Service to interface with other service discovery mechanisms, without being tied to Kubernetes' implementation.

For headless `Services`, a cluster IP is not allocated, kube-proxy does not handle these Services, and there is no load balancing or proxying done by the platform for them. How DNS is automatically configured depends on whether the Service has selectors defined:

### **With selectors**

For headless Services that define selectors, the endpoints controller creates `Endpoints` records in the API, and modifies the DNS configuration to return A records (IP addresses) that point directly to the `Pods` backing the `Service`.

### **Without selectors**

For headless Services that do not define selectors, the endpoints controller does not create `Endpoints` records. However, the DNS system looks for and configures either:

- CNAME records for `[ExternalName](https://kubernetes.io/docs/concepts/services-networking/service/#externalname)`type Services.
- A records for any `Endpoints` that share a name with the Service, for all other types.

## **Key differences**

Here are some main differences between Deployments and StatefulSets:

- Deployments are used for stateless applications whereas StatefulSets for stateful applications
- The pods in a deployment are interchangeable whereas the pods in a StatefulSet are not.
- In statefulset pod`s names are in sequential order on the other hand in deployment pod`s names are unique.
- A headless service handles the pods’ network ID in StatefulSets, while deployments require a service to enable interaction with pods
- In a deployment, the replicas all share a volume and PVC, while in a StatefulSet each pod has its own volume and PVC.

References:

[examples/staging at master · kubernetes/examples](https://github.com/kubernetes/examples/tree/master/staging)

[What is Statefulset and how is it different from Deployment?](https://collabnix.github.io/kubelabs/StatefulSets101/)

[kubelabs/web_stateful.yaml at master · collabnix/kubelabs](https://github.com/collabnix/kubelabs/blob/master/StatefulSets101/web_stateful.yaml)

[StatefulSet Basics](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/)

[What is Statefulset and how is it different from Deployment?](https://collabnix.github.io/kubelabs/StatefulSets101/)