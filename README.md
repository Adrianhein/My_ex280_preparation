
![](https://img.shields.io/badge/OpenShift-platform-red?logo=RedHatOpenShift&style=flat)

![](https://img.shields.io/badge/RedHatOpenShiftAdministration-EX280K-red)


![Red Hat Certified OpenShift Administrator exam: OVERVIEW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=overview)

    

The Red Hat Certified OpenShift Administrator exam (EX280) tests the knowledge, skills, and ability to create, configure, and manage a cloud application platform using Red Hat OpenShift Container Platform.

By passing this exam, you become a Red Hat Certified OpenShift Administrator that also counts towards earning a Red Hat Certified Architect (RHCA®).

Objectives listed for this exam are based on the most recently released version of the exam. Once you have purchased the exam you may have older versions available.
Audience for this exam

    System and Software Architects who need an understanding of the features and functionality of an OpenShift Container Platform cluster
    System Administrators who need to support the initial establishment of an OpenShift cluster
    Cluster Operators who need to support ongoing maintenance of an OpenShift cluster
    Site Reliability Engineers who need to support the ongoing maintenance and troubleshooting of an OpenShift cluster
    System administrators who want to demonstrate their OpenShift Container Platform skills
    Red Hat Certified Engineers who wish to become a Red Hat Certified Architect (RHCA)
    System administrators or developers who are working in a DevOps environment using Red Hat OpenShift Container Platform

Prerequisites for this exam

Candidates for this exam should:

    Have taken Red Hat System Administration I (RH124) or have comparable experience. Red Hat Certified System Administrator (RHCSA) is strongly recommended but not required
    Have taken Red Hat OpenShift Administration I: Containers & Kubernetes (DO180) course or have comparable work experience using OpenShift Container Platform
    Have taken Red Hat OpenShift Administration II: Operating a Production Kubernetes Cluster (DO280) course or have comparable work experience using OpenShift Container Platform
    Review the Red Hat Certified OpenShift Administrator exam (EX280) objectives
    Experience with container technology is recommended
    Take our free assessment to find the course that best supports your preparation for this exam

Build your skills path

Take this course as part of a Red Hat Learning Subscription, which gives you on-demand, unlimited access to our online learning resources for an entire year.
Find out how
Verify your knowledge

Take a free skills assessment to test your expertise, determine gaps and get recommendations for where to start with Red Hat training.
Assess your skills
Red Hat logo
LinkedIn
YouTube
Facebook
X
Products & portfolios

    Red Hat AI
    Red Hat Enterprise Linux
    Red Hat OpenShift
    Red Hat Ansible Automation Platform
    Cloud services
    See all products

Tools

    Training and certification
    My account
    Customer support
    Developer resources
    Find a partner
    Red Hat Ecosystem Catalog
    Documentation

Try, buy, & sell

    Product trial center
    Red Hat Store
    Buy online (Japan)
    Console

Communicate

    Contact sales
    Contact customer service
    Contact training
    Social

About Red Hat

Red Hat is an open hybrid cloud technology leader, delivering a consistent, comprehensive foundation for transformative IT and artificial intelligence (AI) applications in the enterprise. As a trusted adviser to the Fortune 500, Red Hat offers cloud, developer, Linux, automation, and application platform technologies, as well as award-winning services.

    Our companyHow we workCustomer success storiesAnalyst relationsNewsroomOpen source commitmentsOur social impactJobs

Select a language

    About Red Hat
    Jobs
    Events
    Locations
    Contact Red Hat
    Red Hat Blog
    Inclusion at Red Hat
    Cool Stuff Store
    Red Hat Summit

© 2025 Red Hat, Inc.

    Privacy statement
    Terms of use
    All policies and guidelines
    Digital accessibility
    Cookie preferences



[Red Hat Certified OpenShift Administrator exam: OBJECTIVE](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=objectives)

[Red Hat Certified OpenShift Administrator exam: WHAT YOU NEED TO KNOW](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam?section=what-you-need-to-know)

#
=> 01 [OpenShift_Local_LabEnvironment](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift_Local_LabEnv_0.2.pdf)

=> 02 [OpenShift Local installation on CentOS Stream 9](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OpenShift%20Local%20installation%20on%20CentOS%20Stream%209.pdf)

=> 03 [Configure HAProxy](https://github.com/Adrianhein/My_ex280_preparation/blob/main/haproxy%20configuring)

=> 04 [OC command not found error](https://github.com/Adrianhein/My_ex280_preparation/blob/main/If%20oc%20command%20not%20found)
#
=> 05 

Manage OpenShift Container Platform with [oc command](https://github.com/Adrianhein/My_ex280_preparation/blob/main/OC_Command_Usage.md)

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

=> 09 []()

=> 10 []()

#


--------------------------------------------------------------------------------------------------------------------------------------------
#  Possible Infra minimum Setup
--------------------------------------------------------------------------------------------------------------------------------------------

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









