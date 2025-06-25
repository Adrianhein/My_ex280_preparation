
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
### Deleteing
    oc delete sa my-service-account-name

---



