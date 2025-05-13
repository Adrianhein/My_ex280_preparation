

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

`oc adm create-bootstrap-project-template -o yaml > project_template.yaml`

`less project_template.yaml`

    apiVersion: template.openshift.io/v1
    kind: Template
    metadata:
      creationTimestamp: null
      name: project-request
    objects:
    - apiVersion: project.openshift.io/v1
      kind: Project
      metadata:
        annotations:
          openshift.io/description: ${PROJECT_DESCRIPTION}
          openshift.io/display-name: ${PROJECT_DISPLAYNAME}
          openshift.io/requester: ${PROJECT_REQUESTING_USER}
        creationTimestamp: null
        name: ${PROJECT_NAME}
      spec: {}
      status: {}
    - apiVersion: rbac.authorization.k8s.io/v1
      kind: RoleBinding
      metadata:
        creationTimestamp: null
        name: admin
        namespace: ${PROJECT_NAME}
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: admin
      subjects:
      - apiGroup: rbac.authorization.k8s.io
        kind: User
        name: ${PROJECT_ADMIN_USER}
     ### Add here "Sample_Template_Resource_Definition.yaml" entries ###
     ### Add here "Sample_Template_Resource_Definition.yaml" entries ###
     ### Add here "Sample_Template_Resource_Definition.yaml" entries ###
    parameters:
    - name: PROJECT_NAME
    - name: PROJECT_DISPLAYNAME
    - name: PROJECT_DESCRIPTION
    - name: PROJECT_ADMIN_USER
    - name: PROJECT_REQUESTING_USER


less Sample_Template_Resource_Definition.yaml

    - apiVersion: v1
      kind: ResourceQuota
      metadata:
       name: $(PROJECT_NAME)-quota
      spec:
       hard: 
         cpu: "2"
         memory: 4Gi
         pods: "10"
    - apiVersion: v1
      kind: LimitRange
      metadata:
       name: $(PROJECT_NAME)-limit-range
       namespace: default
      spec:
       limits:
         - default:
            memory: 1Gi
            cpu: "200m"
           defaultRequest:
            memory: 256Mi
            cpu: "10m"
           type: Container

#
Then:

`oc create -f project_template.yaml -n openshift-config`

    [sysadm@openshift-local-01 ~]$ oc create -f project_template.yaml -n openshift-config
    template.template.openshift.io/project-request created
    [sysadm@openshift-local-01 ~]$ 


Then:

`oc edit projects.config.openshift.io/cluster`  # Add project template inside "spec: {}" block

    spec:
      projectRequestTemplate:
        name: project-request
  #  
    [sysadm@openshift-local-01 ~]$ oc edit projects.config.openshift.io/cluster
    project.config.openshift.io/cluster edited
    [sysadm@openshift-local-01 ~]$ 


`oc get pod -n openshift-apiserver -w`

    [sysadm@openshift-local-01 ~]$ oc get pod -n openshift-apiserver
    NAME                        READY   STATUS    RESTARTS   AGE
    apiserver-b4859c89f-66qkf   2/2     Running   0          2m36s
    [sysadm@openshift-local-01 ~]$ 

`oc new-project a-test`

`oc get quota -n a-test`

`oc describe limitrange -n a-test`

See the result:

        [sysadm@openshift-local-01 ~]$ oc new-project a-test
        Now using project "a-test" on server "https://api.crc.testing:6443".

        You can add applications to this project with the 'new-app' command. For example, try:

            oc new-app rails-postgresql-example

        to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

            kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

        [sysadm@openshift-local-01 ~]$ oc get quota
        NAME           AGE   REQUEST                               LIMIT
        a-test-quota   8s    cpu: 0/2, memory: 0/4Gi, pods: 0/10   
        [sysadm@openshift-local-01 ~]$ oc describe limitrange
        Name:       a-test-limit-range
        Namespace:  a-test
        Type        Resource  Min  Max  Default Request  Default Limit  Max Limit/Request Ratio
        ----        --------  ---  ---  ---------------  -------------  -----------------------
        Container   cpu       -    -    10m              200m           -
        Container   memory    -    -    256Mi            1Gi            -
        [sysadm@openshift-local-01 ~]$ 


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

