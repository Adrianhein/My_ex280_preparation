
## ServiceAccount Overview
	* allow components such as pods to access the API directly
	* it controls API access without using any orhers user credentials
	* service account format: 'system:serviceaccount:project:name'
	* Every service account has to groups:
		' system:serviceaccounts ' include all service accounts
		' system:serviceaccounts:<project-name> ' include all service accounts in the specific project

## Default service accounts:
	- builder = for building PODs with "system:image-builder" role
	- deployer = for deploying PODs with "system:deployer" role
	- default = use by other PODs unless specificed
	- all service accounts = for ' system:image-puller ' role for image pulling
	
	
### Creating via CLI

        oc create sa my-service-account-name -n <project_name> 

### Creating via YAML definition my-service-account-name.yaml

      apiVersion: v1
      kind: ServiceAccount
      metadata:
       name: my-service-account-name
       namespace: <project_name> 
###

    oc apply -f my-service-account-name.yaml
    oc describe sa my-service-account-name

### Granting premission to service account 
     oc policy add-role-to-user <role_name> -z <service_account_name>
     oc describe sa my-service-account-name 
### Deleteing service account 
    oc delete sa my-service-account-name

---

## SCC - security context constraints

- controlling pod permission within OpenShif 
- determin pod actions and resources access

- default SCC created during OpenShift installation
- additional SCC can be introuducted by installing Operators
- custom SCC can also be used OpenShift CLI

### Best Practices:
 - avoiding modifying default SCCs
 - use custom SCCs to suit specific requirements
 - if they meet your requirement, use defult SCCs instead.


### Managing SCCs via CLI
    oc create scc <scc_name>
    oc get scc
    oc describe scc <scc_name>
    oc edit scc <scc_name>

### PODs SCC
    - PODs default to restriced SCC
    - SCC determined by ServiceAccount and Group
    - To check pod SCC 
      oc get pod $POD_NAME -o yaml | grep openshift.io/scc
    
### Demo
    oc new-project a-scc-demo
    oc new-app docker.io/httpd
    oc get pod
    oc status --suggest
    oc whoami
    
    [admin@openshift-local ~]$ oc status --suggest
    Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
    In project a-scc-demo on server https://api.crc.testing:6443
    
    svc/httpd - 10.217.5.25:80
      deployment/httpd deploys istag/httpd:latest 
        deployment #2 running for about a minute - 0/1 pods (warning: 3 restarts)
        deployment #1 deployed about a minute ago - 0/1 pods growing to 1
    
    Errors:
      * pod/httpd-68d648c9b7-dpk8c is crash-looping
    
        The container is starting and exiting repeatedly. This usually means the container is unable
        to start, misconfigured, or limited by security restrictions. Check the container logs with
        
          oc logs httpd-68d648c9b7-dpk8c -c httpd
        
        Current security policy prevents your containers from being run as the root user. Some images
        may fail expecting to be able to change ownership or permissions on directories. Your admin
        can grant you access to run containers that need to run as the root user with this command:
        
          oc adm policy add-scc-to-user anyuid -n a-scc-demo -z default
        
    
    Info:
      * deployment/httpd has no liveness probe to verify pods are still running.
        try: oc set probe deployment/httpd --liveness ...
    
    View details with 'oc describe <resource>/<name>' or list resources with 'oc get all'.
    [admin@openshift-local ~]$ 
    [admin@openshift-local ~]$ oc create sa my-demo-sa
    serviceaccount/my-demo-sa created
    [admin@openshift-local ~]$ oc adm policy add-scc-to-user anyuid -z my-demo-sa 
    clusterrole.rbac.authorization.k8s.io/system:openshift:scc:anyuid added: "my-demo-sa"
    [admin@openshift-local ~]$ oc get deployment
    NAME    READY   UP-TO-DATE   AVAILABLE   AGE
    httpd   0/1     1            0           4m4s
    [admin@openshift-local ~]$ oc patch deployment/httpd  --patch '{"spec":{"template":{"spec":{"serviceAccountName":"my-demo-sa"}}}}'
    deployment.apps/httpd patched
    [admin@openshift-local ~]$ oc get pods
    NAME                     READY   STATUS    RESTARTS   AGE
    httpd-65f8cf94c5-zdj2t   1/1     Running   0          17s
    [admin@openshift-local ~]$ oc status
    Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
    In project a-scc-demo on server https://api.crc.testing:6443
    
    svc/httpd - 10.217.5.25:80
      deployment/httpd deploys istag/httpd:latest 
        deployment #3 running for 30 seconds - 1 pod
        deployment #2 deployed 5 minutes ago
        deployment #1 deployed 5 minutes ago

    1 info identified, use 'oc status --suggest' to see details.
    [admin@openshift-local ~]$ 
    
    
    
   ### OR you can set like this below 'set' command instead of using PATCH command: 
   ##### oc patch deployment/httpd  --patch '{"spec":{"template":{"spec":{"serviceAccountName":"my-demo-sa"}}}}'
    
    oc set sa deployment/httpd my-demo-sa
    

