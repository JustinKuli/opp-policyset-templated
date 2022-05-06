# opp-policyset-templated

For reference, check [the original README for this policyset](./policyset/README.md) and the
latest version of the policy set in the
[policy-collection repo](https://github.com/stolostron/policy-collection/tree/main/policygenerator/policy-sets/community/openshift-plus).

1. Install the policy generator as a Kustomize plugin on your machine:
https://github.com/stolostron/policy-generator-plugin/blob/main/README.md#install-from-the-github-release

2. Log in to your ACM Hub

3. Prepare for ODF by adding storage nodes and creating/labelling the storage namespace. More info:
https://github.com/stolostron/policy-collection/tree/main/policygenerator/policy-sets/community/openshift-plus

4. Create the PolicySets: `cd policyset && kustomize build --enable-alpha-plugins | oc apply -f -`

5. Create the `opp-policy-config` configmap on the hub and managed clusters, and the policies should
start progressing towards compliance (before this, the templates that require that configmap will
effectively pause most of those policies on that cluster).

## Summary of resources

### On the hub cluster

| Policy | Configuration | Resources |
| --- | --- | --- |
| [policy-acs-operator-central](./policyset/input-acs-central/policy-acs-operator-central.yaml) | n/a | Namespaces: `stackrox`, `rhacs-operator` |
| ⮑ | n/a | OperatorGroup: `rhacs-operator/rhacs-operator-group` |
| ⮑ | `rhacs_operator_*` | Subscription: `rhacs-operator/rhacs-operator` |
| ⮑ | n/a | Central: `stackrox/stackrox-central-services` |
| [policy-acs-central-status](./policyset/input-acs-central/policy-acs-central-status.yaml) | n/a | Deployments: `stackrox/central`, `stackrox/scanner-db`, `stackrox/scanner` |
| [policy-acs-central-ca-bundle](./policyset/input-sensor/policy-acs-central-ca-bundle.yaml) | n/a | Namespace: `stackrox` |
| ⮑ | n/a | ServiceAccount, Role & RoleBinding: `stackrox/create-cluster-init` |
| ⮑ | n/a | Job: `stackrox/create-cluster-init-bundle` |
| ⮑ | `rhacs_operator_*` | Subscription: `openshift-operators/rhacs-operator` |
| ⮑ | n/a | SecuredCluster: `stackrox/stackrox-secured-cluster-services` |
| ⮑ | n/a | Secrets: `policies/admission-control-tls`, `policies/collector-tls`, `policies/sensor-tls` |
| [policy-advanced-managed-cluster-status](./policyset/input-sensor/policy-advanced-managed-cluster-status.yaml) | n/a | Deployment: `stackrox/sensor` |
| ⮑ | n/a | DaemonSet: `stackrox/collector` |
| [policy-ocm-observability](./policyset/input-acm-observability/policy-ocm-observability.yaml) | n/a | Secret: `open-cluster-management-observability/thanos-object-storage` |
| ⮑ | n/a | MultiClusterObservability: `observability` |
| [policy-odf](./policyset/input-odf/policy-odf.yaml) | n/a | Namespace: `openshift-storage` |
| ⮑ | n/a | OperatorGroup: `openshift-storage/openshift-storage-operatorgroup` |
| ⮑ | `odf_operator_*` | Subscription: `openshift-storage/odf-operator` |
| ⮑ | `ocs_operator_*` | Subscription: `openshift-storage/ocs-operator` |
| ⮑ | `mcg_operator_*` | Subscription: `openshift-storage/mcg-operator` |
| ⮑ | n/a | StorageSystem: `openshift-storage/ocs-storagecluster-storagesystem` |
| ⮑ | n/a | StorageCluster: `openshift-storage/ocs-storagecluster` |
| [policy-odf-status](./policyset/input-odf/policy-odf-status.yaml) | n/a | Deployments: `openshift-storage/noobaa-operator`, `openshift-storage/ocs-operator`, `openshift-storage/odf-operator-controller-manager` |
| [policy-config-quay](./policyset/input-quay/policy-config-quay.yaml) | n/a | Namespace: `local-quay` |
| ⮑ | n/a | OperatorGroup: `local-quay/local-quay` |
| ⮑ | `quay_operator_*` | Subscription: `local-quay/quay-operator` |
| ⮑ | n/a | Secret: `local-quay/init-config-bundle-secret` |
| ⮑ | n/a | QuayRegistry: `local-quay/registry` |
| ⮑ | n/a | ServiceAccount, Role & RoleBinding: `local-quay/create-admin-user` |
| ⮑ | n/a | Job: `local-quay/create-admin-user` |
| [policy-quay-status](./policyset/input-quay/policy-quay-status.yaml) | n/a | Deployments: `local-quay/registry-quay-app`, `local-quay/registry-quay-database` |
| [policy-compliance-operator-install](./policyset/input-compliance/policy-compliance-operator-install.yaml) | n/a | Namespace: `openshift-compliance` |
| ⮑ | n/a | OperatorGroup: `openshift-compliance/compliance-operator` |
| ⮑ | `compliance_operator_*` | Subscription: `openshift-compliance/compliance-operator` |


### On the managed clusters

| Policy | Configuration | Resources |
| --- | --- | --- |
| [policy-advanced-managed-cluster-security](./policyset/input-sensor/policy-advanced-managed-cluster-security.yaml) | n/a | Namespaces: `stackrox`, `rhacs-operator` |
| ⮑ | n/a | OperatorGroup: `rhacs-operator/rhacs-operator-group` |
| ⮑ | `rhacs_operator_*` | Subscription: `rhacs-operator/rhacs-operator` |
| ⮑ | n/a | Secrets: `stackrox/admission-controler-tls`, `stackrox/collector-tls`, `stackrox/sensor-tls` |
| ⮑ | n/a | SecuredCluster: `stackrox/stackrox-secured-cluster-services` |
| [policy-advanced-managed-cluster-status](./policyset/input-sensor/policy-advanced-managed-cluster-status.yaml) | n/a | Deployment: `stackrox/sensor` |
| ⮑ | n/a | DaemonSet: `stackrox/collector` |
| [policy-compliance-operator-install](./policyset/input-compliance/policy-compliance-operator-install.yaml) | n/a | Namespace: `openshift-compliance` |
| ⮑ | n/a | OperatorGroup: `openshift-compliance/compliance-operator` |
| ⮑ | `compliance_operator_*` | Subscription: `openshift-compliance/compliance-operator` |
