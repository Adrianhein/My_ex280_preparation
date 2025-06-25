
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

### SCC - security context constraints

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
    
    


