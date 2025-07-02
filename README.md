
![](https://img.shields.io/badge/OpenShift-platform-red?logo=RedHatOpenShift&style=flat)

![](https://img.shields.io/badge/RedHatOpenShiftAdministration-EX280K-red)

---

![Red Hat Certified OpenShift Administrator exam: OVERVIEW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=overview)

[Red Hat Certified OpenShift Administrator exam: OBJECTIVE](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives)

[Red Hat Certified OpenShift Administrator exam: WHAT YOU NEED TO KNOW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=what-you-need-to-know)

---
## => 01 [OpenShift_Local_LabEnvironment](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift_Local_LabEnv_0.2.pdf)
#
#### => 02 [OpenShift Local installation on CentOS Stream 9](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift%20Local%20installation%20on%20CentOS%20Stream%209.pdf)
#
#### => 03 [Configure HAProxy](https://github.com/Adrianhein/My_ex280_preparation/blob/main/haproxy%20configuring)
#
## => 04 [OC command not found error](https://github.com/Adrianhein/My_ex280_preparation/blob/main/If%20oc%20command%20not%20found)
#
#### => 05 

## Manage OpenShift Container Platform with [oc command](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OC_Command_Usage.md)

    - Use the web console to manage and configure an OpenShift cluster
    - Use the command-line interface to manage and configure an OpenShift cluster
    - Query, format, and filter attributes of Kubernetes resources
    - Import, export, and configure Kubernetes resources
    - Locate and examine container images
    - Create and delete projects
    - Examine resources and cluster status
    - View logs
    - Monitor cluster events and alerts
    - Assess the health of an OpenShift cluster
    - Troubleshoot common container, pod, and cluster events and alerts
    - Use product documentation
#
=> 06 

Manage Authentication and Authorization with [oc command and web Ui](https://github.com/Adrianhein/My_ex280_preparation/blob/main/Manage%20Authentication%20and%20Authorization.md)

    - Configure the HTPasswd identity provider for authentication
    - Create and delete users
    - Modify user passwords
    - Create and manage groups
    - Modify user and group permissions

#
=> 07 

Enable Developer Self-Service with [oc command and web Ui](https://github.com/Adrianhein/My_ex280_preparation/blob/main/Enable%20Developer%20Self-Service.md)

    - Configure cluster resource quotas
    - Configure project quotas
    - Configure project resource requirements
    - Configure project limit ranges
    - Configure project templates
#

=> 08 []()

Configure Network Security

    - Configure networking components
    - Troubleshoot software defined networking
    - Create and edit external routes
    - Control cluster network ingress
    - Secure external and internal traffic using TLS certificates
    - Configure application network policies
#

=> 09 []()

Manage OpenShift Operators

    - Install an operator
    - Delete an operator

#

=> 10 []()

Configure Application Security

    - Configure and manage service accounts
    - Run privileged applications
    - Create service accounts
    - Manage and apply permissions using security context constraints
    - Create and apply secrets to manage sensitive information
    - Configure application access to Kubernetes APIs
    - Configure Kubernetes CronJobs

---

#  Possible Infra minimum Setup

Create 6 "blank" VMs (or use BareMetal) for OpenShift itself (don't install an OS on this just yet)

    3 Masters Nodes
        4 vCPUs
        16 GB RAM
        120 GB HD
    2 Workers ( 2 VMs also fine)
        2 vCPUs
        8 GB RAM
        120 GB HD
    1 Boostrap (you remove this after the install is done)
        4 vCPUs
        16 GB RAM
        120 GB HD


Create 2 VMs for the LoadBalancer (can use Centos/Fedora/RHEL)

    LoadBalancer VM
        2 vCPUs
        4 GB RAM
        30 GB HD

Create 1 VM for DNS (can use Centos/Fedora/RHEL)

    DNS VM
        2 vCPUs (if using IDM use 4 vCPUs)
        4 GB RAM
        30 GB HD (if Using IDM set this to 50GB HD)
        =>can also use this for your Webserver too
        =>can install the DHCP components here too

Create 1 VM for Local Container Registry (can use Centos/Fedora/RHEL)

        4 vCPUs
        4 GB RAM
        500 GB HD

Create 1 VM for Git Server (can use Centos/Fedora/RHEL)

        4 vCPUs
        8 GB RAM
        1000 GB HD

Possible Infra minimum setup requirement for fresh OpenShift Cluster

        -------------------------------------------------
        |Master Node 01| 4 vCPUs | 16 GB RAM | 120 GB HD|
        |Master Node 02| 4 vCPUs | 16 GB RAM | 120 GB HD|
        |Master Node 03| 4 vCPUs | 16 GB RAM | 120 GB HD|
        -------------------------------------------------
        |Worker Node 01| 2 vCPUs | 8 GB RAM | 120 GB HD |
        |Worker Node 02| 2 vCPUs | 8 GB RAM | 120 GB HD |
        |Worker Node 03| 2 vCPUs | 8 GB RAM | 120 GB HD |
        -------------------------------------------------
        |Boostrap Node | 2 vCPUs | 16 GB RAM | 120 GB HD|
        -------------------------------------------------
        |LoadBalancer  | 2 vCPUs | 4 GB RAM | 30 GB HD  |
        -------------------------------------------------
        |DNS Host      | 2 vCPUs | 4 GB RAM | 30 GB HD  |
        -------------------------------------------------
        |Registry Host | 4 vCPUs | 4 GB RAM | 500 GB HD |
        -------------------------------------------------
        |Git Host      | 4 vCPUs | 8 GB RAM | 1000 GB HD|
        -------------------------------------------------
        |Jump Host     | 4 vCPUs | 4 GB RAM | 30 GB HD  |
        -------------------------------------------------


--------------------------------------------------------------------------------------------------
Useful URLs      
- [link 1](https://github.com/mgonzalezo/RedHat_ex280) - [link 2](https://www.udemy.com/course/openshift-administration/?couponCode=KEEPLEARNING) - [link 3.1](https://www.youtube.com/watch?v=250cIQ9hY64&list=PLnFCwVWiQz4nE1Y4UPRzJdN9DReAoobMn) - [link 3.2](https://www.youtube.com/watch?v=V69I-M1AXs4&list=PLnFCwVWiQz4kuAEO3lgGiSkjJtEZSAEtd) - [link 4](https://github.com/christianh814/openshift-toolbox/tree/master)
--------------------------------------------------------------------------------------------------









