

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
 #
 =>   Configure project templates


[EX280](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives)

