
# Subscription samples

View samples and YAML definitions that you can use to build your files. As with channels, subscriptions (`subscription.apps.open-cluster-management.io`) provide you with improved continuous integration and continuous delivery capabilities for application management. Learn more at [Creating and managing subscriptions](managing_subscriptions.md).

## Example subscription definition YAML structure

The following YAML structure shows the required fields for a subscription and some of the common optional fields. Your YAML structure needs to include some required fields and values. Depending on your application management requirements, you might need to include other optional fields and values. You can compose your own YAML content with any tool.

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name:
  namespace:
  labels:
spec:
  sourceNamespace:
  source:
  channel:
  name:
  packageFilter:
    version:  
    labelSelector:
      matchLabels:  
        package:
        component:
    annotations:  
  packageOverrides:
  - packageName:
    packageAlias:
    - path:
      value:
  placement:
    local:
    clusters:
      name:
    clusterSelector:
    placementRef:
      name:
      kind: PlacementRule
  overrides:
    clusterName:
    clusterOverrides:
      path:
      value:
  timewindow:
    windowtype:
    location:
    daysofweek:
    hours:
      - start:
        end:
```
## YAML values table {#subscription-yaml-values}

|Field|Description|
|-- | -- |
| apiVersion | Required. Set the value to `apps.open-cluster-management.io/v1`. |
| kind | Required. Set the value to `Subscription` to indicate that the resource is a subscription. |
| metadata.name | Required. The name for identifying the subscription. |
| metadata.namespace | Required. The namespace resource to use for the subscription. |
| metadata.labels | Optional. The labels for the subscription. |
| spec.channel | Optional. The NamespaceName ("Namespace/Name") that defines the channel for the subscription. Define either the `channel`, or the `source`, or the `sourceNamespace` field. In general, use the `channel` field to point to the channel instead of using the `source` or `sourceNamespace` fields. If more than one field is defined, the first field that is defined is used. |
| spec.sourceNamespace | Optional. The source namespace where deployables are stored on the Hub cluster. Use this field only for namespace channels. Define either the `channel`, or the `source`, or the `sourceNamespace` field. In general, use the `channel` field to point to the channel instead of using the `source` or `sourceNamespace` fields. |
| spec.source | Optional. The path name ("URL") to the Helm repository where deployables are stored. Use this field for only Helm repository channels. Define either the `channel`, or the `source`, or the `sourceNamespace` field. In general, use the `channel` field to point to the channel instead of using the `source` or `sourceNamespace` fields. |
| spec.name | Required for `HelmRepo` type channels, but optional for `Namespace` and `ObjectBucket` type channels. The specific name for the target Helm chart or deployable within the channel. If neither the `name` or `packageFilter` are defined for channel types where the field is optional, all deployables are found and the latest version of each deployable is retrieved. |
| spec.packageFilter | Optional. Defines the parameters to use to find target deployables or a subset of a deployables. If multiple filter conditions are defined, a deployable must meet all filter conditions. |
| spec.packageFilter.version | Optional. The version or versions for the deployable. You can use a range of versions in the form `>1.0`, or `<3.0`. By default, the version with the most recent "creationTimestamp" value is used. |
| spec.packageFilter.annotations | Optional. The annotations for the deployable. |
| spec.packageOverrides | Optional. Section for defining overrides for the Kubernetes resource that is subscribed to by the subscription, such as a Helm chart, deployable, or other Kubernetes resource within a channel. |
| spec.packageOverrides.packageName  | Optional, but required for setting an override. Identifies the Kubernetes resource that is being overwritten. |
| spec.packageOverrides.packageAlias | Optional. Gives an alias to the Kubernetes resource that is being overwritten. |
| spec.packageOverrides.packageOverrides | Optional. The configuration of parameters and replacement values to use to override the Kubernetes resource. For more information, see [Package overrides](#package-overrides). |
| spec.placement | Required. Identifies the subscribing clusters where deployables need to be placed, or the placement rule that defines the clusters. Use the placement configuration to define values for multi-cluster deployments. |
| spec.local | Optional, but required for a stand-alone cluster or cluster that you want to manage directly. Defines whether the subscription must be deployed locally. Set the value to `true` to have the subscription synchronize with the specified channel. Set the value to `false` to prevent the subscription from subscribing to any resources from the specified channel. Use this field when your cluster is a stand-alone cluster or you are managing this cluster directly. If your cluster is part of a multi-cluster and you do not want to manage the cluster directly, use only one of `clusters`, `clusterSelector`, or `placementRef` to define where your subscription is to be placed. If your cluster is the Hub of a multi-cluster and you want to manage the cluster directly, you must register the Hub as a managed cluster before the subscription operator can subscribe to resources locally. |
| spec.placement.clusters | Optional. Defines the clusters where the subscription is to be placed. Use only one of `clusters`, `clusterSelector`, or `placementRef` to define where your subscription is to be placed for a multi-cluster. If your cluster is a stand-alone cluster that is not your Hub cluster, you can also use `local`. |
| spec.placement.clusters.name | Optional, but required for defining the subscribing clusters. The name or names of the subscribing clusters. |
| spec.placement.clusterSelector | Optional. Defines the label selector to use to identify the clusters where the subscription is to be placed. Use only one of `clusters`, `clusterSelector`, or `placementRef` to define where your subscription is to be placed for a multi-cluster. If your cluster is a stand-alone cluster that is not your Hub cluster, you can also use `local`. |
| spec.placement.placementRef | Optional. Defines the placement rule to use for the subscription. Use only one of `clusters`, `clusterSelector` , or `placementRef` to define where your subscription is to be placed for a multi-cluster. If your cluster is a stand-alone cluster that is not your Hub cluster, you can also use `local`. |
| spec.placement.placementRef.name | Optional, but required for using a placement rule. The name of the placement rule for the subscription. |
| spec.placement.placementRef.kind | Optional, but required for using a placement rule. Set the value to `PlacementRule` to indicate that a placement rule is used for deployments with the subscription. |
| spec.overrides | Optional. Any parameters and values that need to be overridden, such as cluster-specific settings. |
| spec.overrides.clusterName | Optional. The name of the cluster or clusters where parameters and values are being overridden. |
| spec.overrides.clusterOverrides | Optional. The configuration of parameters and values to override. |
| spec.timeWindow | Optional. Defines the settings for configuring a time window when the subscription is active or blocked. |
| spec.timeWindow.type | Optional, but required for configuring a time window. Indicates whether the subscription is active or blocked during the configured time window. Deployments for the subscription occur only when the subscription is active. |
| spec.timeWindow.location | Optional, but required for configuring a time window. The time zone of the configured time range for the time window. All time zones must use the Time Zone (tz) database name format. For more information, see [Time Zone Database](https://www.iana.org/time-zones). |
| spec.timeWindow.daysofweek | Optional, but required for configuring a time window. Indicates the days of the week when the time range is applied to create a time window. The list of days must be defined as an array, such as `daysofweek: ["Monday", "Wednesday", "Friday"]`. |
| spec.timeWindow.hours | Optional, but required for configuring a time window. Defined the time range for the time window. A start time and end time for the hour range must be defined for each time window. You can define multiple time window ranges for a subscription. |
| spec.timeWindow.hours.start | Optional, but required for configuring a time window. The timestamp that defines the beginning of the time window. The timestamp must use the Go programming language Kitchen format `"hh:mmpm"`. For more information, see [Constants](https://godoc.org/time#pkg-constants). |  
| spec.timeWindow.hours.end | Optional, but required for configuring a time window. The timestamp that defines the ending of the time window. The timestamp must use the Go programming language Kitchen format `"hh:mmpm"`. For more information, see [Constants](https://godoc.org/time#pkg-constants). |
{: caption="Table 1. Required and optional definition fields" caption-side="top"}

**Notes:**

* When you are defining your YAML, a subscription can use `packageFilters` to point to multiple Helm charts, deployables, or other Kubernetes resources. The subscription, however, only deploys the latest version of one chart, or deployable, or other resource.
* Annotations are used by a subscription operator for `Namespace` type channels to search for versions of a deployable. The subscription operator searches the versions to find the appropriate deployable version to retrieve. If your channel is a `Namespace` channel, include the annotations for identifying the deployable version.
* For time windows, when you are defining the time range for a window, the start time must be set to occur before the end time. If you are defining multiple time windows for a subscription, the time ranges for the windows cannot overlap. The actual time ranges are based on the `subscription-controller` container time, which can be set to a different time and location than the time and location that you are working within.
* Within your subscription spec, you can also define the placement of a Helm release or deployable as part of the subscription definition. Similar to the definition for deployables, each subscription can reference an existing placement rule, or define a placement rule directly within the subscription definition.
* When you are defining where to place your subscription in the `spec.placement` section, use only one of `clusters`, `clusterSelector`, or `placementRef` for a multi-cluster environment. If you include more than one of `clusters`, `clusterSelector`, or `placementRef`, the following priority is used to determine which setting the subscription operator uses:
  1. `placementRef`
  2. `clusters`
  3. `clusterSelector`

Your subscription can resemble the following YAML content:

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: nginx
  namespace: ns-sub-1
  labels:
    app: nginx-app-details
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
  packageFilter:
    version: "1.36.x"
  placement: # Placement rules help you facilitate multi-cluster deployments, see placement rules documentation.
    placementRef:
      kind: PlacementRule
      name: towhichcluster
  overrides: # See Deployable documentation for more about overrides. Include overrides for any single cluster than requires some different settings
  - clusterName: "/"
    clusterOverrides:
    - path: "metadata.namespace"
      value: default
```

