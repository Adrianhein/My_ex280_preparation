
---

" self-provisioner " vs " self-provisioners " can be confusing, but here's the clear difference:

    | ------------------- | ----------- | ------------------------------------------------ |
    | Term                | Type        | Purpose                                          |
    | ------------------- | ----------- | ------------------------------------------------ |
    | `self-provisioner`  | ClusterRole | Grants project creation permissions              |
    | `self-provisioners` | Group       | Default group of users allowed to self-provision |
    | ------------------- | ----------- | ------------------------------------------------ |


self-provisioner (Cluster Role)
#You assign this role to a user or group to enable project creation.

    oc adm policy add-cluster-role-to-user self-provisioner leader

#self-provisioners (Group Name)
#You can remove users from this group to prevent them from creating projects.

    oc adm policy remove-cluster-role-from-group self-provisioner self-provisioners

---

" deployment (Singular) " vs " deployments (Plural) "

In OpenShift (and Kubernetes), the terms deployment and deployments refer to the same resource type, but are used differently depending on context:

Example:

    #It typically refers to one resource.
    oc get deployment my-app

    #It’s the pluralized form used by the OpenShift/Kubernetes API.
    oc describe deployments



    | --------------|--------------------- | -----------------------------------------|
    | Term          | Context              | Purpose                                  |
    | ------------- | -------------------- | ---------------------------------------- |
    | `deployment`  | YAML or CLI (single) | Refers to a specific Deployment resource |
    | `deployments` | Listing or querying  | Refers to all Deployment resources       |
    | --------------| -------------------- | -----------------------------------------|

---


Different between  openshift-etcd-operator and openshift-etcd


🔹 openshift-etcd-operator

    Purpose: This is the operator responsible for managing the lifecycle of the etcd cluster in OpenShift.

    Namespace: openshift-etcd-operator

    Responsibilities:

        Deploying and managing etcd pods

        Monitoring etcd health and scaling (when applicable)

        Performing rolling upgrades

        Handling backup/restore coordination

        Reconciling etcd configuration based on cluster state

This operator is part of the Cluster Version Operator (CVO) system that ensures the etcd cluster is always in the desired state.



🔹 openshift-etcd

    Purpose: This is the namespace where the actual etcd pods and their resources (ConfigMaps, PVCs, etc.) live.

    Namespace: openshift-etcd

    Contains:

        etcd-0, etcd-1, etcd-2 pods (typically one per control plane node)

        Supporting secrets, configmaps, and possibly PVCs for data storage

        The etcd service that other control plane components use

This namespace is where the running etcd servers (the distributed key-value store that underpins Kubernetes and OpenShift) reside.

    | ------------------------- | ------------------------ | ---------------------------------------------------- |
    | Namespace                 | Component                | Role                                                 |
    | ------------------------- | ------------------------ | ---------------------------------------------------- |
    | `openshift-etcd-operator` | `etcd-operator` Pod      | Manages etcd deployment & lifecycle (the controller) |
    | `openshift-etcd`          | `etcd-0`, `etcd-1`, etc. | Actual etcd servers running as pods (the database)   |
    | ------------------------- | ------------------------ | ---------------------------------------------------- |


    [sysadm@openshift-local-01 ~]$ oc get componentstatus 
    Warning: v1 ComponentStatus is deprecated in v1.19+
    NAME                 STATUS    MESSAGE   ERROR
    controller-manager   Healthy   ok        
    scheduler            Healthy   ok        
    etcd-0               Healthy   ok        
    [sysadm@openshift-local-01 ~]$ 

    [sysadm@openshift-local-01 ~]$ oc get pods -n openshift-etcd-operator
    NAME                            READY   STATUS    RESTARTS   AGE
    etcd-operator-b45778765-928mm   1/1     Running   2          5d3h
    [sysadm@openshift-local-01 ~]$ 

    [sysadm@openshift-local-01 ~]$ oc get pods -n openshift-etcd
    NAME       READY   STATUS    RESTARTS   AGE
    etcd-crc   5/5     Running   10         5d3h
    [sysadm@openshift-local-01 ~]$ 
    [sysadm@openshift-local-01 ~]$ oc -n openshift-etcd rsh etcd-crc
    sh-5.1# 
    sh-5.1# whoami
    root
    sh-5.1# ls
    192.168.126.11:0  afs  bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
    sh-5.1# 
    sh-5.1# etcdctl --help
    sh-5.1# etcdctl endpoint status
    https://192.168.126.11:2379, d44fc94b15474c4c, 3.5.17, 76 MB, true, false, 11, 1243461, 1243461, 
    sh-5.1# 
    sh-5.1# exit
    exit
    [sysadm@openshift-local-01 ~]$ 

---

enable disable monitoring

    https://github.com/Adrianhein/rhel-notes/blob/main/OpenShift%20crc%20monitoring%20enable-disable.md

---

### On OpenShift Server

#### To verify api url on OpenShift server

    oc whoami --show-server
#### Generate crt file OpenShift Server Like "openshift-ca.crt" 
    echo | openssl s_client -showcerts -connect api.crc.testing:6443 </dev/null 2>/dev/null | openssl x509 -outform PEM > openshift-ca.crt

#

### On Client RHEL
### Place "openshift-ca.crt" on Client Linux System

    cd /etc/pki/ca-trust/source/anchors/openshift-ca.crt
    update-ca-trust 

### Then maping "IP of OpenShift and api.crc.testing" in '/etc/hosts' 
    
    192.168.122.10 api.crc.testing  #example

### Try login

    oc login https://api.crc.testing:6443 --username=<user> --password=<password>
    oc status

---

### Most recent created project on 

    oc get projects --sort-by=.metadata.creationTimestamp | tail -n 2

    OR
    
    oc get projects --sort-by=.metadata.creationTimestamp -o custom-columns="NAME:.metadata.name,CREATED:.metadata.creationTimestamp" | tail -n 2


    oc get projects --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-5].metadata.name}{"\n"}'

---

### Port forwarding to quick verify

    oc port-forward <app-name> 8080:8080

---

#

---

### OC    - OpenShift CLI (Command Line Interface) 
### HPA   - Horizaontal Pod Autoscaling
### RS    - Replica Set
### SA    - Service Account
### SCC   - Security Context Constraints
### RBAC  - Roll Based Access Control
### PV    - Persisten Volume 
### PVC   - Persisten Volume Claim
### SV    - Service
### CM    - Config Map
### CRDs  - Custom Resource Definition
### OLM   - Operator Lifecycle Manager