### CHECK SCC STATUS : OC GET SCC -O WIDE

    [admin@openshift-local ~]$ oc get scc -o wide
    NAME                              PRIV    CAPS                   SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY     READONLYROOTFS   VOLUMES
    anyuid                            false   <no value>             MustRunAs   RunAsAny           RunAsAny    RunAsAny    10           false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    hostaccess                        false   <no value>             MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","hostPath","persistentVolumeClaim","projected","secret"]
    hostmount-anyuid                  false   <no value>             MustRunAs   RunAsAny           RunAsAny    RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","hostPath","nfs","persistentVolumeClaim","projected","secret"]
    hostnetwork                       false   <no value>             MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    hostnetwork-v2                    false   ["NET_BIND_SERVICE"]   MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    hostpath-provisioner              true    <no value>             RunAsAny    RunAsAny           RunAsAny    RunAsAny    <no value>   false            ["*"]
    machine-api-termination-handler   false   <no value>             MustRunAs   RunAsAny           MustRunAs   MustRunAs   <no value>   false            ["downwardAPI","hostPath"]
    nonroot                           false   <no value>             MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    nonroot-v2                        false   ["NET_BIND_SERVICE"]   MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    privileged                        true    ["*"]                  RunAsAny    RunAsAny           RunAsAny    RunAsAny    <no value>   false            ["*"]
    restricted                        false   <no value>             MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    restricted-v2                     false   ["NET_BIND_SERVICE"]   MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <no value>   false            ["configMap","csi","downwardAPI","emptyDir","ephemeral","persistentVolumeClaim","projected","secret"]
    [admin@openshift-local ~]$ 
    
---
#

---

## Privileged Tips for application running

	- OpnenShift denies running containers as ROOT by default
	- Restricted SCC used by default for PODs
	- SA or Group determines POD's SCC

### Security Risk Running as ROOT 
	- Higher privileges pose security risks
	- containers running as 'root' common in community images
	- Malicious code could escalate host node permissions

### Metigate Security Risk running as non-ROOT containers
	- Bitnami, RedHat, Google, VMWare offer non-ROOT containers
	- Non-ROOT containers run with non-root user for adding security
	- Tailored for specific purposes (e.g Nginx server)
	- Principle of least privilege limits security breach impact

### Port binding comparism between Non-ROOT and ROOT containers
	- Non-ROOT containers bind port numbers range is greater than '1024' by default
	- Privileged ports below '1024' requires ROOT privileges
	- OpenShift intelligently handles port binding for non-ROOT containers, that external routes use default port as 80 while internal services access with Openshift requires using specific higher ports


### Demo
#### oc new-project a-privileged-demo
#### oc new-app bitnami/apache
#### oc get all
#### oc expose svc/apache
#### oc get svc
#### oc get svc -o wide
#### oc get route
#### oc delete project a-privileged-demo


        [admin@openshift-local ~]$ oc new-project a-privileged-demo
        Now using project "a-privileged-demo" on server "https://api.crc.testing:6443".
        
        You can add applications to this project with the 'new-app' command. For example, try:
        
            oc new-app rails-postgresql-example
        
        to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:
        
            kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname
        
        [admin@openshift-local ~]$ oc new-app bitnami/apache
        --> Found container image 8ff97bd (3 weeks old) from Docker Hub for "bitnami/apache"
        
            * An image stream tag will be created as "apache:latest" that will track this image
        
        --> Creating resources ...
            imagestream.image.openshift.io "apache" created
            deployment.apps "apache" created
            service "apache" created
        --> Success
            Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
             'oc expose service/apache' 
            Run 'oc status' to view your app.
        [admin@openshift-local ~]$ oc get all
        Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
        NAME                         READY   STATUS              RESTARTS   AGE
        pod/apache-86c8c877d-pk244   0/1     ContainerCreating   0          5s
        
        NAME             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE
        service/apache   ClusterIP   10.217.5.41   <none>        8080/TCP,8443/TCP   8s
        
        NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
        deployment.apps/apache   0/1     1            0           8s
        
        NAME                                DESIRED   CURRENT   READY   AGE
        replicaset.apps/apache-85744c8677   1         0         0       8s
        replicaset.apps/apache-86c8c877d    1         1         0       5s
        
        NAME                                    IMAGE REPOSITORY                                                                   TAGS     UPDATED
        imagestream.image.openshift.io/apache   default-route-openshift-image-registry.apps-crc.testing/a-privileged-demo/apache   latest   5 seconds ago
        [admin@openshift-local ~]$ 
        [admin@openshift-local ~]$ 
        [admin@openshift-local ~]$ oc expose svc/apache
        route.route.openshift.io/apache exposed
        [admin@openshift-local ~]$ 
        [admin@openshift-local ~]$ oc get svc
        NAME     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE
        apache   ClusterIP   10.217.5.41   <none>        8080/TCP,8443/TCP   24s
        [admin@openshift-local ~]$ 
        [admin@openshift-local ~]$ oc get svc -o wide
        NAME     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE   SELECTOR
        apache   ClusterIP   10.217.5.41   <none>        8080/TCP,8443/TCP   29s   deployment=apache
        [admin@openshift-local ~]$ 
        [admin@openshift-local ~]$ oc get route
        NAME     HOST/PORT                                   PATH   SERVICES   PORT       TERMINATION   WILDCARD
        apache   apache-a-privileged-demo.apps-crc.testing          apache     8080-tcp                 None
        [admin@openshift-local ~]$oc delete project a-privileged-demo
	project.project.openshift.io "a-privileged-demo" deleted
	






