# IdM

The aeriOS Identity Manager (IdM) is crucial for managing identities during the access control process. It authenticates users, applications, and other entities, ensuring the validity of user credentials and tokens for the identity process.

aeriOS IdM is composed by two main components: 

* **Keycloak**: the component that is incharge of the  Role Based Access cControl (RBAC) 
* **OpenLDAP**: the component where the users/roles/groups are stored and federated in Keycloak.

## Getting Started / Use

The primary goal of the Identity Manager is to facilitate secure access control by managing authentication and identity verification processes effectively.

## [Docker](docker)

### How to Build, Install, or Deploy It

- [X] Prerequisites:
    - Ensure you have Docker and docker-compose installed on your system. These tools are necessary for deploying the Identity Manager.

#### Deployment Steps

1. Navigate to the Docker Directory:

    Open a terminal and change to the directory containing the Identity Manager's docker-compose file.

    ```bash
    cd identity-manager/docker
    ```

2. Start the Identity Manager:

    Execute the following command in the terminal, ensuring you are in the directory where docker-compose.yml is located.
    
    ```bash
    docker-compose up -d
    ```



### Tutorial

1. Clone the repository containing the Identity Manager's source code and configuration files.
2. Navigate to the Docker directory as described in the deployment steps.
3. Execute the `docker-compose up -d` command to start the Identity Manager service.
4. Test the Identity Manager by performing authentication tasks, ensuring it operates as expected.

## [Helm](helm)

- [X] Prerequisites:
    - Ensure you have kubectl and helm installed on your system. These tools are necessary for deploying the IdM. Also, the kubernetes context to connect to the target cluster it is necesary.

### OpenLDAP

First, it is necesary to ensure that in the Kubernetes cluster the openladp-0 PVC is not present. In order to erase it, execute the following command:
```bash
kubectl delete pvc data-openldap-0
```
Once the PVC is removed, we can proceed to install the last version of the OpenLdap component from the aeriOS common deployments repository:
```bash
helm install openldap eclipse-aerios/openldap-stack-ha --version 4.1.2 --set ltb-passwd.ingress.enabled=false --set phpldapadmin.ingress.enabled=false --set phpldapadmin.service.type=NodePort --set persistence.size=100Mi --set replication.enabled=false --set replicaCount=1 
```
This last package has been developed to deploy the default aeriOS default users. These are the default credentials of these users:
  - continuumadministrator1 - continuumadministrator1
  - dataproductowner1 - dataproductowner1
  - verticaldeployer1 - verticaldeployer1
  - aeriosuser1 - aeriosuser1
  - externaluser1 - externaluser1

In order to check the actual users/roles/groups or generate new ones, the OpenLDAP portal, phpldapadmin dashboard,  can be accessed forwarding teh port of the service and accessing :

```bash
kubectl port-forward svc/openldap-phpldapadmin 8080:80
```

Once the port is forwarded, the dashboard can be accessed via http://localhost:8080 with the following credentials:

  - user: cn=admin,dc=example,dc=org
  - pass: Not@SecurePassw0rd

In the folder OpenLDAP_templates templates for generating new users/roles/groups can be found. Once the .ldif file with the necessary data has been generated, it can be imported from the OpenLDAP dashboard.
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
  helm install idm eclipse-aerios/idm --set keycloak.service.ports.keycloak.nodePort=<nodePort> --debug
  ```

- Expose via Ingress (default ingress configuration, please check the values):

  ```bash
  helm install idm eclipse-aerios/idm --set keycloak.envVars.proxyAddressForwarding=true --set keycloak.ingress.enabled=true --debug
  ```


## Testing

### Basic Test

To verify the deployment, you can attempt to authenticate a user or entity using the Identity Manager. Ensure that the authentication process aligns with the expected access control policies.

## Credits

This module is developed and maintained as part of the aeriOS project, aiming to enhance cybersecurity measures in IoT environments.

## License

The module is distributed under the specified license. For detailed licensing information, refer to the [LICENCE.TXT](LICENCE.TXT) file in the repository.
