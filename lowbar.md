# Technology Optimisation - Minimum Bar

This document describes the minimum requirements for an application/deployment to use the DevOps CoE Zeus Pipeline containing TO App Automation. Onboarding to the required tools is out of scope for this document but will be covered in separate linked pages.

## Condensed Checklist
- **Source code management**
	- Value stream organisation exists
	- Application repository exists
	- Source code & Config in a Github repository
	- Github repository configured with CODEOWNERS and Branch protection.
- **Artefact Storage**
	- Onboarded to Nexus
	- Binary and packaged applications stored in Nexus repo
- **Secrets**
	- Onboarded to Hashicorp Vault (all necessary environments)
	- Secrets published to vault 
	- Service/Privileged accounts stored in Cyberark safe.
- **Security Scanning**
	- Onboarded to Sonarqube
	- Sonarqube project configured.
	- Onboarded to NexusIQ
	- Exceptions list checked and updated.

## Source Code Management
All application code and associated config must be stored in a GitHub repository - https://ghe.service.group. Where the application is not source code based an application repository should still be used to house the configuration required for application automation. The repository should reside in an "Organisation" for the applications Value stream. 

N.B. No binary files should be stored in a Github repository!

### Repository configuration.
All repositories must have a CODEOWNERS file detailing the associated users/groups that must provide their approval for changes to the files in the repository. Details of how to use a CODEOWNERS file can be found on the DevOps CoE Github space here: LINK. This must be combined with Github branch protection detailing the necessary approvals required for the application. This can be customised to give the application owners the levels of control they require but as a minimum must include minimum 2 person peer review from overall owner or CODEOWNERS appropriate member not including the person who created the Pull Request.

For additional information/training on the use of Github refer to the Github guides here: LINK.

### Required files
There are a few required files that need to be created in the repository in additional to the application code to enable tracking, reporting and automation to happen in a controlled way.

##### CODEOWNERS
As detailed above a CODEOWNERS file is required to ensure the necessary people in the team have approval on changes to files/folder within the repo as part of the Pull request process which triggers the deployment. This file should reside in the root of the repository file structure.

##### manifest.yaml
This file is required to enable the automated deployment of the application. The full syntax and structure of this file is detailed here: LINK. This will evolve inline with additional content being created and patterns being made available. As a minimum bar only the infrastructure and application identifier information sections are required.

##### lbgmeta.yaml
This file is utilised by the data aspect of automation deployment and MUST exist for every repo created to allow reporting and tracking on the utilisation of automation and pipelines with Github.
```
Example:

    app: my_app
    components:
	    - id: AL12345
	      classification: confidential
	      accountable-owner: group@lloydsbanking.com
	      version: 0.1.0
	      type: netframework48
```
## Artefact storage
All binary artefacts and compiled source code must be stored in an application specific repository in Nexus. Onboarding to this repository can be achieved by referring to this LINK. This is achieved in many ways depending on the type of application. 

##### External Binary Package/Installer:
This type of package would need to be ingested into Nexus via the group OSG process LINK.

##### Non Containerised Packaged applications
All non-container based applications must be stored in an application specific RAW type repository.

## Secrets management
To comply with Group security standards all secrets and service account details must be stored in their associated approved stores. For secrets this will be Hashicorp Vault, onboarding process can be found here: LINK. For service accounts or other privileged accounts this will be Cyberark, process for onboarding can be found here: LINK

## Security Scanning
If the application is source code based then the following scans must be completed prior to publishing a packaged artefact to Nexus. This will happen as part of the pipeline and will need the relevant sections in the manifest.yaml.

Sonarqube: Quality & CVE
	LINK
NexusIQ: Open Source Dependency Scanning
	LINK
	ONBOARDING_LINK
Aquasec: Containerised applications
	LINK
