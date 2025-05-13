

=>Opsnshift administrator want to check for "oc" client helps you develop, build, deploy, and run your applications on any OpenShift or Kubernetes cluster. It also includes the administrative commands for managing a cluster under the 'adm' subcommand.
    
    --------------------------------------------------------------------------------------------------
    | oc help                                                                                        |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to identify for WEB console URL
    
    --------------------------------------------------------------------------------------------------
    | oc get routes -n openshift-console                                                             |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc whoami --show-console                                                                       |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check for Administrative Commands Actions for administering an OpenShift cluster
    
    --------------------------------------------------------------------------------------------------
    | oc adm --help                                                                                  |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator to check the status of the namespace
    
    --------------------------------------------------------------------------------------------------
    | oc status -h                                                                                   |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc status -n <name of project>                                                                 |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc status -n openshift-console                                                                 |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check for all status of the namespaces
    
    --------------------------------------------------------------------------------------------------
    | oc status -A                                                                                   |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check information about the current session
    
    --------------------------------------------------------------------------------------------------
    | oc whoami                                                                                      |
    --------------------------------------------------------------------------------------------------


=>Opsnshift administrator want to create new project for task
    
    --------------------------------------------------------------------------------------------------
    | oc new-project <project name>                                                                  |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to create new application for deployment inside the specific project
    
    --------------------------------------------------------------------------------------------------
    | oc new-app --name=<app name> --image=image_repositories -n <project name>                      |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | #oc new-app --name=my-app --image=openshift/hello-openshift -n <project name>                  |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check for logs
    
    --------------------------------------------------------------------------------------------------
    | oc logs                                                                                        |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs --help                                                                                 |
    --------------------------------------------------------------------------------------------------

#Logs
oc logs -h

#bc=> build config
=>Opsnshift administrator want to check for logs 
    
    --------------------------------------------------------------------------------------------------
    | oc logs -f bc/<bc-name>                                                                        |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    |oc logs -f bc/<bc-name> --version=<version-name>                                                |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs -f deployment/<deployment-name>                                                        |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs -f deployment/<deployment-name> --version=<version-name>                               |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs -f pod/<pod-name>                                                                      |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc get nodes                                                                                   |
    | oc adm node-logs <NODE-NAME>                                                                   |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc get nodes                                                                                   |
    | oc adm node-logs --role master                                                                 |
    | oc adm node-logs --role master -u kublet                                                       |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | #To debug inside the CRC node                                                                  |
    | oc get nodes                                                                                   |
    | oc debug nodes/crc -n default                                                                  |
    --------------------------------------------------------------------------------------------------


=>Opsnshift administrator want to check for logs under "deployment"
    
    --------------------------------------------------------------------------------------------------
    | oc get deployments -n <project name>                                                           |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs deployment/<app name Or deployment name> -n <project name>                             |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs -f deployment/<app name Or deployment name> -n <project name>                          |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check for logs under "pod"
    
    --------------------------------------------------------------------------------------------------
    | oc get pod -n <project name>                                                                   |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs pod/<pod name> -n <project name>                                                       |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | oc logs -f pod/<pod name> -n <project name>                                                    |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to print the supported API resources on the server.
    --------------------------------------------------------------------------------------------------
    | oc api-resources                                                                               |
    --------------------------------------------------------------------------------------------------


=>Opsnshift administrator want to do "Querying, Filtering"
#oc get all
#oc get deployment -n <project name> -o wide
#oc get deployment -n <project name> -o yam
#oc describe deployment -n <project name>
#oc get deployments -o custom-columns=NAME:.metadata.name,RSRC:.metadata.labels,NAMESPACE:.metadata.namespace,STATUS:.status.phase,REPLICAS:.spec.replicas

#oc describe pods -n <project name>

#oc get deployments -o custom-columns=NAME:.metadata.name,RSRC:.metadata.labels,NAMESPACE:.metadata.namespace,STATUS:.status.phase,REPLICAS:.spec.replicas
#oc get pods -o custom-columns=NAME:.metadata.name,RSRC:.metadata.labels,NAMESPACE:.metadata.namespace,STATUS:.status.phase,REPLICAS:.spec.replicas

#oc get pods -l deployment=<app name>
#oc describe pods my-app-name | grep IP

#oc describe pods my-app-name

#oc get pods my-app-name -o yaml

#oc get pods --field-selector=status.phase=Running


=>Opsnshift administrator want to import/export the yaml configs
#Importing => oc apply -f my-app.yaml
#Exporting => oc get deployment my-app -o yaml > exported-my-app.yaml
oc get secret -n openshift-congif

#Exporting => #oc extract secret/<secret name> -n openshift-config --to=./backup --confirm
#Importing => #oc replace -f ./backup/<secret name> -n openshift-config
#Importing => #oc create secret generic <secret name> --from-file=htpasswd=<secret name> --dry-run=client -o yaml -n openshift-config | oc replace -f -



#Container image stream = >
oc get imagestream -n openshift
oc describe imagestream <IMAGE_NAME> -n openshift
oc get image sha256:b80a514f136f738736d
oc describe image sha256:b80a514f136f738736d


#Create project and app
oc new-project my-project-name --description='My New Project' --display-name='My Test'
oc new-app --name=my-app --image=openshift/hello-openshift
oc describe project my-project-name

#Delete project
oc delete project my-project-name


oc get deployments
oc get deployments -n openshift-apiserver
oc get apiserver

oc get pods --all-namespaces

## To list all the nodes in the clutser
oc get nodes
oc get componentstatus


#OC ADM command
oc adm
oc adm top nodes 
oc adm top nodes --sort-by=cpu
oc adm top pods 
oc adm top pods --namespaces=my-namespace
oc adm top pods -l app=my-app-name

=>Opsnshift administrator want to access for health checking inside OpenShift cluster
    
    --------------------------------------------------------------------------------------------------
    |oc get nodes                                                                                    |
    |oc desctibe nodes                                                                               |
    |oc get componentstatus                                                                          |
    |oc get pods -n openshift-etcd                                                                   |
    |oc -n openshift-etcd rsh etcd-crc                                                               |
    |#> Inside the etcd-pod, check with "etcdctl --help" for the status                              |
    --------------------------------------------------------------------------------------------------

=>Opsnshift administrator want to check the OPERATORS which is key component of the OpenShift clster that handle the lifecycle management of the various resources and services

#As an Opsnshift administrator, regularly need to check these below status:
    
    --------------------------------------------------------------------------------------------------
    |oc get clusteroperators                                                                         |
    |oc describe clusteroperators                                                                    |
    |oc version                                                                                      |
    --------------------------------------------------------------------------------------------------








