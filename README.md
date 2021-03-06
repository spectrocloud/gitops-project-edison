# Project Edison

Config for the project

## Module

- One or more account files account-aws-1.yaml
- One or more cluster yaml files

## Cluster YAML

The yaml file:
```
name: eks-dev-1
cloud_account: aws-account-1
profiles:
  infra: ProdEKS-1
  ehl: EHS-1.6-small
cloud_config:
  aws_region: us-east-1
  aws_vpc_id: vpc-0c9679602584608f9
  eks_subnets:
    us-west-2a: subnet-07f0af093b8233990,subnet-080b9dc42d15b57a2
    us-west-2b: subnet-0e0d96dc7ad3f02a5,subnet-025428faaddc4201f
    us-west-2c: subnet-09e74165071ce03e6,subnet-0964e92baa663e495
node_groups:
- name: worker-basic
  count: 3
  disk_size_gb: 61
  instance_type: t3.large
  worker_subnets:
    us-west-2a: subnet-07f0af093b8233990
    us-west-2b: subnet-0e0d96dc7ad3f02a5
    us-west-2c: subnet-09e74165071ce03e6
fargate_profiles:
- name: fg-1
  subnets:
  - subnet-07f0af093b8233990
  - subnet-0e0d96dc7ad3f02a5
  - subnet-09e74165071ce03e6
  additional_tags: {hello: test1}
  selectors:
  - namespace: fargate
    labels:
      abc: cool
rbac:
  clusterRoleBindings:
  - role: view
    subjects:
    - {type: User, name: user6}
    - {type: Group, name: group6}
    - {type: ServiceAccount, name: group6, namespace: foo}
  namespaces:
  - namespace: team2
    createNamespace: true
    roleBindings:
    - role: admin
      kind: ClusterRole
      subjects:
      - {type: Group, name: team-vendor-b}
manifest_repos:
- name: myapps
  type: helm
  url: https://github.com/spectrocloud/gitops-argocd.git
  path: apps
  namespace: team2
terraform_repos:
- name: kubeless-services
  url: https://github.com/spectrocloud/gitops-picard.git
  path: sandbox
  state:
    type: in-cluster-secret
    namespace: team2


```

