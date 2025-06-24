---
### Secure Traffic TLS
#### Three main types of route:
	
 	- Edge route
	- Pass-through route
	- Re-encrypt route


#### (1) Edge route
	
 	- clients can communicate securely through the edge route using TLS
	- certificates configured at edge route
	- communication from edge route to application is insecure

#### (2) Pass-through route
	
 	- Route passes through without any operations
	- certificates are configured only on application

#### (3) Re-encrypt route
	
 	- clients establish TLS connection to Re-encrypt route
	- TLS configured on both route and application
	- it uses different certificates for Client-To-Route and Route-To-Application communication

---

#### Certificates components:
	
 	- Private key : represent the service's identity.s
	- Public key : Handed out to the client
	- CA : 

#### Self-signed certificates
	- commonly use for internal communication in OpenShift


#### Openssl tool can be used to generate certicates:
##### PrivateKey
    openssl genrsa -des3 -out demo-ca.key 2048

##### public cert
    openssl req -x509 -new -key demo-ca.key -sha256 -days 360 -out demo-ca.pem

##### custom key for the route
    openssl genrsa -out tls.key 2048

#### generate CSR
    openssl req -new -key tls.key -out tls.csr

#### certificate singing request
    openssl x509 -req -in tls.csr -CA demo-ca.pem -CAkey demo-ca.key -CAcreateserial -out tls.crt -days 366

#### Create a project for TLS demo project
    oc new-project a-tls-demo

#### Create new app using secure certificate
    oc new-app --name=secureapp --image=bitnami/apache 
    oc get all

#### Create edge route with TLS 
    oc create route edge --service=secureapp --hostname=secureapp.apps-crc.testing --cert=tls.crt --key=tls.key

#### Verify
    curl -vk https://secureapp.apps-crc.testing
    oc describe route secureapp
    oc get route secureapp -o yaml

