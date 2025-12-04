# IdM

The aerOS Identity Manager (IdM) is crucial for managing identities during the access control process. It authenticates users, applications, and other entities, ensuring the validity of user credentials and tokens for the identity process.

aerOS IdM is composed by two main components: 

* **Keycloak**: the component that is incharge of the  Role Based Access cControl (RBAC) 
* **OpenLDAP**: the component where the users/roles/groups are stored and federated in Keycloak.

## Getting Started / Use

The primary goal of the Identity Manager is to facilitate secure access control by managing authentication and identity verification processes effectively.

## [Helm](helm)

- [X] Prerequisites:
    - Ensure you have kubectl and helm installed on your system. These tools are necessary for deploying the IdM. Also, the kubernetes context to connect to the target cluster it is necesary.

### OpenLDAP

First, it is necesary to ensure that in the Kubernetes cluster the openladp-0 PVC is not present. In order to erase it, execute the following command:
```bash
kubectl delete pvc data-openldap-0
```
Once the PVC is removed, we can proceed to install the last version of the OpenLdap component from the aerOS common deployments repository:
```bash
helm install openldap aeros-common/openldap-stack-ha --version 4.1.2 --set ltb-passwd.ingress.enabled=false --set phpldapadmin.ingress.enabled=false --set phpldapadmin.service.type=NodePort --set persistence.size=100Mi --set replication.enabled=false --set replicaCount=1 
```
This last package has been developed to deploy the default aerOS default users. These are the credentials of these users:
  - continuumadministrator1 - VDw6!x7!8^O(
  - dataproductowner1 - 0oa9g44Nf/EM
  - verticaldeployer1 - i,4a3z(W1JNf
  - aerosuser1 - iI3O^Nk9V£66
  - externaluser1 - 031V;5Cfg]i

In order to check the actual users/roles/groups or generate new ones, the OpenLDAP portal, phpldapadmin dashboard,  can be accessed forwarding teh port of the service and accessing :

```bash
kubectl port-forward svc/openldap-phpldapadmin 8080:80
```

Once the port is forwarded, the dashboard can be accessed via http://localhost:8080 with the following credentials:

  - user: cn=admin,dc=example,dc=org
  - pass: Not@SecurePassw0rd

In the folder [OpenLDAP_templates](https://gitlab.aeros-project.eu/wp3/t3.4/idm/-/tree/develop/OpenLDAP_templates) templates for generating new users/roles/groups can be found. Once the .ldif file with the necessary data has been generated, it can be imported from the OpenLDAP dashboard.
### Keycloak

⚠️**Warning**
> 
> If Keycloak will be exposed behind a reverse proxy (e.g. through a K8s Ingress), you must set the PROXY_ADDRESS_FORWARDING env var to *true*

⚠️**Warning**
> 
> Take into consideration that Keycloak must be reachable by the user's browser when accessing to the Management Portal. Therefore, it can be exposed through a K8s ingress, 
through a NodePort or exposing the Docker container port to the host machine. It will depend on the specific needs or network configuration of the pilot.


- Expose via NodePort:

  ```bash
  helm install idm aeros-common/idm --set keycloak.service.ports.keycloak.nodePort=<nodePort> --debug
  ```

- Expose via Ingress (default ingress configuration, please check the values):

  ```bash
  helm install idm aeros-common/idm --set keycloak.envVars.proxyAddressForwarding=true --set keycloak.ingress.enabled=true --debug
  ```


## Testing

### Basic Test

To verify the deployment, you can attempt to authenticate a user or entity using the Identity Manager. Ensure that the authentication process aligns with the expected access control policies.

## Credits

This module is developed and maintained as part of the aerOS project, aiming to enhance cybersecurity measures in IoT environments.

## License

The module is distributed under the specified license. For detailed licensing information, refer to the [LICENCE.TXT](LICENCE.TXT) file in the repository.