## Subscription examples

```YAML
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: nginx
  namespace: ns-sub-1
  labels:
    app: nginx-app-details
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
```

### Subscription time window example

The following example subscription includes multiple configured time windows. A time window occurs between 10:20 AM and 10:30 AM occurs every Monday, Wednesday, and Friday. A time window also occurs between 12:40 PM and 1:40 PM every Monday, Wednesday, and Friday. The subscription is active only during these six weekly time windows for deployments to begin.  

```YAML
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: nginx
  namespace: ns-sub-1
  labels:
    app: nginx-app-details
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
  packageFilter:
    version: "1.36.x"
  placement:
    placementRef:
      kind: PlacementRule
      name: towhichcluster
  timewindow:
    windowtype: "active" #Enter active or block depending on the purpose of the type.
    location: "America/Los_Angeles"
    daysofweek: ["Monday", "Wednesday", "Friday"]
    hours:
      - start: "10:20AM"
        end: "10:30AM"
      - start: "12:40PM"
        end: "1:40PM"
```

### Subscription with overrides example

The following example includes package overrides to define a different release name of the Helm release for Helm chart. A package override setting is used to set the name `my-nginx-ingress-releaseName` as the different release name for the  `nginx-ingress` Helm release.

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: simple
  namespace: default
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
  packageOverrides:
  - packageName: nginx-ingress
    packageAlias: my-nginx-ingress-releaseName
    packageOverrides:
    - path: spec
      value:
        defaultBackend:
          replicaCount: 3
  placement:
    local: false
