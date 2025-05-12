
=> Manage Authentication and Authorization: 

    sudo dnf install httpd-tools -y                  #if htpasswd was not installed yet.

=> Configure the HTPasswd identity provider for authentication
#Generating htpasswd file and then upload to the openshift. #dnf install -y httpd-tools
    
    --------------------------------------------------------------------------------------------------
    | htpasswd -nbB <User list name> username password                                               |
    --------------------------------------------------------------------------------------------------
    --------------------------------------------------------------------------------------------------
    | htpasswd -cBb htpasswd_user_list.txt trump america_first                                       |
    | htpasswd -Bb htpasswd_user_list.txt putin black_belt_Karate-Do                                 |
    --------------------------------------------------------------------------------------------------

![Photo](https://github.com/Adrianhein/My_ex280_preparation/blob/main/images/IdentityProvider.png)


   => Create and delete users
   
Types of users in OpenShift:-

    - Regular user
    - System user
    - Services account
 
Deleting user from OpenShift steps:
 
    ##Get secret name from "openshift-config" project
    oc get secret -n openshift-config
    oc create secret generic --help

    ##Update or removed from htpasswd file for the users need to be deleted
    vim htpasswd_user_list.txt

    ##Update secret name from "openshift-config" project
    oc create secret generic <secret name> --from-file=<htpasswd_user_list.txt> --dry-run=client -o yaml -n openshift-config | oc replace -f -

    #Identify and delete the users in the openshift cluster
    oc get users
    oc delete user <user>

    #Identify and delete the "identity" in the openshift cluster
      oc get identity
      oc delete identity <identity>:<users>


   => Modify user passwords
      
    1) Update htpassd file 
    htpasswd -Bb htpasswd_user_list.txt trump america_second

    2) Get secret name from "openshift-config" project
    oc get secret -n openshift-config

    3) Update secret name from "openshift-config" project
    oc create secret generic <secret name> --from-file=<htpasswd_user_list.txt> --dry-run=client -o yaml -n openshift-config | oc replace -f -


   =>  Create and manage groups
   
      # group :=> collection of users that sharing common "ROLE"
      # There are three default virtual groups:
        - All authenticated users  = system:authenticated
        - All unauthenticated users   = system:unauthenticated
        - Users authenticated with OAuth tokens   = system:authenticated:oauth
          
      oc adm groups new <grop-name>    
      oc adm groups new <group-name> <users1 users2 users3>      
      oc adm add-users <group-name> <users4 users5>                     <<<< adding users to existing group
      oc adm policy add-role-to-group view <group-name> -n my-project   <<<< assigning group roles



   =>  Modify user and group permissions < ROLE BASE ACCESS CONTROL (RBAC) >
   
      # ROLE BASE ACCESS CONTROL (RBAC)
      # ROLE                          :=> collection of permissions which assigned to users or groups 
      # Two types of ROLE             :=>  [ Cluster RBAC and Local RBAC ]  
      # Roles defined by these verbs  :=> [ get list view watch delete update patch create ]
      # Role binding, that is connecting roles to the users or gruops

      # Default roles :
        'admin'              << Project admin access
        'basic-user'         << R-O view basic info access
        'cluster-admin'      << Full Superuser access
        'cluster-status'     << R-O to view the cluster status access
        'cluster-reader'     << R-O across cluster access
        'edit'               << can modify objects in project access
        'self-provisioner'   << can create own project access
       

       oc get clusterrolebinding.rbac                                         <<<<< to check role binding at cluster wide
       oc get rolebinding.rbac                                                <<<<< to check role binding at project level
       oc get rolebinding.rbac -n my-project                                  <<<<< to check role binding at specific project
       oc describe clusterrolebinding.rbac                                    <<<<< to check role binding at cluster wide
       oc describe rolebinding.rbac                                           <<<<< to check role binding at project level
       oc describe rolebinding.rbac -n my-project                             <<<<< to check role binding at specific project
       oc adm policy add-role-to-user <role> <user-name> -n <project>         <<<< assigning role to user
       oc adm policy remove-role-from-user <role> <user-name> -n <project>    <<<< removing role from user
       oc adm policy add-role-to-group <role> <group-name> -n <project>       <<<< assigning role to group
       oc adm policy remove-role-from-group <role> <group-name> -n <project>  <<<< removing role from group
       oc adm policy who-can <verb> <resource>                                <<<<< Listing user and grup with permission

#
=> To remove "self-provisioner" role from "system:authenticated:oauth" GROUP
#       
       oc get clusterrolebinding -o wide | (head -n 1 && grep self-provisioner)
       NAME               ROLE                         AGE   USERS    GROUPS                      SERVICEACCOUNTS
       self-provisioners  ClusterRole/self-provisioner 78d            system:authenticated:oauth                     
#
       oc adm policy remove-cluster-role-from-group <R O L E> <GROUP NAME>
       oc adm policy remove-cluster-role-from-group self-provisioner system:authenticated:oauth 
       [self-provisioner  vs  self-provisioners](https://github.com/Adrianhein/My_ex280_preparation/blob/main/Appendix)
#

https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives
