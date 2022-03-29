# Deployment | Jobs

# Services

---

**Service** is an abstraction level that Kubernetes uses to make a deployed application accessible from the internal and external of the cluster.  A Service in Kubernetes is a REST object, similar to a Pod. Like all of the REST objects, you can `POST`
 a Service definition to the API server to create a new instance. The name of a Service object must be a valid [`RFC 1035 label name`](https://kubernetes.io/docs/concepts/overview/working-with-objects/names#rfc-1035-label-names)
.

[Understanding Services in Kubernetes with Examples!](https://blog.knoldus.com/understanding-services-in-kubernetes-with-examples/)

[Service](https://kubernetes.io/docs/concepts/services-networking/service/)

# ReplicaController | ReplicaSet

---

## **What is Kubernetes replication for?**

Before we go into the details on how you would do replication, let’s talk about why.  Typically you would want to replicate your containers (and thereby your applications) for several reasons, including:

- **Reliability:** By having multiple versions of an application, you prevent problems if one or more fails. This is particularly true if the system replaces any containers that fail.
- **Load balancing:** Having multiple versions of a container enables you to easily send traffic to different instances to prevent overloading of a single instance or node. This is something that Kubernetes does out of the box, making it extremely convenient.
- **Scaling:** When load does become too much for the number of existing instances, Kubernetes enables you to easily scale up your application, adding additional instances as needed.

Replication is appropriate for numerous use cases, including:

- **Microservices-based applications:** In these cases, multiple small applications provide very specific functionality.**Cloud native applications:** Because cloud-native applications are based on the theory that any component can fail at any time, replication is a perfect environment for implementing them, as multiple instances are baked into the architecture.
- **Mobile applications:** Mobile applications can often be architected so that the mobile client interacts with an isolated version of the server application.

Kubernetes has multiple ways in which you can implement replication.

[ReplicationController and ReplicaSet in Kubernetes](https://blog.knoldus.com/replicationcontroller-and-replicaset-in-kubernetes/)

[](https://www.mirantis.com/blog/kubernetes-replication-controller-replica-set-and-deployments-understanding-replication-options/)

ReplicationController and ReplicaSet are considered as wrappers on a pod. Before we start learning about replication we should have known about Pods.

## ReplicaController

---

replication controller, also termed as **`rc`**in short, is a wrapper on pod. The replication controller provides an additional functionality to the pods which is providing replicas.

The replication controller monitors pods and automatically restarts them if they fail. Also, if the whole node fails, the replication controller respawn all the pods of that node on some different node whereas in case of pods, once they dies, they won’t be spawn again unless and until they are wrapped around replication controller or replica set.

The scope of replication controller is decided by the `label-selectors`. Whichever pod matches its label with the provided selector, that will be managed by the replication controller.

A replication controller continuously monitors the list of running pods by running a `reconciliation loop` and ensures that the specified number of replicas are always running. It maintains the replicas in two ways:

- By creating new replicas if the actual replicas are less than the desired replicas, and
- By removing extra replicas if the actual replicas are more than the desired replicas. This can be possible if,
    - A pod is created of the same type manually.
    - A label of an existing pod’s changes to a value which is same as the replication controller label-selector.
    - Someone decreases the desired number of pods.

As I told before, the replication controller ensures that the actual pods always match the desired pods, we can verify the same by deleting a running pod controlled by replication controller manually and the replication controller will automatically spawn a new pod to meet the desired state. Here’s an example:

**Get the pod replicas:**`kubectl get pods`**Delete any pod in replication controller fetched from previous cmd:**`kubectl delete pod nginx-rc-haShCoDe`**Get the pod replicas again:**`kubectl get pods`Here, you’ll see that a new pod has spawned with a different podname.

![Untitled](Deployment%20b36c8/Untitled.png)

Also, inside the spec, if we didn’t put the selector, the replication controller will automatically configure it with the labels of pod, as the pod labels must be same as the replication controller selectors, otherwise the pods will move out of the scope of replication controller.

## ReplicaSet

---

Replica set, also termed as **`rs`** in short, is almost same as the replication controller is, only with a single difference. The replica set are also known as next generation replication controller. The only difference between replica set and replication controller is the `selector types`.

The replication controller supports equality based selectors whereas the replica set supports equality based as well as set based selectors.

[What is the difference between ReplicaSet and ReplicationController?](https://stackoverflow.com/questions/36220388/what-is-the-difference-between-replicaset-and-replicationcontroller)

```
+--------------------------------------------------+-----------------------------------------------------+
|                   Replica Set                    |               Replication Controller                |
+--------------------------------------------------+-----------------------------------------------------+
| Replica Set supports the new set-based selector. | Replication Controller only supports equality-based |
| This gives more flexibility. for eg:             | selector. for eg:                                   |
|          environment in (production, qa)         |             environment = production                |
|  This selects all resources with key equal to    | This selects all resources with key equal to        |
|  environment and value equal to production or qa | environment and value equal to production           |
+--------------------------------------------------+-----------------------------------------------------+
```

```
+-------------------------------------------------------+-----------------------------------------------+
|                      Replica Set                      |            Replication Controller             |
+-------------------------------------------------------+-----------------------------------------------+
| rollout command is used for updating the replica set. | rolling-update command is used for updating   |
| Even though replica set can be used independently,    | the replication controller. This replaces the |
| it is best used along with deployments which          | specified replication controller with a new   |
| makes them declarative.                               | replication controller by updating one pod    |
|                                                       | at a time to use the new PodTemplate.         |
+-------------------------------------------------------+-----------------------------------------------+
```

# Jobs

---

[Kubernetes Jobs 101 - The Task Jobs](https://www.magalix.com/blog/kubernetes-jobs-101)

[Understanding Jobs in Kubernetes](https://levelup.gitconnected.com/understanding-jobs-in-kubernetes-68ac21b272d8)

[Jobs in Kubernetes](https://blog.knoldus.com/jobs-in-kubernetes/)

![Untitled](Deployment%20b36c8/Untitled%201.png)

The main function of a job is to create one or more pod and tracks about the success of pods. They ensure that the specified number of pods are completed successfully. When a specified number of successful run of pods is completed, then the job is considered complete.

The main purpose behind the job is to ensure that the pods created by the job complete their tasks and **terminates**
 successfully.

### **NOTE :**

> Pods are just container-based applications that run code. And so it’s the responsibility of a job to run a program in a container until its completion. And it’s the responsibility of the job controller to ensure that the specified number of pods complete successfully.
> 

A job is specified to run pods that will not run indefinitely. Successful completion of pods means that the jobs have completed successfully. `Deleting a job will delete all the pods that it creates.`If a job has a specified number upto which times, it has to be carried out; and if any of those times, a pod fails, the job starts a new pod to replace the older versions, keeping the number of pods required to run, the same. i.e,

If a job had to run 5 times, and imposingly after the first two runs, the third run fails, then that run will be restarted, thereby making the total number of runs to six, in order to maintain the total number of successful runs to five.

[Job resources](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) are used to facilitate the execution of a batch job. Through Job resources, Kubernetes also supports parallel jobs which will finish executing when a specific number of successful completions is reached.

Therefore with Job resources, we can run work items such as frames to be rendered, files to be transcoded, ranges of keys in a NoSQL database to scan, and so on.

Have a look at [Jobs Api reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#job-v1-batch) to see how to build a job resource in Kubernetes.

Pods created by jobs are not automatically deleted. Keeping the pods around allows you to view the logs of completed jobs in order to check for potential errors. If you want to remove them, you need to do that manually.

## **Job Spec**

---

The job spec includes :

- apiVersion
- kind
- metadata
- podTemplate section
- pod selector field is optional.

## **RestartPolicy**

---

The RestartPolicy can be set to *`Never`* or *`OnFailure`*. A container in a pod can fail due to any number of reasons. So in such a case, if the restartPolicy in the spec.template.spec section is set to “OnFailure”, then the pod stays on the node, and the container is started again.

But if in such a case, if the restartPolicy is set to “Never”, then the job controller starts a new pod.

> The restartPolicy of a pod in the spec section always defaults to `Always`. Job pods can’t use the default policy, because they’re not meant to run indefinitely. Therefore, one needs to explicitly set the restart policy to either OnFailure or Never.
> 

## **Parallel Jobs**

---

There are many variations in such a case :

***Non-parallel jobs :***

- Here, normally only one pod is up, unless that pod fails. Only when a pod fails, a new pod is started.
- The job is said to be completed as soon as the pods terminates successfully.

***Running multiple pod instances in a Job***

<aside>
💼 Use of completions and parallelism fields :

</aside>

Jobs can be configured to create more than one pod instances and run them in parallel or in a sequential manner.*completions :*

- The spec.completions field is assigned a non-zero positive integer.
- In order to run a job more than once, set completions to how many times you want the Job’s pod to run. *parallelism :*
- One can specify how many pods should run in parallel,with the parallelism Job spec property.

<aside>
💼 for example, By setting parallelism to 2, the Job creates two pods and runs them in parallel

</aside>

- The parallelism property of the job can be changed even when the job is running, with the help of scale command.`kubectl scale job job-name --replicas count`

### **Few extra fields that can be used with jobs :**

---

1. **backOffLimit** :

This is useful when we want to a job to be considered as a failure after some amount of retries due to a logical error,etc.Use `.spec.backoffLimit` to specify the number of retries before considering a Job as failed. The back-off limit is set by default to 6.

1. **activeDeadlineSeconds** :

A pod’s time can be limited by setting the activeDeadlineSeconds field in the pod spec. If the pod runs longer than that, the system will try to terminate it and will mark the Job as failed.

**Note** :

- Note that a Job’s `.spec.activeDeadlineSeconds` takes precedence over its `.spec.backoffLimit`.
- Even after the termination of a job, the pods are not deleted, instead they are preserved in order for us to view the logs.
1. **ttlSecondsAfterFinished**This is again a way of cleanup of finished jobs (either Completed or Failed). The field `.spec.ttlSecondsAfterFinished` specifies the time period for how long a finished job will persist in the system. If this field is set to zero, the jobs are automatically deleted / cleaned up as soon as they are finished. But if the field is unset, the job won’t be cleaned up the TTL controller. Important Note :

In order to delete a job but leave it’s pods running, perform

```
 kubectl delete job job-name --cascade=false
```

### **Use case of jobs :**

- In a Message queuing system, ( consider as a producer – consumer service ) : The producer can send the message and exit, but each message has to be processed by a consumer, that can be done by creating a job each for all the consumers.
- One-time initialising of resources such as databases, etc.

[Jobs and CronJobs in Kubernetes](https://blog.knoldus.com/jobs-and-cronjobs-in-kubernetes/)

## **CronJobs**

---

Jobs are classified as :

- Runs to completion ( JOBS )
- Scheduled jobs ( CRONJOBS )

A cronjob creates a job to be repeated on a schedule. A cron job in Kubernetes is configured by creating a CronJob resource. The schedule for running the job is specified in the well-known cron format.All **CronJob schedules** times are based on the `timezone` of the kube-controller-manager.

**The cron schedule format** From left to right, the schedule contains the following five entries:

- Minute
- Hour
- Day of month
- Month
- Day of week.

### **Cron schedule syntax**

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │                                   OR sun, mon, tue, wed, thu, fri, sat
# │ │ │ │ │
# * * * * *

```

[Cronetes](https://www.notion.so/9716eaaafc574104b1d309e883a212f7)

<aside>
💼 Ending Note :Pods that perform a batch task should be created through a Kubernetes Job resource, and not directly or through a ReplicationController, etc.Jobs that need to run sometime in the future can be created through CronJob resources.

</aside>

### Job Summary

---

- Kubernetes Jobs are used when you want to create pods that will do a specific task and then exit.
- Kubernetes Jobs do not need pod selectors by default as pods; their labels and selectors are handled automatically by the Job.
- The restartPolicy for a Job accepts “Never” and “OnFailure”
- Jobs use completion and parallelism parameters to control the patterns the pods run through. Job pods can run as a single task, several sequential tasks, or some parallel tasks in which the first task that finishes instructs the rest of the pods to complete and exit.
- You can control how many times a Job attempts to restart a failing pod using the .spec.backoffLimit. This limit defaults to six.
- You can control how long a job will run using the .spec.activeDeadlineSeconds. This limit overrules the backoffLimit. So the Job does not attempt to restart a failing pod if the deadline is reached.
- Jobs and their pods do not get deleted automatically when they finish. You have to manually delete them or use the ttlSecondsAfterFinished controller, which is still in alpha stage as of the time of this writing.

[Kubernetes - Jobs](https://www.tutorialspoint.com/kubernetes/kubernetes_jobs.htm)

[Running a Job | Kubernetes Engine Documentation | Google Cloud](https://cloud.google.com/kubernetes-engine/docs/how-to/jobs)

[](http://ubernetes.io/docs/concepts/workloads/controllers/job/)

# Deployment

---

in Kubernetes, a Deployment is an object that can represent an application running on your cluster.

[Belajar Kubernetes - 41 Deployment](https://www.youtube.com/watch?v=JSriEH8U7RU)

[What is Deployment in Kubernetes?](https://blog.knoldus.com/what-is-deployment-in-kubernetes/)

[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) in kubernetes is an upgraded and higher version of Replication controller. They manage the deployment of a replica set which is also an upgraded version of the replication controller.

In simple terms, a Kubernetes deployment is a [tool that manages the performance](https://containerjournal.com/features/fairwinds-updates-kubernetes-cluster-monitoring-tool/)
 and specifies the desired behavior or traits of a pod.

For example, Kubernetes deployments can be used to roll out a ReplicaSet to create pods and check their health to see if they are working optimally.

- [Deployment](https://blog.knoldus.com/introduction-to-kubernetes-deployment-strategies/) in kubernetes allows you to describe an application life cycle, such as which images to use for the app, the number of pods, and the way in which they should be updated.
- In kubernetes, Deployment have the capability to update the replica set and are also capable of rolling back to the previous version. They provide many updated features of match labels and selectors.
- What we do is we describe a desired state in a deployment and we have a controller called deployment controller which makes it happen. Deployment controller changes the actual state to the desired state.
- Deployments are declarative, which means that they have what to achieve not how to achieve. To achieve this desired state, deployment uses Replica Sets, which further maintains the required set of pods

Many resources in Kubernetes use a [Pod template](https://kubernetes.io/docs/concepts/workloads/pods/#pod-templates). Both `Deployments` and `Jobs` use it, because they manage Pods.

> Controllers for workload resources create Pods from a pod template and manage those Pods on your behalf.
> 

> PodTemplates are specifications for creating Pods, and are included in workload resources such as Deployments, Jobs, and DaemonSets.
> 

The main difference between `Deployments` and `Jobs` is **how they handle a Pod that is terminated**. A Deployment is intended to be a "service", e.g. it should be up-and-running, so it will try to restart the Pods it manage, to match the desired number of replicas. While a Job is intended to execute and successfully terminate.