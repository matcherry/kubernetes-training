# Volumes

On-disk files in a container are ephemeral, which presents some problems for non-trivial applications when running in containers. First, when a container crashes kubelet will restart it, but the files will be lost - the container starts with a clean state. Second, when running containers together in a `Pod` it is often necessary to share files between those containers. The Kubernetes `Volume`abstraction solves both of these problems.

<aside>
🔊 Volumes in a simple way is a directory that could accessed by container in pod.

</aside>

A Kubernetes volume is essentially a directory accessible to all containers running in a pod. In contrast to the container-local filesystem, the data in volumes is preserved across container restarts. The medium backing a volume and its contents are determined by the volume type:

- node-local types such as `emptyDir` or `hostPath`
- file-sharing types such as `nfs`
- cloud provider-specific types like `awsElasticBlockStore`, `azureDisk`, or `gcePersistentDisk`
- distributed file system types, for example `glusterfs` or `cephfs`
- special-purpose types like `secret`, `gitRepo`

### Types of Kubernetes Volume

---

Here is a list of some popular Kubernetes Volumes −

- **emptyDir** − It is a type of volume that is created when a Pod is first assigned to a Node. It remains active as long as the Pod is running on that node. The volume is initially empty and the containers in the pod can read and write the files in the emptyDir volume. Once the Pod is removed from the node, the data in the emptyDir is erased.
- **hostPath** − This type of volume mounts a file or directory from the host node’s filesystem into your pod.
- **gcePersistentDisk** − This type of volume mounts a Google Compute Engine (GCE) Persistent Disk into your Pod. The data in a **gcePersistentDisk** remains intact when the Pod is removed from the node.
- **awsElasticBlockStore** − This type of volume mounts an Amazon Web Services (AWS) Elastic Block Store into your Pod. Just like **gcePersistentDisk**, the data in an **awsElasticBlockStore** remains intact when the Pod is removed from the node.
- **nfs** − An **nfs** volume allows an existing NFS (Network File System) to be mounted into your pod. The data in an **nfs** volume is not erased when the Pod is removed from the node. The volume is only unmounted.
- **iscsi** − An **iscsi** volume allows an existing iSCSI (SCSI over IP) volume to be mounted into your pod.
- **flocker** − It is an open-source clustered container data volume manager. It is used for managing data volumes. A **flocker** volume allows a Flocker dataset to be mounted into a pod. If the dataset does not exist in Flocker, then you first need to create it by using the Flocker API.
- **glusterfs** − Glusterfs is an open-source networked filesystem. A glusterfs volume allows a glusterfs volume to be mounted into your pod.
- **rbd** − RBD stands for Rados Block Device. An **rbd** volume allows a Rados Block Device volume to be mounted into your pod. Data remains preserved after the Pod is removed from the node.
- **cephfs** − A **cephfs** volume allows an existing CephFS volume to be mounted into your pod. Data remains intact after the Pod is removed from the node.
- **gitRepo** − A **gitRepo** volume mounts an empty directory and clones a **git** repository into it for your pod to use.
- **secret** − A **secret** volume is used to pass sensitive information, such as passwords, to pods.
- **persistentVolumeClaim** − A **persistentVolumeClaim** volume is used to mount a PersistentVolume into a pod. PersistentVolumes are a way for users to “claim” durable storage (such as a GCE PersistentDisk or an iSCSI volume) without knowing the details of the particular cloud environment.
- **downwardAPI** − A **downwardAPI** volume is used to make downward API data available to applications. It mounts a directory and writes the requested data in plain text files.
- **azureDiskVolume** − An **AzureDiskVolume** is used to mount a Microsoft Azure Data Disk into a Pod.

## Presistent Volumes

---

**Persistent Volume (PV)** − It’s a piece of network storage that has been provisioned by the administrator. It’s a resource in the cluster which is independent of any individual pod that uses the PV. 

Kubernetes persistent volumes are user-provisioned storage volumes assigned to a Kubernetes cluster. Persistent volumes’ life-cycle is independent from any pod using it. Thus, persistent volumes are perfect for use cases in which you need to retain data regardless of the unpredictable life process of Kubernetes pods.

Without persistent volumes, maintaining services as common as a database would be impossible. Whenever a pod gets replaced, the data gained during the life-cycle of that pod would be lost. However, thanks to persistent volumes, data is contained in a consistent state.

