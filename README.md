# acm-application-policy
This repo is just meant to test the capabilities of the ACM Subscriptions pointing to the PolicyGenerator from https://github.com/open-cluster-management-io/policy-generator-plugin

# Please Note!
In order for subscriptions to PolicyGenerator to work, you need to add the user to the right [ClusterRoleBinding](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.7/html/applications/managing-applications#granting-subscription-admin-privilege):
```
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: open-cluster-management:subscription-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: open-cluster-management:subscription-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: system:admin
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: kube:admin
```

Then, you need to [allow the various objects types](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.7/html/applications/managing-applications#creating-allow-deny-list) in the Subscription:
```
---
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  [...]
spec:
  allow:
  - apiVersion: policy.open-cluster-management.io/v1
    kinds:
    - Policy
    - PlacementBinding
  - apiVersion: apps.open-cluster-management.io/v1
    kinds:
    - PlacementRule
  - apiVersion: v1
    kinds:
    - Namespace
```
