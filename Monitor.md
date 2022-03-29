# Monitor Node Health

[https://kubernetes.io/docs/tasks/debug-application-cluster/monitor-node-health/](https://kubernetes.io/docs/tasks/debug-application-cluster/monitor-node-health/)

*Node Problem Detector* is a daemon for monitoring and reporting about a node's health. You can run Node Problem Detector as a `DaemonSet`or as a standalone daemon. Node Problem Detector collects information about node problems from various daemons and reports these conditions to the API server as [NodeCondition](https://kubernetes.io/docs/concepts/architecture/nodes/#condition) and [Event](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#event-v1-core)

## **Limitations[](https://kubernetes.io/docs/tasks/debug-application-cluster/monitor-node-health/#limitations)**

- Node Problem Detector only supports file based kernel log. Log tools such as `journald` are not supported.
- Node Problem Detector uses the kernel log format for reporting kernel issues. To learn how to extend the kernel log format, see [Add support for another log format](https://kubernetes.io/docs/tasks/debug-application-cluster/monitor-node-health/#support-other-log-format).