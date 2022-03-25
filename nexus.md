
# Nexus Outline Design

## Requirement
The artefact storage solution needs to provide highly available, recoverable artefact storage for Value streams, application teams and the Zeus pipeline service via a simple to use/manage process whilst reducing the manual overhead required by the DevOps CoE team. 

## Infrastructure
Nexus will be deployed in Aurora FMO in an Active/Passive model (LON02/LON06) behind an F5 load balancer. 

FMO provisioning building block along with Ansible playbooks will be used to ensure the deployment is fully automated. 
F5 configuration and certificates will need to be generated outside of this process.

Failover will be configured using F5 liveness probes and also via manual F5 switchover for patching and maintenance windows. Replicator will need to be executed and confirmed successful prior to any leg switchover to ensure replication has been successful.

Postgres SQL database replication will be used to ensure config applied to "Active" application node is federated to the "Hot standby" application node. As the databases are in different locations.

DECISION: 
- Transaction Log Shipping (File or streaming)
- Trigger Based Master-Standby Replication

Backups TBC (Is replication cross site enough or is backup required)

CPU,MEM,Initial Disk Allocations TBC

## Authentication
Authentication will be implemented utilising SSO via SAML/ADFS. This will simplify the onboarding, access and ongoing utilisation for the consumers of the service, allowing them to use Service Now pre-existing methods to manage membership of groups for access requirement changes.

Interaction Model TBC - BUCF Engagement

## Management

##### Onboarding
Application onboarding will be handled by the Zeus service onboarding process currently in design phase > *LINK*. The current recommended approach is that for each application there are two active directory GG groups created per application for Read/Write and these groups will be mapped to repository access using the SAML integration.

A building block will be created/utilised to apply GitOps driven config from the onboarding repository and apply the necessary config changes.

Example nexus.yaml:
```
repositories:
	- type: raw
	  retention: 6 (default 4)
	- type: docker
	  retention: 3 (default 4)
```

Access groups and owner information can then be derived from the higher level onboarding.yaml file as an ongoing source of truth and to apply RBAC in Nexus.

##### Repository Management
To simplify the management of Nexus repositories and reduce storage sprawl housekeeping and tagging will be used to ensure only RTL artefacts are replicated between datacentres and retained in storage. This will ensure that ephemeral packages and artefacts created by CI are only retained for their useful lifecycle and all immutable packages/artefacts are stored and replicated to be highly available based on a pre agreed policy (n-4 or similar). This will be implemented using group repositories for each app and repository type with a number of repositories behind to handle the different artefact types.

##### Additional Reporting
Additional reporting will be configured at both an application and web server level to show repository usage and access endpoint retrievals.

Reporting jobs will be used to monitor quota usage of the repositories and either notify the repository owner that they are close to threshold or restrict write access to the repository until housekeeping has occurred.