```

### Helm repository subscription example

The following subscription automatically pulls the latest `nginx` Helm release for the version `1.36.x`. The Helm release deployable is placed on the `my-development-cluster-1` cluster when a new version is available in the source Helm repository.

The `spec.packageOverrides` section shows optional parameters for overriding values for the Helm release. The override values are merged into the Helm release `values.yaml` file, which is used to retrieve the configurable variables for the Helm release.

```YAML
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: nginx
  namespace: ns-sub-1
  labels:
    app: nginx-app-details
spec:
  channel: ns-ch/predev-ch
  name: nginx-ingress
  packageFilter:
    version: "1.36.x"
  placement:
    clusters:
    - name: my-development-cluster-1
  packageOverrides:
  - packageName: my-server-integration-prod
    packageOverrides:
    - path: spec
      value:
        persistence:
          enabled: false
          useDynamicProvisioning: false
        license: accept
        tls:
          hostname: my-mcm-cluster.icp
        sso:
          registrationImage:
            pullSecret: hub-repo-docker-secret
```

### GitHub repository subscription example

#### Subscribing specific branch and directory of GitHub repository

   ```yaml
    apiVersion: apps.open-cluster-management.io/v1
    kind: Subscription
    metadata:
      name: sample-subscription
      namespace: default
      annotations:
        apps.open-cluster-management.io/github-path: sample_app_1/dir1
        apps.open-cluster-management.io/github-branch: branch1
    spec:
      channel: default/sample-channel
      placement:
        placementRef:
          kind: PlacementRule
          name: dev-clusters
   ```

  In this example subscription, the annotation `apps.open-cluster-management.io/github-path` indicates that the subscription subscribes to all Helm charts and Kubernetes resources within the `sample_app_1/dir1` directory of the GitHub repository that is specified in the channel. The subscription subscribes to `master` branch by default. In this example subscription, the annotation `apps.open-cluster-management.io/github-branch: branch1` is specified to subscribe to `branch1` branch of the repository.

#### Adding a `.kubernetesignore` file

You can include a `.kubernetesignore` file within your GitHub repository root directory, or within the `apps.open-cluster-management.io/github-path` directory that is specified in subscription's annotations.

You can use this `.kubernetesignore` file to specify patterns of files or subdirectories, or both, to ignore when the subscription deploys Kubernetes resources or Helm charts from the repository.

You can also use the `.kubernetesignore` file for fine-grain filtering to selectively apply Kubernetes resources. The pattern format of the `.kubernetesignore` file is the same as a `.gitignore` file.

If the `apps.open-cluster-management.io/github-path` annotation is not defined, the subscription looks for a `.kubernetesignore` file in the repository root directory. If the `apps.open-cluster-management.io/github-path` field is defined, the subscription looks for the `.kubernetesignore` file in the `apps.open-cluster-management.io/github-path` directory. Subscriptions do not search in any other directory for a `.kubernetesignore` file.

#### Applying Kustomize

If there is `kustomization.yaml` or `kustomization.yml` file in a subscribed GitHub folder, kustomize is applied.

You can use `spec.packageOverrides` to override `kustomization` at the subscription deployment time.

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  name: example-subscription
  namespace: default
spec:
  channel: some/channel
  packageOverrides:
  - packageName: kustomization
    packageOverrides:
    - value: |
patchesStrategicMerge:
- patch.yaml
```

