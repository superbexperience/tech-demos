# Intro
This repository attempts to show off the capabilities of [ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/) and [karpenter](https://karpenter.sh/) in on demo.

## Prerequisites
- EKS cluster with at least 1 node (nodegroup or fargate profile) and OIDC enabled
- Kubectl access to the cluster

## Setup
### Karpenter
1. Follow the steps in the [AWS documentation](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html) to create an IAM role for the kubernetes service account to use.
2. Create a security group for the nodes' ingress and egress traffic
3. Deploy the Karpenter Helm chart with the following value overrides:
```yaml
serviceAccount:
    create: 'true'
    name: karpenter-service-account
annotations:
  eks.amazonaws.com/role-arn: <arn of the Role you created>
aws:
    defaultInstanceProfile: <name of the instance profile associated with the rol>
clusterName: <Cluster name>
clusterEndpoint: <Cluster endpoint>
```
3. Deploy the [Karpenter provisioner](karpenter/provisioner.yaml) replacing the `<subnet tag>` and  `<security group name>` with relevant values


### ArgoCD
1. Deploy the [ArgoCD Helm chart](https://github.com/bitnami/charts/tree/master/bitnami/argo-cd/#installing-the-chart)
2. Find the `argocd-secret` and copy the value associated with the `clearPassword` key
3. Login to ArgoCD
4. Add this repository as an application
