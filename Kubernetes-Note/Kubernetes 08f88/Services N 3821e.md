# Services | Network

# Services

---

**Service** is an abstraction level that Kubernetes uses to make a deployed application accessible from the internal and external of the cluster.  A Service in Kubernetes is a REST object, similar to a Pod. Like all of the REST objects, you can `POST`a Service definition to the API server to create a new instance. The name of a Service object must be a valid [`RFC 1035 label name`](https://kubernetes.io/docs/concepts/overview/working-with-objects/names#rfc-1035-label-names)

### **Why Services?**

---

Pods are the most basic entities inside our Kubernetes cluster. We can host our applications inside the pods with the help of containers. A Service inside the Kubernetes cluster acts as a layer above the pods. Communication between different pods in a cluster can be enabled with the help of Services.

Services simply point to pods using labels. Since services are not node-specific, a service can point to a pod regardless of where it runs in the cluster at any given moment in time. By exposing a service IP address as well as a DNS service name, the application can be reached by either method as long as the service exists.

Consider the following example: we created a web application, our application has groups of pods running various sections such as first group for serving front-end, second group for running back-end processes and a third group for connecting to external data source.

![Untitled](Services%20N%203821e/Untitled.png)

Therefore, services enables connectivity between these group of pods. Services helps front-end apps to be made available to end users, facilitates communication between back-end pods and front-end pods and also helps in establishing connectivity to an external data source.

## Service Component

---

Kubernetes services connect a set of pods to an abstracted service name and IP address. Services provide discovery and routing between pods. For example, services connect an application front-end to its backend, each of which running in separate deployments in a cluster. Services use labels and selectors to match pods with other applications. The core attributes of a Kubernetes service are:

- A label selector that locates pods
- The clusterIP IP address and assigned port number
- Port definitions
- Optional mapping of incoming ports to a targetPort

Services can be defined without pod selectors. For example, to point a service to another service in a different namespace or cluster.

### **Types of Services in Kubernetes**

Kubernetes supports different types of services:

- **ClusterIP**. Exposes a service which is only accessible from within the cluster.
- **NodePort**. Exposes a service via a static port on each node’s IP.
- **LoadBalancer**. Exposes the service via the cloud provider’s load balancer.
- **ExternalName**. Maps a service to a predefined externalName field by returning a value for the CNAME record.

### ClusterIP

---

ClusterIP is the default type of service, which is used to expose a service on an IP address internal to the cluster. Access is only permitted from within the cluster.

### NodePort Service

---

NodePorts are open ports on every cluster node. Kubernetes will route traffic that comes into a NodePort to the service, even if the service is not running on that node. NodePort is intended as a foundation for other higher-level methods of ingress such as load balancers and are useful in development.

### External Name Service

---

ExternalName services are similar to other Kubernetes services; however, instead of being accessed via a clusterIP address, it returns a CNAME record with a value that is defined in the externalName: parameter when creating the service.

### LoadBalancer

---

For clusters running on [public cloud](https://www.vmware.com/topics/glossary/content/public-cloud.html)
 providers like AWS or Azure, creating a load LoadBalancer service provides an equivalent to a clusterIP service, extending it to an external load balancer that is specific to the cloud provider. Kubernetes will automatically create the load balancer, provide firewall rules if needed, and populate the service with the external IP address assigned by the cloud provider.

![Untitled](Services%20N%203821e/Untitled%201.png)

however, we couldnt specify directly using LoadBalancer Services on minikube, but we could use use another way called `tunnelling`. Tunnel creates a route to services deployed with type LoadBalancer and sets their Ingress to their ClusterIP.

Services of type `LoadBalancer` can be exposed via the `minikube tunnel` command. It must be run in a separate terminal window to keep the `LoadBalancer` running. Ctrl-C in the terminal can be used to terminate the process at which time the network routes will be cleaned up.

[Accessing apps](https://minikube.sigs.k8s.io/docs/handbook/accessing/)

## Ingress

---

[Ingress](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#ingress-v1-networking-k8s-io) exposes HTTP and HTTPS routes from outside the cluster to [services](https://kubernetes.io/docs/concepts/services-networking/service/) within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

![Untitled](Services%20N%203821e/Untitled%202.png)

[Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

## Access Services

---

There are two ways to discover a Kubernetes service:

**DNS (most common):** The DNS method is the recommended method of discovering services. To use this method, a DNS server must first be installed on the cluster. The DNS server monitors the Kubernetes API, and when a new service is created its name becomes available for easy resolution for requesting applications.

**ENV variable:** This method relies on the kubelet adding environment variables for each active service for every node a pod is running on.

## Deployement vs Services

---

What's the difference between a Service and a Deployment in Kubernetes?

A deployment is responsible for keeping a set of pods running.

A service is responsible for enabling network access to a set of pods.

We could use a deployment without a service to keep a set of identical pods running in the Kubernetes cluster. The deployment could be scaled up and down and pods could be replicated. Each pod could be accessed individually via direct network requests (rather than abstracting them behind a service), but keeping track of this for a lot of pods is difficult.

We could also use a service without a deployment. We'd need to create each pod individually (rather than "all-at-once" like a deployment). Then our service could route network requests to those pods via selecting them based on their labels.

Services and Deployments are different, but they work together nicely.

/

References:

---

[Understanding Services in Kubernetes with Examples!](https://blog.knoldus.com/understanding-services-in-kubernetes-with-examples/)

[Service](https://kubernetes.io/docs/concepts/services-networking/service/)

[Belajar Kubernetes - 29 Mengekspos Service](https://www.youtube.com/watch?v=zbx0AY-gGPw)

[Exposing applications using services | Kubernetes Engine Documentation | Google Cloud](https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps/)