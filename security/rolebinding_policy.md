# Rolebinding policy

Apply a rolebinding policy to bind a policy to a namespace in your cluster.

## Rolebinding policy YAML structure


   ```yaml
   apiVersion: policy.mcm.ibm.com/v1alpha1
   kind: Policy
   metadata:
     name: policy-rolebinding
     namespace:
   spec:
     complianceType:
     remediationAction:
     namespaces:
       exclude:
       include:
     object-templates:
       - complianceType:
         objectDefinition:
           kind: RoleBinding # role binding must exist
           apiVersion: rbac.authorization.k8s.io/v1
           metadata:
             name: operate-pods-rolebinding
           subjects:
           - kind: User
             name: admin # Name is case sensitive
             apiGroup:
           roleRef:
             kind: Role #this must be Role or ClusterRole
             name: operator # this must match the name of the Role or ClusterRole you wish to bind to
             apiGroup: rbac.authorization.k8s.io
       ...
   ```

## Rolebinding policy table 

<!--place holder until i update the table with the appropriate parameters-->
|Field|Description|
|-- | -- |
| apiVersion | Required. Set the value to `policy.mcm.ibm.com/v1alpha1`. <!--current place holder until this info is updated--> |
| kind | Required. Set the value to `Policy` to indicate the type of policy. |
| metadata.name | Required. The name for identifying the policy resource. |
| metadata.namespaces | Optional. |
| spec.namespace | Required. The namespaces within the hub cluster that the policy is applied to. Enter parameter values for `include`, which are the namespaces you want to apply to the policy to. `exclude` specifies the namespaces you explicitly do not want to apply the policy to. **Note**: A namespace that is specified in the object template of a policy controller, overrides the namespace in the corresponding parent policy.|
| remediationAction | Optional. Specifies the remediation of your policy. The parameter values are `enforce` and `inform`. **Important**: Some policies may not support the enforce feature.|
| disabled | Required. Set the value to `true` or `false`. The `disabled` parameter provides the ability to enable and disable your policies.|
| spec.complianceType | Required. Set the value to `"musthave"`|
| spec.object-template| Optional. Used to list any other Kubernetes object that must be evaluated or applied to the managed clusters. |
{: caption="Table 1. Required and optional definition fields" caption-side="top"}

## Rolebinding policy sample

Apply a role binding policy to bind a policy to a namespace in your cluster. Your role binding policy might resemble the following YAML file:

   ```yaml
   apiVersion: policy.mcm.ibm.com/v1alpha1
   kind: Policy
   metadata:
     name: policy-rolebinding
     namespace: mcm
   spec:
     complianceType: musthave
     remediationAction: inform
     namespaces:
       exclude: ["kube-*"]
       include: ["default"]
     object-templates:
       - complianceType: musthave
         objectDefinition:
           kind: RoleBinding # role binding must exist
           apiVersion: rbac.authorization.k8s.io/v1
           metadata:
             name: operate-pods-rolebinding
           subjects:
           - kind: User
             name: admin # Name is case sensitive
             apiGroup: rbac.authorization.k8s.io
           roleRef:
             kind: Role #this must be Role or ClusterRole
             name: operator # this must match the name of the Role or ClusterRole you wish to bind to
             apiGroup: rbac.authorization.k8s.io
       ...
   ```
<!--the following section will be moved to the task page create_rb_pol.md when it is created-->

### Apply a rolebinding policy

Complete the following steps to apply the role binding policy from the console:

1. Log in to your Red Hat Advanced Cluster Management for Kubernetes console
2. From the navigation menu, click **Govern risk**. 
3. Click **Create policy**. 
4. Select **Rolebinding** from the _Specifications_ field.
