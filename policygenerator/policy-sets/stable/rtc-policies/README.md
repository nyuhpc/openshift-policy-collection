# Openshift RTC PolicySet

## Prerequisites
 To install OpenShift RTC using this PolicySet, you must first have:
 - A supported version of Red Hat OpenShift Container Platform 
 - An install of Red Hat Advanced Cluster Management for Kubernetes version 2.8 or newer.
   - Follow the document [Requirements and Recommendations] for Red Hat Advanced Cluster Management for Kubernetes(https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.8/html/install/installing#requirements-and-recommendations)

---

## Installation

The OpenShift RTC PolicySet contains one `PolicySet` that will be deployed.  The OpenShift RTC PolicySet installs everything onto the ACM/Open Cluster Management hub cluster.

Prior to applying the `PolicySet`, perform these steps:

1. To allow for subscriptions to be applied below you must apply and set to enforce the policy [policy-configure-subscription-admin-hub.yaml](https://github.com/open-cluster-management-io/policy-collection/blob/main/community/CM-Configuration-Management/policy-configure-subscription-admin-hub.yaml).
2. Install the Policy generator Kustomize plugin by following the [installation instructions](https://github.com/stolostron/policy-generator-plugin#installation). It is recommended to use Kustomize v4.5+.
3. Policies are installed to the `rtc-policies` namespace.  Make sure the placement bindings match this namespace for the hub and other managed clusters.
   Example yaml to apply a ManagedClusterSetBinding for the policies namespace.
    ```apiVersion: cluster.open-cluster-management.io/v1beta1
    kind: ManagedClusterSetBinding
    metadata:
        name: default
        namespace: rtc-policies
    spec:
        clusterSet: default
    ```
    ```bash
    oc apply -f managed-cluster.yaml 
    ```

Apply the policies using the kustomize command or subscribing to a fork of the repository and pointing to this directory.  See the details for using the Policy Generator for [more information](https://github.com/stolostron/policy-collection/tree/main/policygenerator).  The command to run is `kustomize build --enable-alpha-plugins  | oc apply -f -`

**Note**: For any components of OpenShift Plus that you do not wish to install, edit the `policyGenerator.yaml` file and remove or comment out the policies for those components.