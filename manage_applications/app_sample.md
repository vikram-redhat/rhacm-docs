# Application samples

View samples and YAML definitions that you can use to build your files. Applications (`Application.app.k8s.io`) in Red Hat Advanced Cluster Management for Kubernetes are used for viewing the application components. For more information about creating and managing applications, see [Creating and managing applications](managing_apps.md).

## Application definition YAML structure {#app_compose}

To compose the application definition YAML content for creating or updating an application resource, your YAML structure needs to include some required fields and values. Depending on your application requirements or application management requirements, you might need to include other optional fields and values.

The following YAML structure shows the required fields for an application and some of the common optional fields.

```yaml
apiVersion: app.k8s.io/v1beta1
kind: Application
metadata:
  name:
  namespace:
  resourceVersion:
  annotations:
  labels:
    app:
    chart:
    heritage:
    name:
    release:
spec:
  componentKinds:
  - group:
    kind:
  descriptor:
  selector:
    matchExpressions:
    - key:
      operator:
      values:
```

## Application table

|Field|Description|
|-- | -- |
| apiVersion | Required. Set the value to `app.k8s.io/v1beta1`. |
| kind | Required. Set the value to `Application` to indicate the resource is an application resource. |
| metadata.name | Required. The name for identifying the application resource. |
| metadata.namespace | The namespace resource to use for the application. |
| metadata.resourceVersion | The version of the application resource. |
| metadata.annotations | Optional. The annotations for the application. |
| metadata.labels | Optional. The labels for the deployable. |
| spec.componentKinds | Optional. The list of the kinds of resources to be associated with the application. |
| spec.selector.matchExpressions | Optional. Label selectors for associating other Kubernetes resources with the application. |
| spec.selector.matchExpressions.key | Required when defining label selectors. The Kubernetes label key that a resource needs to match. |
| spec.selector.matchExpressions.values | Required when defining label selectors. The Kubernetes label values that a resource needs to match. |
{: caption="Table 1. Required and optional definition fields" caption-side="top"}

The spec for defining these applications is based on the Application metadata descriptor custom resource definition that is provided by the Kubernetes Special Interest Group (SIG). You can use this definition to help you compose your own application YAML content. For more information about this definition, see [Kubernetes SIG Application CRD community specification](https://github.com/kubernetes-sigs/application).

## Application samples

Applications (`Application.app.k8s.io`) in Red Hat Advanced Cluster Management for Kubernetes are used for viewing the application components.

The definition structure for an application can resemble the following example YAML content:

```yaml
apiVersion: app.k8s.io/v1beta1
kind: Application
metadata:
  labels:
    app: nginx-app-details
  name: nginx-app-3
  namespace: ns-sub-1
spec:
  componentKinds:
  - group: apps.open-cluster-management.io
    kind: Subscription
  selector:
    matchLabels:
      app: nginx-app-details
status: {}
```
