# IdM

The aerOS Identity Manager (IdM) is crucial for managing identities during the access control process. It authenticates users, applications, and other entities, ensuring the validity of user credentials and tokens for the identity process.

aerOS IdM is composed by two main components: 

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


## Testing

### Basic Test

To verify the deployment, you can attempt to authenticate a user or entity using the Identity Manager. Ensure that the authentication process aligns with the expected access control policies.

## Credits

This module is developed and maintained as part of the aerOS project, aiming to enhance cybersecurity measures in IoT environments.

## License

The module is distributed under the specified license. For detailed licensing information, refer to the [LICENCE.TXT](LICENCE.TXT) file in the repository.
