
OpenShift Services:
  - Services expose applications and enable communication between system components
  - Act as a load balances for pods
  - Enable decoupling of application components
  - Load balancing and service delivery

  - It connects to pods using labels and selectors
  - Services are created on the top of deployments to provide access
  - "oc new-app" command automatically creates services resources

Types:
  - Cluster IP : default service type, stable IP and DNS within the cluster, internal pod comminication
  - NodePort : expose service with a static port on each in the cluster, allowing external to internal access
  - Load Balancer : Only available on Public Cloud platform
  - ExternalName : mapping service to DNS, useful in migration, return CNAME record with the value specified in the externalName parameter 

    
### Cluster IP example:

    apiVersion: v1
    kind: Service
    metadata: 
      name: my-service
    spec:
      selector:
        app: my-app
      ports:
        - port: 80
          targetPort: 8090
        
##### To access this "my-service" , in "default" namespace. The pattern is :  " my-service.default.svc.cluster.local "


### NodePort example:

    apiVersion: v1
    kind: Service
    metadata:
      name: my-nodeport-service
    spec:
      type: NodePort
      selector:
        app: my-app
      ports:
        - port: 80
          targetPort: 8090
          nodePort: 30000
##### The default port range for NodePort services is: 30000–32767

