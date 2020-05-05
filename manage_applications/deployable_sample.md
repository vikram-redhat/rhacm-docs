# Deployables samples

Deployables (`deployable.apps.open-cluster-management.io`) are Kubernetes resources that contain templates to wrap other Kubernetes resources or represent Helm releases for deployment to clusters to create or manage applications. For more information about creating and managing deployables, see [Managing deployables](managing_deployables.md).

## Example deployable definition YAML structure

The following YAML structure shows the required fields for a deployable and some of the common optional fields. Your YAML structure needs to include some required fields and values. Depending on your deployable requirements or application management requirements, you might need to include other optional fields and values. The structure for a deployable is the same whether you are deploying to a single cluster or multiple clusters.

The following YAML structure shows the required fields for an application and some of the common optional fields. You can compose the YAML content with any tool.

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Deployable
metadata:
  name:
  namespace:
  annotations:
  labels:
spec:
  channels:
  template:
  dependencies:
    name:
    kind:
    apiVersion:
    annotations:
  overrides:
    clusterName:
    clusterOverrides:
      path:
      value:
  placement:
    clusterSelector:
```
## YAML values table {#deployable-yaml-values}

|Field|Description|
|-- | -- |
| apiVersion | Required. Set the value to `apps.open-cluster-management.io/v1`. |
| kind | Required. Set the value to `Deployable` to indicate that the resource is a deployable. |
| metadata.name | Optional. The name of the deployable. If you do not set a name when you are creating a deployable, a `generateName` is created by Kubernetes to identify the deployable. |
| metadata.namespace | Optional. The namespace for the deployable.  |
| metadata.annotations | Optional. The annotations for the deployable. If the deployable needs to be in a channel, the annotations must match the channel gate annotations. |
| metadata.labels | Optional. The labels for the deployable. |
| spec.channels | The channel or channels where the deployable is to be promoted. A deployable must have any required annotations before the deployable is promoted.
| spec.dependencies | Optional. Specify dependencies between the deployable and other Kubernetes objects. You can define an array of dependencies and apply the dependencies to all target clusters. Each dependency must reference a Kubernetes object on the Hub cluster.  |
| spec.overrides | Define the overrides for the deployable, such as to override placement settings from a shared placement rule. |
| spec.placement | Define where the deployable is to be deployed, such as to a single cluster or to multiple clusters. Alternatively, you can specify a placement rule for the deployable, which can then define how to deploy the deployable to the target cluster or clusters. |
{: caption="Table 1. Required and optional definition fields" caption-side="top"}

## Deployable sample

```YAML
apiVersion: apps.open-cluster-management.io/v1
kind: Deployable
metadata:
  annotations:
    apps.open-cluster-management.io/is-local-deployable: "false"
  labels:
    app: nginx-app-details
  name: example-configmap
  namespace: ns-sub-1
spec:
  template:
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: config1
      namespace: default
    data:
      purpose: for test
```