In order to override `kustomization.yaml` file, `packageName: kustomization` is required in `packageOverrides`. The override either adds new entries or updates existing entries. It does not remove existing entries.

#### Enabling GitHub WebHook

By default, a GitHub channel subscription clones the GitHub repository specified in the channel every minute and applies changes when the commit ID has changed. Alternatively, you can configure your subscription to apply changes only when the GitHub repository sends repo PUSH and PULL webhook event notifications.

In order to configure webhook in a GitHub repository, you need a target webhook payload URL and optionally a secret.

##### Payload URL

Create a route (ingress) in the hub cluster to expose the subscription operator's webhook event listener service.

  ```shell
  oc create route passthrough --service=multicluster-operators-subscription -n open-cluster-management
  ```

Then, use `oc get route multicluster-operators-subscription -n open-cluster-management` command to find the externally-reachable hostname. The webhook payload URL is `https://<externally-reachable hostname>/webhook`.

##### WebHook secret

WebHook secret is optional. Create a Kubernetes secret in the channel namespace. The secret must contain `data.secret`. See the following example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-github-webhook-secret
data:
  secret: BASE64_ENCODED_SECRET
```

The value of `data.secret` is the base-64 encoded WebHook secret you are going to use.

**Best practice:** Use a unique secret for each GitHub repository.

##### Configuring WebHook in GitHub repository

Use the payload URL and webhook secret to configure WebHook in your GitHub repository.

##### Enable WebHook event notification in channel

Annotate the subscription channel. See the following example:

```shell
oc annotate channel.apps.open-cluster-management.io <channel name> apps.open-cluster-management.io/webhook-enabled="true"
```

If you used a secret to configure WebHook, annotate the channel with this as well where `<the_secret_name>` is the kubernetes secret name containing webhook secret.

```shell
oc annotate channel.apps.open-cluster-management.io <channel name> apps.open-cluster-management.io/webhook-secret="<the_secret_name>"
```

##### Subscriptions of webhook-enabled channel

No webhook specific configuration is needed in subscriptions.
