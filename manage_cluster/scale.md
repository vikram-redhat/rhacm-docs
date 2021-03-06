# Resizing a cluster before creating

You can customize your cluster specifications, such as virtual machine sizes and number of nodes. See the following list of recommended settings for each available provider, but also see the documentation for more specific information:

* [Amazon Web Services](#amazon-web-services)
* [Google Cloud Platform](#google-cloud-platform)
* [Azure Kubernetes Service](#azure-kubernetes-service)

## Amazon Web Services

See the Amazon documentation at [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) for more details.

After the cluster is created, you can resize your cluster to increase or decrease the number of nodes in that cluster. To learn how to resize your cluster, refer to [Creating a MachineSet to scale your cluster](https://docs.openshift.com/container-platform/4.1/machine_management/creating-machineset.html).

## Google Cloud Platform

After the cluster is created, you can resize your cluster to increase or decrease the number of nodes in that cluster. To learn how to resize your cluster, refer to Google Kubernetes Engine [Resizing a cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/resizing-a-cluster) documentation.

## Azure Kubernetes Service

For more information, see [General purpose virtual machine sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-general).

For more information or customization, see [Ephemeral OS disks for Azure VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ephemeral-os-disks).

After the cluster is created, you can resize your cluster to increase or decrease the number of nodes in that cluster. To learn how to resize your cluster, refer to [Scale the node count in an Azure Kubernetes Service (AKS) cluster](https://docs.microsoft.com/en-us/azure/aks/scale-cluster).
