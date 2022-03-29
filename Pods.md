# Pods

### `kubectl commands to check the cluster health`

[Kubectl using command to get cluster status](https://stackoverflow.com/questions/54882727/kubectl-using-command-to-get-cluster-status#:~:text=To%20get%20detailed%20information%20to,componentstatus%20or%20kubectl%20get%20cs)

The below command would display the health of scheduler, controller and `etcd`

The least expensive way to check if you can reach the API server is `kubectl version.`In addition `kubectl cluster-info`
 gives you some more info. another one is `kubectl get componentstatus`. `kubectl get componentstatus`
 depricated

`kubectl get cs`Command below lists Kubernetes core components like, `etcd`, controller, scheduler, kube-proxy, core-dns, network plugin. All those pods should be running to be sure that Kubernetes is healthy. `kubectl get pod -n kube-system`

Finally deploy one front-end and back-end Pod and verify the inter-pod communication to ensure that cluster is up and working correctly.

Below are the commands to get cluster status based on requirements:

- To get information regarding where your Kubernetes master is running at, CoreDNS is running at, kubernetes-dasboard is running at, use `kubectl cluster-info`
- To get detailed information to further debug and diagnose cluster problem, use `kubectl cluster-info dump`
- To get only the health status for your node use, `kubectl get componentstatus` or `kubectl get cs`
- To show detailed information about a resource use `kubectl describe node <node>`

[https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### `What are Pods`

[Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

[Mengatur Pod dan Kontainer](https://kubernetes.io/id/docs/tasks/configure-pod-container/_print/)

[https://cloud.google.com/kubernetes-engine/docs/concepts/pod#:~:text=Pods are the smallest%2C most,and share the Pod's resources](https://cloud.google.com/kubernetes-engine/docs/concepts/pod#:~:text=Pods%20are%20the%20smallest%2C%20most,and%20share%20the%20Pod's%20resources)

***Pods*** are the smallest deployable units of computing that you can create and manage in Kubernetes.

A *Pod* (as in a pod of whales or pea pod) is a group of one or more [containers](https://kubernetes.io/docs/concepts/containers/), with shared storage and network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.

As well as application containers, a Pod can contain [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) that run during Pod startup. You can also inject [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) for debugging if your cluster offers this.

<aside>
🔮 **Note: While Kubernetes supports more [container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes) than just Docker, [Docker](https://www.docker.com/) is the most commonly known runtime, and it helps to describe Pods using some terminology from Docker.**

</aside>

The shared context of a Pod is a set of Linux namespaces, cgroups, and potentially other facets of isolation - the same things that isolate a Docker container. Within a Pod's context, the individual applications may have further sub-isolations applied.

In terms of Docker concepts, a Pod is similar to a group of Docker containers with shared namespaces and shared filesystem volumes.

Pods in a Kubernetes cluster are used in two main ways:

- **Pods that run a single container**. The "one-container-per-Pod" model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container; Kubernetes manages Pods rather than managing the containers directly.
- **Pods that run multiple containers that need to work together**. A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers form a single cohesive unit of service—for example, one container serving data stored in a shared volume to the public, while a separate *sidecar* container refreshes or updates those files. The Pod wraps these containers, storage resources, and an ephemeral network identity together as a single unit.

## Pod lifecycle

[Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

Pods are ephemeral. They are not designed to run forever, and when a Pod is terminated it cannot be brought back. In general, Pods do not disappear until they are deleted by a user or by a controller.

Pods do not "heal" or repair themselves. For example, if a Pod is scheduled on a node which later fails, the Pod is deleted. Similarly, if a Pod is evicted from a node for any reason, the Pod does not replace itself.

Each Pod has a `PodStatus` API object, which is represented by a Pod's `status` field. Pods publish their *phase* to the `status: phase` field. The phase of a Pod is a high-level summary of the Pod in its current state.

When you run `[kubectl get pod](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)` to inspect a Pod running on your cluster, a Pod can be in one of the following possible phases:

- **Pending:** Pod has been created and accepted by the cluster, but one or more of its containers are not yet running. This phase includes time spent being scheduled on a node and downloading images.
- **Running:** Pod has been bound to a node, and all of the containers have been created. At least one container is running, is in the process of starting, or is restarting.
- **Succeeded:** All containers in the Pod have terminated successfully. Terminated Pods do not restart.
- **Failed:** All containers in the Pod have terminated, and at least one container has terminated in failure. A container "fails" if it exits with a non-zero status.
- **Unknown:** The state of the Pod cannot be determined.

Additionally, `PodStatus` contains an array called `PodConditions`, which is represented in the Pod manifest as `conditions`. The field has a `type` and `status` field. `conditions` indicates more specifically the conditions within the Pod that are causing its current status.

### `kubectl explain pod`

rismanda.kusumadewi@BA-00002663 kubernetes-training % kubectl explain pod

KIND:     Pod

VERSION:  v1

**DESCRIPTION:**

Pod is a collection of containers that can run on a host. This resource is

created by clients and scheduled onto hosts.

**FIELDS:**

**apiVersion	<string>**

APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: [https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources) 

**kind	<string>**

Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: [https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds) 

**metadata	<Object>**

Standard object's metadata. More info: [https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata) 

**spec	<Object>**

Specification of the desired behavior of the pod. More info: [https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status) 

**status	<Object>**

Most recently observed status of the pod. This data may not be up to date. Populated by the system. Read-only. More info: [https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status) 

`Creating a Pod using the kubectl command
Getting a Pod
Describing a Pod
Writing a Pod Manifest(Spec)`

The Kubernetes scheduler automatically assigns pods to nodes, however there are times where you need more control over where the scheduler assigns pods. For example, you need to schedule a pod to a certain machine that has an SSD attached to it.

There are different ways to achive this, using: `nodeSelector`, `Affinity` and `Anti-Affinity`, which all use [Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).

In this scenario, we'll be covering how to assign pods to nodes using `nodeSelector`. For further reading, check out [Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/).

# **Init Container**

An [Init Container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) is a container which is executed before the application container is started. `Init-containers` are usually used for deploying utilities or execute scripts which are not loaded and executed in the application container image.