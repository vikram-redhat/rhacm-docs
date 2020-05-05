# Channels samples

View samples and YAML definitions that you can use to build your files. Channels (`channel.apps.open-cluster-management.io`) provide you with improved continuous integration and continuous delivery capabilities for creating and managing your Red Hat Advanced Cluster Management for Kubernetes applications. Learn more at [Creating and managing channels](managing_channels.md).

## Example channel definition YAML structure {#channel-definition-yaml-structure}

The definition structure for a channel can resemble the following YAML content:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name: predev-ch
  namespace: ns-ch
  labels:
    app: nginx-app-details
spec:
  type: HelmRepo
  pathname: https://kubernetes-charts.storage.googleapis.com/
```

The following YAML structures show the required fields for a channel and some of the common optional fields. Your YAML structure needs to include some required fields and values. Depending on your application management requirements, you might need to include other optional fields and values. You can compose your own YAML content with any tool.

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name:
  namespace:
spec:
  sourceNamespaces:  
  type:
  pathname:
  secretRef:
    name:
  gates:
    annotations:
  labels:
```
## YAML values table {#channel-yaml-values}

|Field|Description|
|-- | -- |
| apiVersion | Required. Set the value to `apps.open-cluster-management.io/v1`. |
| kind | Required. Set the value to `Channel` to indicate that the resource is a channel. |
| metadata.name | Required. The name of the channel. |
| metadata.namespace | Required. The namespace for the channel. |
| spec.sourceNamespaces | Optional. Identifies the namespace that the channel controller monitors for new or updated deployables to retrieve and promote to the channel. For a `Namespace` channel, the namespace must be on the hub cluster. |  
| spec.type | Required. The channel type. The supported types are: `Namespace`, `HelmRepo`, `GitHub`, and `ObjectBucket` |
| spec.pathname | Required for `HelmRepo`, `GitHub`, `ObjectBucket`, `Namespace` channels. <ul><li>For a `HelmRepo` channel, set the value to be the URL for the Helm repository.</li><li>For an `ObjectBucket` channel, set the value to be the URL for the Object store.</li><li>For a `GitHub` channel, set the value to be the HTTPS URL for the GitHub repository.</li><li>For a `Namespace` channel, set the value to be the namespace where the channel is included.</li></ul> |
| spec.secretRef.name | Optional. Identifies a Kubernetes Secret resource to use for authentication, such as for accessing a repository or chart. You can use a secret for authentication with only `HelmRepo`, `ObjectBucket`, and `GitHub` type channels. |
| spec.gates | Optional. Defines requirements for promoting a deployable within the channel. If no requirements are set, any deployable that is added to the channel namespace or source is promoted to the channel. `gates` do not apply for `HelmRepo` and `GitHub` channel types, only for `Namespace` and `ObjectBucket` channel types. |
| spec.gates.annotations | Optional. The annotations for the channel. Deployables must have matching annotations to be included in the channel. |
| spec.labels | Optional. The labels for the channel. |
{: caption="Table 1. Required and optional definition fields" caption-side="top"}

## Kubernetes namespace (Namespace) channel

The following example channel definition abstracts a namespace as a channel that holds deployable resources. When this YAML is applied, a namespace `ch-qa` is created for the channel that is named `qa`. When created, this channel points to the source default namespace for identifying deployables. The channel controller maintains the resources at the actual namespace location and ensures that the resources are kept up-to-date.

  ```yaml
  apiVersion: apps.open-cluster-management.io/v1
  kind: Channel
  metadata:
    name: qa
    namespace: ch-qa
  spec:
    sourceNamespaces:
    - default
    type: Namespace
    pathname: ch-qa
    gates:
      annotations:
        dev-ready: approved
  ```

## Object store bucket (`ObjectBucket`) channel

The following example channel definition abstracts an object store bucket as a channel:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
 name: dev
 namespace: ch-obj
spec:
 type: ObjectBucket
 pathname: [http://9.28.236.243:31311/dev] # URL is appended with the valid bucket name, which matches the channel name.
 secretRef:
   name: miniosecret
 gates:
   annotations:
     dev-ready: true
```

## Helm repository (`HelmRepo`) channel

The following example channel definition abstracts a Helm repository as a channel:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: hub-repo
---
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name: helm
  namespace: hub-repo
spec:
    pathname: [https://9.21.107.150:8443/helm-repo/charts] # URL points to a valid chart URL.
    configRef:
      name: insecure-skip-verify
    type: HelmRepo
---
apiVersion: v1
data:
  insecureSkipVerify: "true"
kind: ConfigMap
metadata:
  name: insecure-skip-verify
  namespace: hub-repo
```

The following channel definition shows another example of a Helm repository channel:

```YAML
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name: predev-ch
  namespace: ns-ch
  labels:
    app: nginx-app-details
spec:
  type: HelmRepo
  pathname: https://kubernetes-charts.storage.googleapis.com/
```

## GitHub (`GitHub`) repository channel

The following example channel definition shows an example of a channel for the GitHub Repository. In the following example, `secretRef` refers to the user identity used to access the GitHub repo that is specified in the `pathname`. If you have a public repo, you do not need the `secretRef`:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name: hive-cluster-gitrepo
  namespace: gitops-cluster-lifecycle
spec:
  type: GitHub
  pathname: https://github.com/open-cluster-management/gitops-clusters.git
  secretRef:
    name: github-gitops-clusters
---
apiVersion: v1
kind: Secret
metadata:
  name: github-gitops-clusters
  namespace: gitops-cluster-lifecycle
data:
  user: dXNlcgo=            # Value of user and accessToken is Base 64 coded.
  accessToken: cGFzc3dvcmQ
```
