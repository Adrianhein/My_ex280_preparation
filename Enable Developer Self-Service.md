

Project Quotas

#
Terminal View

    [sysadm@openshift-local-01 ~]$ oc get quota
    No resources found in default namespace.
    [sysadm@openshift-local-01 ~]$ oc get quota -n openshift-host-network
    NAME                            AGE   REQUEST                                                              LIMIT
    host-network-namespace-quotas   78d   count/daemonsets.apps: 0/0, count/deployments.apps: 0/0, pods: 0/0   limits.cpu: 0/0, limits.memory: 0/0
    [sysadm@openshift-local-01 ~]$ 
    [sysadm@openshift-local-01 ~]$ oc get quota host-network-namespace-quotas -n openshift-host-network
    NAME                            AGE   REQUEST                                                              LIMIT
    host-network-namespace-quotas   78d   count/daemonsets.apps: 0/0, count/deployments.apps: 0/0, pods: 0/0   limits.cpu: 0/0, limits.memory: 0/0
    [sysadm@openshift-local-01 ~]$ 
    [sysadm@openshift-local-01 ~]$ oc describe quota host-network-namespace-quotas -n openshift-host-network
    Name:                   host-network-namespace-quotas
    Namespace:              openshift-host-network
    Resource                Used  Hard
    --------                ----  ----
    count/daemonsets.apps   0     0
    count/deployments.apps  0     0
    limits.cpu              0     0
    limits.memory           0     0
    pods                    0     0
    [sysadm@openshift-local-01 ~]$ 

WEB UI View
![Photo](https://github.com/Adrianhein/My_ex280_preparation/blob/main/images/ResourceQuota.png)
![Photo](https://github.com/Adrianhein/My_ex280_preparation/blob/main/images/ResourceQuota_1.png)

#
#
=> Default value when we will create quota:
![Photo](https://github.com/Adrianhein/My_ex280_preparation/blob/main/images/create%20quota%20default%20value.png)


    apiVersion: v1
    kind: ResourceQuota
    metadata:
      name: example
      namespace: default
    spec:
      hard:
        pods: '4'
        requests.cpu: '1'
        requests.memory: 1Gi
        limits.cpu: '2'
        limits.memory: 2Gi

#
#
 =>   Configure cluster resource quotas

    oc create clusterresourcequota -h
    oc create clusterresourcequota for-user --project-annotation-selector openshift.io/requester=<USER-NAME> --hard pods=10 --hard secrets=10
    oc create clusterresourcequota limit-bob --project-annotation-selector=openshift.io/requester=<USER-NAME> --hard=pods=10

    oc describe clusterresourcequota
    oc describe appliedclusterresourcequota -n <PROJECT NAME>


 =>   Configure project quotas
 #
 =>   Configure project resource requirements
 #
=>   Configure project limit ranges
`Default value when we will create LimitRange`

    apiVersion: v1
    kind: LimitRange
    metadata:
      name: mem-limit-range
      namespace: default
    spec:
      limits:
        - default:
            memory: 512Mi
          defaultRequest:
            memory: 256Mi
          type: Container

   `oc get limits`
   `oc create -f <file name>.yaml -n <project name>`
   `oc describe limits <limitrange name> -n <project name>`
 
 #
 =>   Configure project templates
#
=> The difference between ResourceQuota and ClusterResourceQuota 

🔹 ResourceQuota (Standard Kubernetes)

🔸 ClusterResourceQuota (OpenShift-only)

    | ------------------------------ | --------------- | ------------------------------ |
    | Feature                        | `ResourceQuota` | `ClusterResourceQuota`         |
    | ------------------------------ | --------------- | ------------------------------ |
    | Scope                          | Namespace-only  | Cluster-wide (multi-namespace) |
    | Available in Kubernetes        | ✅ Yes           | ❌ No                         |
    | Available in OpenShift         | ✅ Yes           | ✅ Yes                        |
    | Based on label selector        | ❌ No            | ✅ Yes                        |
    | Applies to multiple namespaces | ❌ No            | ✅ Yes                        |
    | ------------------------------ | --------------- | ------------------------------ |


[EX280](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives)

