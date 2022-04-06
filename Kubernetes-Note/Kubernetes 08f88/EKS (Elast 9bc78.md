# EKS (Elastic Kubernetes Service)

![Untitled](EKS%20(Elast%209bc78/Untitled.png)

Amazon Elastic Kubernetes Service (Amazon EKS) is a managed container service to run and scale Kubernetes applications in the cloud or on-premises.to start using EKS read this documentation below :

[Getting started with Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)

[Getting started with Amazon EKS - AWS Management Console and AWS CLI](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html)

[AWS EKS - Create Kubernetes cluster on Amazon EKS | the easy way](https://www.youtube.com/watch?v=p6xDCz00TxU)

## Prerequisite

---

You have to configure your `aws configure` to authenticate your aws CLI with your aws user account. before it, you must have your user account first, to obtain the user access key ID

step : 

[Configuration basics](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

[Amazon EKS identity-based policy examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)

Somehow role which we are creating as per aws doc is not working. so please follow the steps.

1. create vpc as per doc with a IAM user

2. create the cluster with same IAM user

3. generate the kubconfig file as per doc using aws command

4. comment -r option in kubeconfig file

It should work.