[Kubernetes Persistent Volumes: Examples & Best Practices](https://loft.sh/blog/kubernetes-persistent-volumes-examples-and-best-practices/)

## Presistent Volumes Claim

---

**Persistent Volume Claim (PVC)** − The storage requested by Kubernetes for its pods is known as PVC. The user does not need to know the underlying provisioning. The claims must be created in the same namespace where the pod is created.

While `PersistentVolumeClaims`allow a user to consume abstract storage resources, it is common that users need `PersistentVolumes`with varying properties, such as performance, for different problems. Cluster administrators need to be able to offer a variety of `PersistentVolumes`that differ in more ways than just size and access modes, without exposing users to the details of how those volumes are implemented. For these needs there is the `StorageClass` resource.

![Untitled](Volumes%20c09e3/Untitled.png)

<aside>
🔊 PVC and pods must be in the same namespace

</aside>

## StorageClasses

---

Storage classes are Kubernetes objects that let the users specify which type of storage they need from the cloud provider. Different storage classes represent various service quality, such as disk latency and throughput, and are selected depending on the scenario they are used for and the cloud provider’s support. Persistent Volumes and Persistent Volume Claims use Storage Classes.

A `StorageClass`provides a way for administrators to describe the "classes" of storage they offer. Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators. Kubernetes itself is unopinionated about what classes represent. This concept is sometimes called "profiles" in other storage systems.

## ConfigMap and Secret

---

**Kubernetes Secrets and ConfigMaps separate the configuration of individual container instances from the container image, reducing overhead and adding flexibility.** 

[An Introduction to Kubernetes Secrets and ConfigMaps](https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps)

# **Secrets**

---

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) specification or in a [container image](https://kubernetes.io/docs/reference/glossary/?all=true#term-image). Using a Secret means that you don't need to include confidential data in your application code.

Because Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. Kubernetes, and applications that run in your cluster, can also take additional precautions with Secrets, such as avoiding writing secret data to nonvolatile storage.

Secrets are similar to [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) but are specifically intended to hold confidential data.

There are **two levels** involved in working with secrets. First, to **create** the secret, and second, to **introduce** it into the pod. Putting confidential data in secret is safer and adaptable rather than putting it in a container image or a Pod definition.

To use a secret, a pod has to reference the secret. There are three ways to use a secret with a Pod:

- As a file in a volume mounted on one or more of its containers.
- As a container environment variable.
- kubelet uses secrets by pulling images for the Pod.

### **Built-in Secrets**

Built-in Secrets is where **Service accounts** automatically create and Attach Secrets with API credentials. K8s automatically creates secrets that include credentials for reaching the API and automatically alters your pods to use this type of secret. The automatic and also the use of API credentials can be **overridden** or **disabled** if wanted.

### **Creating a Secret**

There are **several** options to create a Secret:

- Create Secret using kubectl command
- We can create a Secret from the config file
- Create Secret using customise

[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

## ConfigMap

---

A ConfigMap is an API object used to store non-confidential data in key-value pairs. [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a [volume](https://kubernetes.io/docs/concepts/storage/volumes/).

A ConfigMap allows you to decouple environment-specific configuration from your [container images](https://kubernetes.io/docs/reference/glossary/?all=true#term-image), so that your applications are easily portable.

> **Caution: ConfigMap does not provide secrecy or encryption. If the data you want to store are confidential, use a [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) rather than a ConfigMap, or use additional (third party) tools to keep your data private.**
> 

ConfigMaps are used to provide configuration data in key-value pairs in K8s. There are two phases involved in configuring ConfigMaps. The first is to create the ConfigMaps, and the second to inject them into the pod.

There are two ways of **creating** a config map:

**i)** The imperative way – without using a ConfigMap definition file

**ii)** Declarative way by using a Configmap definition file.

![Untitled](Volumes%20c09e3/Untitled%201.png)

### **Using ConfigMaps**

---

ConfigMaps can be mounted as **data volumes**. Other parts of the system can also use ConfigMaps without being directly exposed to the Pod. For example, ConfigMaps can hold data that different system parts should use for configuration. The usual way of using ConfigMaps is to **configure environments** for containers running in a Pod in the same namespace. You can also use ConfigMap separately.

References:

[Kubernetes Configmaps | Secrets | Create and Update](https://k21academy.com/docker-kubernetes/configmaps-secrets/)

[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)

[How to use ConfigMaps and Secrets in Kubernetes?](https://blog.knoldus.com/how-to-use-configmaps-and-secrets-in-kubernetes/)

## Summary

---

- **volumeMounts:** → This is the path in the container on which the mounting will take place.
- **Volume:** → This definition defines the volume definition that we are going to claim.
- **persistentVolumeClaim:** → Under this, we define the volume name which we are going to use in the defined pod.

References :

---

[Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

[Kubernetes - Volumes](https://www.tutorialspoint.com/kubernetes/kubernetes_volumes.htm)

[Persistent volumes - Unofficial Kubernetes](https://unofficial-kubernetes.readthedocs.io/en/latest/concepts/storage/persistent-volumes/)

[Kubernetes Volumes explained | Persistent Volume, Persistent Volume Claim & Storage Class](https://www.youtube.com/watch?v=0swOh5C3OVM)

[Kubernetes - Storage Overview - PV, PVC and Storage Class](https://medium.com/devops-mojo/kubernetes-storage-options-overview-persistent-volumes-pv-claims-pvc-and-storageclass-sc-k8s-storage-df71ca0fccc3)

[Configure a Pod to Use a PersistentVolume for Storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

[Kubernetes Persistent Volumes, Claims, Storage Classes, and More](https://cloud.netapp.com/blog/kubernetes-persistent-storage-why-where-and-how)

[Kubernetes ConfigMaps and Secrets](https://medium.com/google-cloud/kubernetes-configmaps-and-secrets-68d061f7ab5b)

[An Introduction to Kubernetes Secrets and ConfigMaps](https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps)

[Kubernetes storage basics: PV, PVC and StorageClass](https://blog.mayadata.io/kubernetes-storage-basics-pv-pvc-and-storageclass)

[Persistent Volumes and Persistent Volume Claims in Kubernetes - I](https://blog.knoldus.com/persistent-volumes-and-persistent-volume-claims-in-kubernetes-i/)