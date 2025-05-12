# [EX280_preparation]


[Red Hat Certified OpenShift Administrator exam: OVERVIEW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=overview)
[Red Hat Certified OpenShift Administrator exam: OBJECTIVE](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives)
[Red Hat Certified OpenShift Administrator exam: WHAT YOU NEED TO KNOW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=what-you-need-to-know)


=> [OpenShift_Local_LabEnvironment](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift_Local_LabEnv_0.2.pdf)

#
=> [OpenShift Local installation on CentOS Stream 9](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift%20Local%20installation%20on%20CentOS%20Stream%209.pdf)

#
=> [OC command not found error](https://github.com/Adrianhein/My_ex280_preparation/blob/main/If%20oc%20command%20not%20found)

#
=> [Configure HAProxy](https://github.com/Adrianhein/My_ex280_preparation/blob/main/haproxy%20configuring)
#


# Possible Infra minimum setup

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

        











