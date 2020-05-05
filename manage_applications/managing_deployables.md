# Creating and managing deployables

You can create deployables to represent Helm releases or to use as a template to wrap Kubernetes resources for deployment to target clusters for constructing or updating applications. You can create and manage subscriptions to identify, retrieve, and deploy new and updated resources to managed clusters. By using subscriptions, you can improve the continuous delivery capabilities of your application management. Samples for all resources, including deployables, are located in the [Application resource samples](app_resource_samples.md) documentation.

Learn more about deployments, then see the following tasks:

  - Create a deployable
  - Update a deployable
  - Delete a deployable
  - Promote a deployable to a channel


Deployables (`deployable.apps.open-cluster-management.io`) are Kubernetes resources that wrap or represent other resources to prevent actions from being run against the resources by Kubernetes and other controllers before the resources are placed on target clusters. By wrapping the resources, deployables can be directly deployed to one or more target clusters from the storage locations that include the deployables. When the deployables are on the target cluster or clusters, the resources are unwrapped so that required actions can then run against the resources.  

**Note:** The `deployable.apps.open-cluster-management.io` Kind is a replacement for the `Deployable.mcm.ibm.com` kind that is used in previous versions of the product.

The deployable controller acts as the default propagation engine and synchronizes local instances of the deployable. The controller follows the cluster and cluster-namespace model.

You do not need to wrap or represent all resources as deployables before you deploy the resources. Depending on the resource type and the type of channel where you promote the resource, you might not need to create a deployable for the resource. For instance, you do not need to directly create deployables for resources that are included in Helm repository and GitHub repository channels. For more information, see [Creating and managing channels](managing_channels.md). Samples for all resources, including deployables, are located in the [Application resource samples](app_resource_samples.md) documentation.

## Create a deployable

1. Compose the definition YAML content for your deployable.

2. Create the deployable within Red Hat Advanced Cluster Management for Kubernetes. You can use Kubernetes command line interface (`kubectl`) tool or REST API:

   * To use the Kubernetes CLI tool, complete the following steps:

     1. Compose and save your deployable YAML file with your preferred editing tool.
     2. Run the following command to apply your file to an apiserver. Replace `filename` with the name of your file:

        ```
        kubectl apply -f filename.yaml
        ```

        Alternatively, you can use a `create` command to create a deployable. For instance, you can use the command when you want a name generated and do not want to specifically name the deployable.
     3. Verify that your deployable is created, by running the following command:

        ```
        kubectl get Deployable
        ```

        Ensure that your new deployable is listed in the resulting output.

   * To use REST API, you need to use the [deployable POST API](../apis/mcm/deployables_app.json).

## Update a deployable

To update a deployable with a new version, you can change the deployed resource in managed clusters by changing the deployable resource in the source location on your Hub cluster. The change is started immediately with a rolling update.

1. Compose the definition updates for your deployable. For more information about the YAML structure, including the required fields, see [Application definition](#app_compose).

2. Update the definition. You can use the console, the Kubernetes command line interface (`kubectl`) tool, or REST API:

   * To use the console search to find and edit a deployable,

     1. Open the console.
     2. Click the _Search_ icon in the Header to open the _Search_ page.
     3. Within the search box, filter by `kind:deployable` to view all deployables.
     4. Within the list of all deployables, click the deployable that you want to update. The YAML for that deployable is displayed.
     5. Click **Edit** to enable editing the YAML content.
     6. When you are finished your edits, click **Save**. Your changes are saved and applied automatically.

   * To use the Kubernetes CLI tool, complete the following steps:

       1. Run the following command against the source deployable:

         ```
         kubectl edit deployables.app.ibm.com <deployableName>
         ```

       2. Update any fields or annotations that you need to change.

   * To use REST API, use the [deployable PATCH API](../apis/mcm/deployables_app.json).

When your changes are saved, the changes can be automatically detected by the channel controller for any channel that subscribes to the deployable. If the updated deployable no longer meets the channel requirements, the deployable is removed from the channel. If the deployable still meets the requirements, the updated version can be deployed to any destination clusters where the version was previously deployed.

## Delete a deployable

To delete a deployable, delete the source Kubernetes resource or Helm release from the default namespace where the resource or Helm release is stored. When you delete the source deployable, any instance of that deployable within a channel is deleted and the deployable is no longer available for deployment.

If the deployable was deployed to a managed cluster through a subscription, the deployable is not removed from the managed cluster. The deployable remains on the managed cluster until the subscription is deleted or is updated to remove or replace the subscribed deployable. When the subscription is deleted or updated, the deleted deployable is deleted from the managed clusters where it was deployed.

You can use the console, the Kubernetes command line interface (`kubectl`) tool, or REST API to delete a deployable:

* To use the console search to find and delete a deployable,

  1. Open the console.
  2. Click the _Search_ icon in the Header to open the _Search_ page.
  3. Within the search box, filter by `kind:deployable` to view all deployables.
  4. Within the list of all deployables, find the row for the deployable that you want to delete. For that row, expand the _Options_ menu and click **Delete deployable**. A confirmation window opens.
  5. Confirm the removal to delete the deployable. When the list of all deployables is refreshed, the deployable is no longer included.
  6. When the list of all deployables is refreshed, the deployable is no longer displayed.

* To use the Kubernetes CLI tool to delete a deployable, complete the following steps:

  1. Run the following command to delete the deployable from a target namespace. Replace `name` and `namespace` with the name of your deployable and your target namespace:

     ```
     kubectl delete Deployable <name> -n <namespace>
     ```

  2. Verify that your deployable is deleted by running the following command:

     ```
     kubectl get Deployable <name>
     ```

* To use REST API, use the [deployable DELETE API](../apis/mcm/deployables_app.json).

* If you want to only remove a deployable from a specific application, you can update the application to remove the content that defines the deployable. For more information about updating an application, see [Creating and managing applications](managing_apps.md).

* If you only need to remove the deployable for a specific channel, edit the subscription to no longer include the deployable. You can also change the defined annotations for the deployable to remove the deployable. If you change the annotations of the source Kubernetes resource or Helm release so that the deployable no longer meets the required annotations for the channel, the deployable is removed from the channel. A deployable that is included in a channel must continue to meet the requirements for a channel to remain in that channel. For instance, the annotations for the deployable must match the defined annotations for the channel (`spec.gate.annotations`).

## Promote a deployable to a channel {#promote-deployable}

Before a deployable can be retrieved by a subscription for deployment to a target cluster, the deployable must be included within a channel. The subscription operator only watches a subscribed channel for new and updated versions of a subscribed deployable. If the deployable is not within a channel, the deployable cannot be detected and deployed by using a subscription.

To promote a deployable to a channel, you can use either of the following methods:

* Point the deployable to a specific channel by configuring the `spec.channels` field with the correct annotations to identify the channel.
* Include the deployable in the target source location for the channel. If the deployable has the same `spec.gate.annotation` values for the channel, the deployable is promoted. In this case, the deployable does not need to point to a specific channel with the  `spec.channels` field.

For the channel, the source and `spec.gate.annotations` must be defined. For example, if a channel is pointing to the default namespace that includes a deployable, the channel controller checks whether the deployable meets the annotation requirements for the channel.
