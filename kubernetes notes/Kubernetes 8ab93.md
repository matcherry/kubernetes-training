# Kubernetes (k8s)

course :

[Learn Kubernetes using Interactive Browser-Based Labs | Katacoda](https://www.katacoda.com/courses/kubernetes/)

[Play with Kubernetes](https://labs.play-with-k8s.com/)

[Học kubenetes](https://thanhtungvo.medium.com/h%E1%BB%8Dc-kubenetes-694024d1e5e1)

[GitHub - microsoft/kubernetes-learning-path: https://azure.microsoft.com/en-us/resources/kubernetes-learning-path/](https://github.com/microsoft/kubernetes-learning-path)

[Kubernetes: Everything You Need to Know](https://www.appvia.io/blog/what-is-kubernetes)

[How To Use Kubernetes and Minikube to Set up a Relational Database Locally](https://levelup.gitconnected.com/how-to-use-kubernetes-and-minikube-to-set-up-a-relational-database-locally-1ea04ba6cb7f)

[GitHub - prabhatsharma/kubernetes-the-hard-way-aws: AWS version of Kelsey's kubernetes-the-hard-way](https://github.com/prabhatsharma/kubernetes-the-hard-way-aws)

[10+ Useful Kubernetes Tools](https://faun.pub/10-useful-kubernetes-tools-ddffa62089cc)

# Introduction

---

[A 1000 Foot Overview of Kubernetes](https://levelup.gitconnected.com/a-1000-foot-overview-of-kubernetes-c00e410b1983)

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.

The name Kubernetes originates from Greek, meaning helmsman or pilot. K8s as an abbreviation results from counting the eight letters between the "K" and the "s". Google open-sourced the Kubernetes project in 2014. Kubernetes combines [over 15 years of Google's experience](https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/) running production workloads at scale with best-of-breed ideas and practices from the community.

[https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

[Kubernetes - kubectl Overview](https://jamesdefabia.github.io/docs/user-guide/kubectl-overview/)

## Kubernetes Benefit

---

Kubernetes provides you with:

- **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- **Storage orchestration** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- **Self-healing** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

## Kubernetes Component

---

When you deploy Kubernetes, you get a cluster.

A Kubernetes cluster consists of a set of worker machines, called [nodes](https://kubernetes.io/docs/concepts/architecture/nodes/), that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) that are the components of the application workload. The [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

![Untitled](Kubernetes%208ab93/Untitled.png)

[https://kubernetes.io/docs/concepts/overview/components/](https://kubernetes.io/docs/concepts/overview/components/)

# Kubernetes API

---

The core of Kubernetes' [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) is the [API server](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver). The API server exposes an HTTP API that lets end users, different parts of your cluster, and external components communicate with one another.

The Kubernetes API lets you query and manipulate the state of API objects in Kubernetes (for example: Pods, Namespaces, ConfigMaps, and Events).

Most operations can be performed through the [kubectl](https://kubernetes.io/docs/reference/kubectl/) command-line interface or other command-line tools, such as [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/), which in turn use the API. However, you can also access the API directly using REST calls.

Consider using one of the [client libraries](https://kubernetes.io/docs/reference/using-api/client-libraries/) if you are writing an application using the Kubernetes API.

[https://kubernetes.io/docs/concepts/overview/kubernetes-api/](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)

# ****Working with Kubernetes Objects****

---

*Kubernetes objects* are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster. Specifically, they can describe:

- What containerized applications are running (and on which nodes)
- The resources available to those applications
- The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance

# Kubernetes Architecture : Made up of

---

[https://collabnix.github.io/kubelabs/Kubernetes_Architecture.html](https://collabnix.github.io/kubelabs/Kubernetes_Architecture.html)

[https://collabnix.github.io/kubelabs/](https://collabnix.github.io/kubelabs/)

# Reources

---

[https://www.youtube.com/watch?v=xjpHggHKm78](https://www.youtube.com/watch?v=xjpHggHKm78)

[Kubernetes requests vs limits: Why adding them to your Pods and Namespaces matters | Google Cloud Blog](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-resource-requests-and-limits)

A resource is **an endpoint in the Kubernetes API that stores a collection of API objects of a certain kind**; for example, the built-in pods resource contains a collection of Pod objects. A custom resource is an extension of the Kubernetes API that is not necessarily available in a default Kubernetes installation.

When you are working with Kubernetes, and want to list down all the resources(Kubernetes objects) associated to a specific namespace, you can either **use individual kubectl get command to list down each resource one by one**, or you can list down all the resources in a Kubernetes namespace by running a single command.

How much resources does Kubernetes?
Resource limit means **the maximum combined value of all resources an individual can have an ownership interest in and still qualify for Medicaid**

They are requests and limits. Requests define the minimum amount of resources that containers need. If you think that your app requires at least 256MB of memory to operate, this is the request value. The application can use more than 256MB, but **Kubernetes guarantees a minimum of 256MB to the container**.

We can find the available resources at cluster level using some of this command

There are couple of ways to achieve this. You didn't mention what environment are you using, however you probably already have `[metric server](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/#metrics-server)` in your cluster.

**1. `Top` command**

`kubectl top pods` or `kubectl top nodes`. This way you will be able to check current usage of pods/nodes. You can also narrow it to `namespace`.

**2. Describe node**

If you will execute `kubectl describe node`, in output you will be able to see Capacity of that node and how much allocated resources left. Similar with `Pods`.

```
...
Capacity:
 attachable-volumes-gce-pd:  127
 cpu:                        1
 ephemeral-storage:          98868448Ki
 hugepages-2Mi:              0
 memory:                     3786684Ki
 pods:                       110
Allocatable:
 attachable-volumes-gce-pd:  127
 cpu:                        940m
 ephemeral-storage:          47093746742
 hugepages-2Mi:              0
 memory:                     2701244Ki
 pods:                       110
 ...

```

**3. Prometheus**

If you need more detailed information with statistics, I would recommend you to use `[Prometheus](https://prometheus.io/)`. It will allow you to create statistics of nodes/pods, generate alerts and many more. It also might provide metrics not only CPU and Memory but also `custom.metrics` which can create statistics of all `Kubernetes` objects.

Many useful information can be found [here](https://www.replex.io/blog/kubernetes-in-production-the-ultimate-guide-to-monitoring-resource-metrics).

[https://enterprisersproject.com/article/2020/8/managing-kubernetes-resources-5-things-remember](https://enterprisersproject.com/article/2020/8/managing-kubernetes-resources-5-things-remember)

# Kubernetes Best Practice

---

[Kubernetes Best Practices](https://www.youtube.com/playlist?list=PLIivdWyY5sqL3xfXz5xJvwzFW_tlQB_GB)

[Configuration Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)