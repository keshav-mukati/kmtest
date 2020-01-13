Initial Author
------------------

Jarosław Gajewski

Changelog
------------------

| Version | Date | User | Changes |
|---------|------|------|---------|
| 0.1 | 20.11.2019 | Jarosław Gajewski | Base version | 
| 0.2 | 05.01.2020 | Piotr Lewandowski | review | 


- [1.	Introduction](#1introduction)
  - [1.1.	Purpose](#11purpose)
  - [1.2.	Audience](#12audience)
  - [1.3.	Scope](#13scope)
  - [1.4.	Related Documents](#14related-documents)
        - [Table 1 ATLM Related Documents](#table-1-atlm-related-documents)
  - [1.5.	Requirement Levels](#15requirement-levels)
- [2.	Architecture Overview](#2architecture-overview)
        - [Figure 1 DHC Architecture Overview](#figure-1-dhc-architecture-overview)
        - [Figure 2 Cloud Assembly Logical Overview](#figure-2-cloud-assembly-logical-overview)
  - [2.1.	Business and Solution Requirements](#21business-and-solution-requirements)
        - [Table 2 Initial Requirements](#table-2-initial-requirements)
  - [2.2.	Tenancy](#22tenancy)
    - [2.2.1.	Multi-tenancy](#221multi-tenancy)
        - [Figure 3 Multi-Tenancy](#figure-3-multi-tenancy)
        - [Table 3 Design Decisions - Multi-tenancy](#table-3-design-decisions---multi-tenancy)
    - [2.2.2.	Departmental Segregation](#222departmental-segregation)
        - [Figure 4 Departmental Segregation](#figure-4-departmental-segregation)
        - [Table 4 Design Decisions - Departmental Segregation](#table-4-design-decisions---departmental-segregation)
- [3.	Detailed Logical Design](#3detailed-logical-design)
  - [3.1.	Management Topology](#31management-topology)
    - [3.1.1. VMware Cloud Assembly Service](#311-vmware-cloud-assembly-service)
    - [3.1.2. VMware Service Broker](#312-vmware-service-broker)
    - [3.1.3. VMware Code Stream](#313-vmware-code-stream)
        - [Table 5 Design Decisions - Management Topology](#table-5-design-decisions---management-topology)
  - [3.2. CAS Communication flow](#32-cas-communication-flow)
        - [TO BE UPDATED Figure 5 CAS Communication Flow](#to-be-updated-figure-5-cas-communication-flow)
  - [3.3. CAS Data model](#33-cas-data-model)
        - [TO BE UPDATED Figure 6 CAS Data Model](#to-be-updated-figure-6-cas-data-model)
  - [3.4.	VMware Cloud Assembly Logical Components](#34vmware-cloud-assembly-logical-components)
    - [3.4.1. Tags](#341-tags)
        - [Design Decisions -Tags](#design-decisions--tags)
        - [Figure 7 CAS Tags application flow](#figure-7-cas-tags-application-flow)
  - [Come back to that and recheck](#come-back-to-that-and-recheck)
        - [Table 6 CAS Standard Tags](#table-6-cas-standard-tags)
        - [Table 7 DHC Standard Tags](#table-7-dhc-standard-tags)
    - [3.4.2.	Cloud Proxy](#342cloud-proxy)
    - [3.4.3.	Cloud Account](#343cloud-account)
    - [3.4.4.	Cloud Zone](#344cloud-zone)
    - [3.4.5. Mappings and Profiles](#345-mappings-and-profiles)
    - [3.4.6.	Projects](#346projects)
        - [Figure 8 CAS Data Model](#figure-8-cas-data-model)
    - [3.4.7. Integrations](#347-integrations)
  - [3.5.	Security](#35security)
    - [3.5.1.	Role Based Access Control](#351role-based-access-control)
        - [Table 8 Design Decisions � RBAC](#table-8-design-decisions--rbac)
    - [3.5.2.	Firewall](#352firewall)
        - [Table 9 Design Decisions - Firewall](#table-9-design-decisions---firewall)
  - [3.6.	Availability and Scalability](#36availability-and-scalability)
    - [3.6.1.	Availability Design](#361availability-design)
    - [3.6.2.	Scalability Design](#362scalability-design)
  - [3.7.	Recoverability](#37recoverability)
- [4.	Detailed Physical Design](#4detailed-physical-design)
  - [4.1.	Management Plane](#41management-plane)
    - [4.1.1.	Virtual Machine Configuration table](#411virtual-machine-configuration-table)
        - [Table 10 VMs list](#table-10-vms-list)
        - [Table 11 Configuration details VM XYZ](#table-11-configuration-details-vm-xyz)
  - [4.2.	Security](#42security)
    - [4.2.1.	Role Based Access Control](#421role-based-access-control)
        - [Table 12 CAS Global Organization Roles](#table-12-cas-global-organization-roles)
        - [Table 13 DHC CAS Global Organization Roles](#table-13-dhc-cas-global-organization-roles)
        - [Table 14 Project Administrator Permissions](#table-14-project-administrator-permissions)
        - [Table 15 Project Member Permissions](#table-15-project-member-permissions)
        - [Table 16 DHC CAS Project Roles](#table-16-dhc-cas-project-roles)
        - [Table 17 RBAC Groups](#table-17-rbac-groups)
    - [4.2.2.	Firewall](#422firewall)
        - [Table 18 Firewall Rules](#table-18-firewall-rules)
  - [4.3.	Availability and Scalability](#43availability-and-scalability)
    - [4.3.1.	Availability](#431availability)
        - [Table 19 Availability details](#table-19-availability-details)
    - [4.3.2.	Scalability](#432scalability)
        - [Table 20 Scalability details](#table-20-scalability-details)
- [5 Service Request capabilities](#5-service-request-capabilities)
  - [5.1 First day request capabilities](#51-first-day-request-capabilities)
  - [5.2 Second day request capabilities](#52-second-day-request-capabilities)

# 1.	Introduction

## 1.1.	Purpose
The purpose of this document is to provide detailed design and architectural guidance required to implement validated model of a automation and orchestration in accordance with Atos standards and portfolio services. The principal aim of this document is to translate the high-level design (HLD) into a technical low-level design (LLD).
Design is providing component architecture overview in Architecture Overview chapter that provides basic building blocks and main principles, followed by Detailed Logical Design.
Architecture Overview provides basic building blocks and main design principles of presented design. It is covering known requirements cascaded from HLD and other LLDs.
Detailed Logical Design presents business logic, relations and fundamental design decisions.
Detailed Physical Design provides detailed configuration of components including POD type specifics.

## 1.2.	Audience
This document is intended for Atos Cloud Services Engineers and Architects responsible for Digital Hybrid Cloud (DHC) solution implementation and maintenance.

## 1.3.	Scope
This LLD is intended to cover below components and domains:
1.	vRealize Automation and vRealize Automation Cloud
 
a.	Definition of CMDB model of automation services

 b.	Structured data model and store (with tags for automated provisioning)

 c.	Policy definition to use tags for intelligent workload placement

 d.	Multi tenancy

 e.	Lifecycyle Management (LCM) and Release management planning with for consumed Software as a Service (SaaS) components

 f.	Version control integration (MUST BE ADDRESSED IN DHC 1.0.0)

[comment]: <> (Piotr Lewandowski - was it?)

2.	vRealize Automation and vRealize Automation Cloud :

 a.	VMware vCenter on premise


 b.	Software Defined Network (SDN)

 c.	Version control

This LLD is not covering:

1.	Management and Base Virtualization
2.	Network
3.	Hybrid Cloud Extension (HCX)
4.	Monitoring
5.	Pivotal Container Services (PKS)
6.	Management Infrastructure Automation
7.	External services integration (like. ServiceNow etc)
8.	ITSM integration (WILL BE COVERED IN DHC 1.0.0)
9.	Reporting & Metering
10.	Customer Onboarding (WILL BE COVERED IN DHC 1.0.0)

## 1.4.	Related Documents
This document is a subset of Atos Technology Lifecycle Management (ATLM) artefacts. All documents are stored on DHC code repository

|Global documentation location|
| ---|
|https://git.atosone.com/private-cloud/dhc-documentation|

##### Table 1 ATLM Related Documents

 | Document Number | Document Name |
 | :---: | --- |
 | TBD | DHC High-Level Design |
 | lldSddcVmwareCloudFoundation | Low Level Design - SDDC VMware Cloud Foundation |
 | lldSoftwareDefinedNetworks | Low Level Design - Software Defined Networks |
 | lldMonitoring | Low Level Design - Monitoring |
 | MSD-S28-0000 | DHC Pivotal Container Services LLD |

## 1.5.	Requirement Levels
This document is following the principles below to categorize all requirements and design decisions.

| Term | Meaning |
| --- | --- |
| MUST | The definition is an absolute requirement of the specification. |
| MUST NOT | The definition is an absolute prohibition of the specification |
| SHOULD | There may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course |
| SHOULD NOT | There may exist valid reasons in particular circumstances when the particular behavior is acceptable or even useful, but the full implications should be understood, and the case carefully weighed before implementing any behavior described with this label |
| MAY | Any design decisions that are not classified as MUST and SHOULD or covering optional feature that is not general available for DHC product |

# 2.	Architecture Overview
DHC will be using VMware Cloud Automation Service (CAS) to provide the blueprinting and orchestration functionality of the platform as well as being the presentation layer for the functions to be consumed via API. VMware CAS is an overarching term for Cloud Assembly, Code Stream and Service Broker. This functionality can be delivered via a SaaS offering further allowing the underlying DHC automation software stack to be as light as possible.
The diagram below highlights the areas of the DHC architecture in scope of this LLD (yellow).

##### Figure 1 DHC Architecture Overview

![Figure 1](images/lldCloudAutomationServices/figure1.svg)

##### Figure 2 Cloud Assembly Logical Overview

![Figure 2](images/lldCloudAutomationServices/figure2.svg)

## 2.1.	Business and Solution Requirements
The table below provides known requirements mandatory to be incorporated into design decisions of VMware Cloud Automation described in this LLD.

##### Table 2 Initial Requirements

| ID | Requirement description | Requirement Source | Requirement Level |
| --- | --- | --- | --- |
| R001 | Design frontend Automation & orchestration for customer payload | HLD | MUST |
| R002 | Design blueprints in CAS for DHC VM Provisioning | HLD |	MUST |
| R003 | Solution is using VMware Cloud Foundation SDDC and SDN workload domains / PODs as integration end points | HLD | MUST |
| R004 | Installation and configuration of required components is automated | Atos Management | SHOULD |
| R005 | Automation domain must be patched in regular schedule with minimal impact into service availability | Portfolio | MUST |
| R006 | Defined Role Base Access Control (RBAC) model to ensure a proper security isolation | Portfolio | MUST |
| R007 | Define Multi-Tenancy and multi departmental model to ensure resource segregation for the different legal entities of the same customer | HLD | MUST |

## 2.2.	Tenancy
DHC is by design a single Customer, multi segregated department capable solution. DHC is providing tenancy for resource and rights segregation of defined customer.

### 2.2.1.	Multi-tenancy

##### Figure 3 Multi-Tenancy
![Figure 3](images/lldCloudAutomationServices/figure3.svg) #MUST BE UPDATED

##### Table 3 Design Decisions - Multi-tenancy

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS001 | Physical isolation of compute and storage resources using dedicated workload domains for each Tenant of the same DHC Customer | This will satisfy the multi-tenancy requirements offered by the DHC solution | NA |
| CAS002 | Multiple CAS Organizations will be used to have multi-tenancy capability for same customer | This will satisfy the multi-tenancy requirements offered by the DHC solution | Compute and storage resources are dedicated for each tenant in DHC environment |
| CAS003 | Multiple Projects within same Organization will be used to deliver departmental segregation capability for same customer | This will satisfy the departmental segregation requirements offered by the DHC solution | Compute and storage resources can be shared in a shared DHC environment |
| CAS004 | Full resource and traffic isolation can be implemented between tenants | | NA |
| CAS005 | External systems integration require to be inline with tenancy decision for automation and SDDC platform | Strong consistency is required in therms of data exchange and components discovery to avoid split brain condition after modification on any component level |

### 2.2.2.	Departmental Segregation

##### Figure 4 Departmental Segregation
![Figure 4](images/lldCloudAutomationServices/figure4.svg)

##### Table 4 Design Decisions - Departmental Segregation

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS006 | Logical isolation of compute and storage resources for each department via Projects | This will satisfy the departmental segregation requirements offered by the DHC solution | Resource saturation can be introduced in resource sharing model. As result single department can impact performance of other department services |
| CAS007 | Multiple Projects within same Organization will be used to have departmental segregation capability for same customer | This will satisfy the departmental segregation requirements offered by the DHC solution | Compute and storage resources are shared in a shared DHC environment |
| CAS008 | Full resource and traffic isolation can be implemented between | Dedicated clusters must be deployed to guarantee explicit storage anc compute power. |
| CAS009 | External systems integration require to be inline with departamental segregation decision for automation and SDDC platform | Strong consistency is required in therms of data exchange and components discovery to avoid split brain condition after modification on any component level | Monitoring needs to be aligned with this approach |

# 3.	Detailed Logical Design

## 3.1.	Management Topology

### 3.1.1. VMware Cloud Assembly Service
DHC uses VMware Cloud Assembly to deliver blueprinting capabilities and unified provisioning through declarative Infrastructure as Code on all supported platforms.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS010 | At least single Cloud Proxy virtual appliance is deployed per workload domain | This will facilitate the interface for integration of entire automation services with endpoints (SDDC & SDN) | N/A |
| CAS011 | VMware Cloud Assembly service will be used for blueprinting and orchestration | This will facilitate the blueprinting and unified provisioning across different endpoints (SDDC, SDN Workload Domains) | N/A |
| CAS012 | VMware Cloud Assembly Console will be operational interface for DHC Cloud Operations Team to manage the customer workload and its component mappings | This will facilitate the single console for DHC Cloud Operations team to manage SDDC and SDN Components | NA |

### 3.1.2. VMware Service Broker
VMware Service Broker provides self-service access to multi-cloud infrastructure and application resources from a single catalog, without requiring disparate tools.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| SB001 | VMware Service Broker will be used as frontend for customer and third party integrations | Unified view for service catalog is mandatory | proper RBAC capabilities must be included in product |
| SB002 | Catalog functionality can be consumed via GUI, API or third party plugins | Flexibility in delivery model | N/A |
| SB003 | Customer Operations (CO) or integration team is responsible for service catalog view and management. | DHC R&D is not responsible by design for service catalog | CO or integration teams must be trained. R&D must define RACI |
| SB004 | First and second day requests are available in Service Broker | Portfolio requirement | N/A |

### 3.1.3. VMware Code Stream
Code Stream automates the code and application release process with a comprehensive set of capabilities for application deployment, testing, and troubleshooting.

##### Table 5 Design Decisions - Management Topology

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CS001 | VMware Code Stream is used as DHC pipeline automation tool for testing and release preparation | Simplicity and native integration into VMware SDDC and automation stack | N/A |
| CS002 | VMware Code Stream is not exposed to customer | Not part of current DHC release | N/A |

## 3.2. CAS Communication flow

##### TO BE UPDATED Figure 5 CAS Communication Flow
![Figure 5](images/lldCloudAutomationServices/figure5.svg)#TO BE UPDATED
- CAS (SaaS Platform) � CAS Platform will be managed by vendor (VMware). All the DHC Customer VM / Network objects will be discovered/managed by CAS Platform via CAS Cloud Proxy (Data Collector) using REST over HTTPS
- CAS Cloud Proxy Server � CAS Cloud Proxy Server will be hosted on WLD vCenter Server which will enable local communication channel (HTTPS) and mediator between CAS Platform and WLD vCenter / SDN Endpoint
- SDDC Endpoint (WLD vCenter) � CAS Cloud Proxy server will be hosted locally per WLD vCenter
- SDN Endpoint (NSX-T) � Same CAS Cloud Proxy Server will be used locally per WLD vCenter and NSX-T instance. SDN Endpoint will be communicating with CAS Cloud Proxy Server over HTTPS

## 3.3. CAS Data model

##### TO BE UPDATED Figure 6 CAS Data Model
![Figure 6](images/lldCloudAutomationServices/figure6.svg)#TO BE UPDATED FOR MULTIZONAL
CAS Data model reflects the relationship between CAS Objects and vSphere Objects:
**CAS Objects** *(Projects, Zones, Regions, Cloud Accounts, Network Profiles, Storage Profiles) are created in CAS Portal*
**vSphere Objects** *(Datastores, Storage Policies, Networks, Network Domains) are discovered by CAS from vCenter Server and NSX-T Endpoint via CAS Cloud Proxy*

## 3.4.	VMware Cloud Assembly Logical Components

### 3.4.1. Tags
Tags drive the placement of deployments' components through matching of capabilities and constraints. DHC standard tags for e.g. Storage, Networks etc. will be used in Cloud Assembly to map the resources and for the automated provisioning placements.

##### Design Decisions -Tags

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAT001 | DHC platform is using both capabilities and constraints tags | Flexibility in therms of addressing future customer needs by using native functionality | N/A |
| CAT002 | Tags are represented by **key:value** pair convention | Simplify troubleshooting and introducing order for payload placement| N/A |
| CAT003 | By default hard constraints are used | Default implementation in automation platform | N/A |

##### Figure 7 CAS Tags application flow
![Figure 7](images/lldCloudAutomationServices/figure7.svg)
- **Capability Tags**
In Cloud Assembly, capability tags enable us to define placement logic for deployment of infrastructure components. They are a more powerful and succinct option to hard coding such placements.
You can create capability tags on compute resources, cloud zones, images and image maps, and networks and network profiles. Capability tags on cloud zones and network profiles affect all resources within those zones or profiles. Capability tags on storage or network components affect only the components on which they are applied.
- **Constraint Tags**
There are two main areas in Cloud Assembly where constraint tags are applicable. The first is on the configuration side in projects and images. The second is on the deployment side in blueprints. Constraints applied in both areas are merged in blueprints to form a set of deployment requirements.
If tags on the project conflict with tags on the blueprint, the project tags take precedence, thus allowing the cloud administrator to enforce governance rules
You can apply up to three constraints on projects. Project constraints can be hard or soft. By default they are hard. Hard constraints allow you to rigidly enforce deployment restrictions. If one or more hard constraints are not met, the deployment will fail. Soft constraints offer a way to express preferences that will be selected if available, but the deployment won't fail if soft constraints are not met.
- **DHC Standard Tags**
Below Cloud Assembly standard tags will be used in DHC to support analysis, monitoring, and grouping of deployed resources.
 ## Come back to that and recheck
Standard tags are unique within Cloud Assembly. Unlike other tags, users do not work with them during deployment configuration, and no constraints are applied. These tags are applied automatically during provisioning deployments. These tags are stored as system custom properties, and they are added to deployments after provisioning.
The list of standard tags appears below.

##### Table 6 CAS Standard Tags
| Description | Tag |
| --- | --- |
| Organization | org:orgID |
| Project | project:projectID |
| Requester | requester:username |
| Deployment | deployment:deploymentID |
| Blueprint reference (if applicable) | blueprint:blueprintID |
| Component name in blueprint | blueprintResourceName:CloudMachine_1 |
| Placement Constraints: applied in blueprint, request parameters, or via IT policy | constraints:key:value:soft |
| Cloud Account | cloudAccount:accountID |
| Zone or profile, if applicable | zone:zoneID, networkProfile:profileID, storageProfile:profileID |

##### Table 7 DHC Standard Tags
| Description | Tag |
| --- | --- |
| Cloud Zone | CloudZone:`<LocationCode>` **For e.g.** `CloudZone:DTA01` `CloudZone:DTA02` |
| Compute POD | ComputePOD:`<ComuptePODTypeAsperCustomer>` **For e.g.:** `ComputePOD:HighPerformance` `ComputePOD:LowPerformance` `ComputePOD:Development` `ComputePOD:Production` |
| Cloud Storage | CloudStorage:DHC-`<TypeofStorage>` **For e.g.:** `CloudStorage:DHC-Gold` `CloudStorage:DHC-Silver` `CloudStorage:DHC-Bronze` |
| Cloud Network | CloudNetwork:`<TypeOfNetwork>` **For e.g.:** `CloudNetwork:APP (Application)` `CloudNetwork:WEB (Web)` `CloudNetwork:DB (Database)` `CloudNetwork:TRU (Trusted)` `CloudNetwork:DMZ (Demilitarized)` `CloudNEtwork:SEC (Secured)` |

### 3.4.2.	Cloud Proxy
DHC will be using VMware Cloud Proxy component which is an OVA supplied by VMware that contains the credentials and protocols that connect a proxy appliance on a host vCenter server to a vCenter-based cloud account. vCenter-based cloud accounts require that a cloud proxy is deployed to source vCenters for data collection and communication between the cloud account and an on-premises endpoints. This relationship within DHC is 1 proxy to MANY vCenter and NSX (T&V) endpoints.
vCenter-based cloud account types include vCenter Server, NSX-V, NSX-T, and VMware Cloud on AWS. Some cloud account types, such as Amazon Web Services and Microsoft Azure, do not require a cloud proxy.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS015 | Single CAS Cloud Proxy virtual appliance will be deployed for SDDC & SDN endpoints of one customer. (As per customer workload forecast if the Cloud Resource count would be crossing threshold of 10000 cloud resources then 1 Cloud Proxy will be deployed per WLD Domain) Service+LocationCode+CASServerNumber Cloud Proxy will be named like below: e.g. DHCmeccas001 | One CAS Proxy supports up to 10000 Cloud resources (All vSphere objects discovered by CAS Platform) | NA |

### 3.4.3.	Cloud Account
In DHC a cloud account identifies a cloud account type and a DHC vSphere workload vCenter where the associated cloud account resources are hosted.
The cloud accounts in DHC CAS project are based on the DHC CAS region (vSphere workload data center) where that cloud account resides. While preparing profiles and mappings, we need to specify data in relation to a specific cloud account type and region in CAS project.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS016 | Cloud Account will be created per Workload vCenter and NSX-T SDN Instance | This will minimize the resource management complexity within CAS portal | NA |
| CAS017 | Cloud Account name will be based on CustomerCode+LocationCode+vCenter/NSXT Number e.g. nx2gre2vcs001 nx2gre2nsx001 | This will simplify the resource mappings at backend | NA |

### 3.4.4.	Cloud Zone
In DHC a cloud zone defines a set of compute resources for a vSphere cloud account, in a specific account region that are used to deploy a blueprint. Cloud zones are specific to a DHC CAS project.
Additional placement controls include placement policy options, capability tags, and compute tags.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS018 | One Cloud Zone will be created per vSphere Datacenter (One for all Compute POD Clusters within same vCenter) | This will minimize the resource management complexity within CAS portal | NA |
| CAS019 | Default placement policy will be used | Resource load balancing within cluster will be managed on vSphere level via DRS | NA |
| CAS020 | Cloud Zone names will be based on LcoationCode+3DigitNumber For e.g gre2001 | | NA |
| CAS021 | Multiple Compute POD in same vCenter will be utilized with help of different tags For e.g. GRE2-CMP01 � HighPerfromancePOD GRE2-CMP02� LowPerformancePOD GRE2-CMP01 � DevelopmentPOD BE-MEC01-CMP02 � PrdocutionPOD Etc� | | NA |

### 3.4.5. Mappings and Profiles
- **Flavor Mappings**
DHC will be CAS flavor mapping groups a set of target deployment sizings for a specific cloud account/region using natural language naming. Flavor mapping lets you create a named mapping that contains similar flavor sizing�s across your account regions.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS022 | DHC Standard flavor sizing will be used **XSmall** (1 vCPU, 2GB RAM) Only for Linux **Small** (2 vCPU, 4GB RAM) **Medium** (4 vCPU, 8GB RAM) **Large** (8 vCPU, 16GB RAM) **XLarge** (16 vCPU, 32GB RAM) | This will standardize the sizing of the workload with taking care of the base capacity design | NA |
| CAS023 | Same flavour can be mapped with list of regions | | NA |
| CAS024 | Customers can create and use Custom Sizing templates with Custom Blueprints created for that customer | | NA |
| CAS025 | DHC Standard flavor mappings names will be based on xsmall, small ,large,xlarge | | NA |

- **Image Mappings**
DHC will use CAS image mapping groups a set of pre-defined target OS specifications for a specific cloud account/region by using natural language naming. The list Standard OS will be used as published and as per AHS Managed Server OS roadmap.
https://sp2013.myatos.net/ms/gd/ahs/gadp/Document%20Library/Roadmap/2019/source/Aligned%20Roadmap%20AHS%202019%20v1.2.pdf

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS025 | DHC Standard Images will be used in the Image Mappings used in Standard DHC Blueprint | This will standardize the OS usage within DHC | NA |
| CAS026 | Image Mappings will be created per Region (Per DHC Workload vCenter): ImageName - Image (From discovered vCenter Templates) For e.g. **SLES12** (GlobalImage_SLES12) **RHEL7** (GlobalImage_RHEL7) **WIN2012** (GlobalImage_WIN2012) **WIN2012R2** (GlobalImage_WIN2012R2) **WIN2016** (GlobalImage_WIN2016) | This will help in segregation of different vCenter Templates per region | NA |
| CAS027 | Image Mappings names will be based on for managed OS: `Atos-<OSCode>` **For e.g.** Atos-Win2k16, Atos-Sles12, Atos-Rhel7 and for unmanaged OS: '<CustomerCode>-<OSCode>' **For e.g. Acme-Win2k16, Acme-SLES12, Acme-Rhel12 | This will help in segregation of different OS management types | NA |

- **Network Profiles**

A network profile defines a group of networks and network settings that are available for a cloud account in a particular region or data center.
DHC will be using tag matching to identify one or more networks in one or more matched network profiles is available for use when a blueprint is deployed. The network and security settings that are defined in the matched network profile are also applied when the blueprint is deployed.
A network profile will contain the following information:

| | |
| --- | --- |
| Capabilities | Capability tags are applied to all networks in the network profile, but only when the networks are used as part of that network profile. Capability tags are an optional grouping and naming tool for network profiles |
| Networks | Networks, also referred to as subnets, are logical subdivisions of an IP network. A network groups a cloud account, IP address or range, and network tags to control how and where to provision a blueprint deployment. Network parameters in the profile define how machines in the deployment can communicate with one another over IP layer 3 |
| Network policies |
| Load balancers | You can add load balancer settings for the networks that are used in the network profile. Available load balancers have been data-collected from the cloud account. You can also update load balancer settings in the blueprint YAML |
| Security | Security groups are applied to all the machines in the deployment that are connected to the network that matches the network profile. As there might be multiple networks in a blueprint, each matching a different network profile, you can use different security groups for different networks |

Listed security groups are available based on information that is data-collected from the source cloud account.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS027 | Network profiles will be created using existing networks, security groups and Load Balancers | The network / groups / LB provisioning automation will be done using Network provisioning SSR�s from CAS using NSX-T integration endpoint | NA |
| CAS028 | On-demand network and On-demand security group network policies will not be used | This will limit the count of networks as on-demand resources can be only used by the same deployment resources | NA |
| CAS029 | Network Profiles names will be based on below: CustomerCode+LocationCode+NetworkTypeFreetext (app,db,web etc.) For e.g. nx2gre2app nx2gre2db | | NA |
| CAS030 | DHC will be using DHCP service from NSX-T for the 1st release, IPAM is not covered in 1st Release of DHC | This will provide minimum network access for hosted workload | There will be no control over the IP assignment due to DHCP |

- **Storage Profiles**

DHC VFC SDDC cloud account region will contain different storage profiles that let the cloud administrator define VMware storage profile for the DHC CAS region.
Storage profiles include disk customizations, and a means to identify the type of storage by capability tags. Tags are then matched against provisioning service request constraints to create the desired storage at deployment time.
Storage profiles are organized under cloud-specific regions. One cloud account might have multiple regions, with multiple storage profiles under each.

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS031 | DHC vSphere Storage policies (DHC-Gold, DHC-Silver, DHC-Bronze) will be utilized in CAS Storage profiles as datastore defaults | This will avoid the issue of misconfiguration of storage policy on CAS level | NA |
| CAS032 | Thin provisioning type will be used in CAS Storage profile | This will be applied on Datastore level instead disk level for all automated provisioned disks | NA |
| CAS033 | DHC Standard Storage Profiles will be created per region | | NA |
| CAS034 | Storage Profile names will be based on below: CustomerCode+LocationCode+StorageType For e.g. nx2gre2gold, nx2gre2silver, nx2gre2bronze etc. | | NA |

### 3.4.6.	Projects
VMware CAS Projects control who has access to Cloud Assembly blueprints and where the blueprints are deployed. CAS Projects will be used to organize and govern what DHC users can do and where they can deploy blueprints in DHC cloud infrastructure.
DHC Cloud administrators will set up the projects, adding users and cloud zones. Anyone who creates and deploys blueprints must be a member of at least one project.

##### Figure 8 CAS Data Model
![Figure 8](images/lldCloudAutomationServices/figure8.svg)

| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS035 | One Project will be created per Customer in Multi-Tenant and Single Tenant Scenarios CustomerCode+3DigitNumber For e.g nx2001 | This will help in the resource segregation for multiple tenants | NA |
| CAS036 | Multiple Projects will be created per customer in Departmental segregation scenario | This will help in the resource segregation for same Tenant having multiple departments / divisions | NA |

### 3.4.7. Integrations
This section will be populated in next release of DHC after finalization of the components to be integrated with CAS for e.g. IPAM, Guest customization tools like Cloud init, ABX, Bitbucket etc�

## 3.5.	Security

### 3.5.1.	Role Based Access Control
Atos based solutions must guarantee proper access management. Following design decisions are made in that area.

##### Table 8 Design Decisions � RBAC
| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS037 | VMware Cloud Automation Service Console will be only managed by DHC Operations team | DHC administrator must have Cloud Administrator role to manage the CAS Console | NA |
| CAS038 | My VMware account with ATOS mail address is mandatory for DHC administrator | This will allow only Atos authorized admins to have possibility to login into CAS Console | Each DHC Administrator require to have MyVMware account with Atos Mail address as primary mail |
| CAS039 | VMware CAS Security Multi-factor authentication to be enabled for Atos Users | Improved security due to dual factor authentication model | This authentication cannot be enabled as default as each user has to map their Mobile device with using Google Authenticator for this |
| CAS040 | Customers will not have any direct access to CAS Console. Only API access will be provided to customers | DHC provisioning will be done using CAS Standard API | Direct CAS Console access will not be available to customers |
| CAS041 | There will be a separate API Service Account created for each CAS Organization | DHC Service account will be used to integration of Customer based ITSM / ATOS SNOW integration | NA |
| CAS042 | Use dedicated service account for Cloud Proxy integration with vCenter | Improved security due to usage dedicated service account per service. In line with AtoS RBAC model | Password rotation needs to be automated |
| CAS043 | Consumer Users are assigned Service Broker user role on organization level  | Required for consumption | Out of the box functionlaity |
| CAS044 | Member role on project level is assigned for customer users | Required for consumption | Out of the box functionlaity |
| CAS045 | 2nd day activities policies are used for restrictions purpose | Flexibility for custoemrs requirements | Policies must be created manually. Poper trainig is required |



### 3.5.2.	Firewall
This section covers all firewall related decisions influencing content of that LLD

##### Table 9 Design Decisions - Firewall
| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS043 | NSX-T based firewalls will be enabled for Workload Domains | Security increase | Detailed configuration is required |
| CAS044 | Traffic between CAS Cloud Proxy and vSphere Management Components (vCenter, NSX-T Manager) is default allowed via NSX-V firewall | Required for functionality to work. Direct Communication is less complex and simple for maintain. | NA |
| CAS045 | Traffic between VMware CAS Poral and Cloud Proxy will be allowed using Internet proxy | Required for the discovery and provisioning functionality to work via CAS | Single proxy design may impact the automation function availability |

## 3.6.	Availability and Scalability

### 3.6.1.	Availability Design
The design decisions below are made to guarantee availability of CAS product etc.
Availability design details awaited from VMware for VMware Cloud Automation Service.
VMware Cloud Automation Service Portal will follow the standard DHC Availability i.e. 99.5

### 3.6.2.	Scalability Design
Scalability design details awaited from VMware for VMware Cloud Automation Service.
Around 150,000 objects per CAS Organization, to be confirmed by VMware.

## 3.7.	Recoverability
The chapter below provides detailed design choices to protect against data loose and backup functionality and against Datacenter failure.
Recoverability details awaited from VMware for VMware Cloud Automation Service. (Details will be updated in LLD in next release as received from VMware)

# 4.	Detailed Physical Design
Detailed physical design is covering fixed configuration details that are fixed for solution. Any values that are customer dependent are presented in `<angle brackets red italic>` form. All configuration details are presented in table form. CAPITALS in any names should not be used as all names should be written I use of lowercases.

## 4.1.	Management Plane

### 4.1.1.	Virtual Machine Configuration table
VMs that are part of implementation and they roles are listed in following table

##### Table 10 VMs list
| VM Name | VM Role | Description |
| --- | --- | --- |
| Cloud Proxy | VMware CAS Cloud Proxy | Appliance delivered by VMware |
Configuration details for VMs are represented below.

##### Table 11 Configuration details VM XYZ
| Parameter | Value | Description |
| --- | --- | --- |
| Number of instances | 1 | A single collector VM can typically support 10,000 VMs, However, taking consideration of Customer Workload volume additional Cloud Proxy appliances required to be deployed per Workload vCenter if managed resource count is reaching the 10000 VM�s threshold. |
| Operating System | Other 32 bit | Appliance delivered by VMware |
| vCPU | 4 |
| Memory | 12 GB |
| Storage | Disk 1: 60 GB Disk 2: 20 GB |

## 4.2.	Security

### 4.2.1.	Role Based Access Control
**Cloud Assembly User Roles:**
User roles determine what you can see and do in Cloud Assembly. Some roles are defined at the service organization level, and some are specific to Cloud Assembly
- **Global roles:**
Global roles are defined in the organization and might be applied across multiple services
Below roles are defined for user access purpose including service accounts.

##### Table 12 CAS Global Organization Roles
| Role Name | Comment |
| --- | --- |
| Cloud administrator | Must have read and write access to the entire user interface and API resources. This is the only user role that can create a new project and assign a project administrator |
| Regular user | Any user who does not have the cloud admin role. |

##### Table 13 DHC CAS Global Organization Roles
| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS046 | CAS Cloud Administrator Role will be assigned to DHC Cloud Administrators also to the DHC IaaS Automation API Account to automate the customer onboarding process | Security increase | Detailed configuration is required |
- **Project roles and permissions:**
 - Project roles, the project administrator and project member, are defined in Cloud Assembly and can vary between projects.
 - In the following tables, where the permissions are defined, remember that the cloud administrator has full permission on all areas of the UI.
 - Project administrators leverage the infrastructure that is created by the cloud administrator to ensure that their project members have the resources they need for their development work.  
**1.	Project Administrator Permissions:**

##### Table 14 Project Administrator Permissions
**Tab - Infrastructure**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Configure - Projects | Yes (only the projects you are a member of) | No | No |
| Configure � Cloud Zones | No | No | No |
| Configure � Flavor Mappings | Yes | No | No |
| Configure � Image Mappings | Yes | No | No |
| Configure � Network Profiles | Yes | No | No |
| Configure � Storage Profiles | Yes | No | No |
| Configure - Tags | Yes | No | No |
| Resources - Compute | Yes | No | No |
| Resources - Network | Yes | No | No |
| Resources - Storage | Yes | No | No |
| Resources - Machines | Yes (only the ones that you deployed) | Yes | Yes (only the ones that you deployed) |
| Resources - Volumes | | | |
| Activity - Requests | Yes (only the ones that you deployed) | N/A | Yes (only the ones that you deployed) |
| Activity - Events | Yes (only the ones that you deployed) | N/A | Yes (only the ones that you deployed) |
| Connections � Cloud Accounts | No | No | No |
| Connections - Integrations | | | |
| Connections � Cloud Proxies | | | |
| Cost � VMC Assessment | Yes | No | No |
| Cost - Private Clouds | Yes | No | No |
| Onboarding | | | |

**Tab - Blueprints**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Blueprints | Yes (only the ones that you deployed) | Yes (only the ones that you deployed) | Yes (only the ones that you deployed) |

**Tab - Deployments**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Deployments | Yes (only the ones that you deployed) | N/A | Yes (only the ones that you deployed) |

**2.	Project Member Permissions:**

##### Table 15 Project Member Permissions
**Tab - Infrastructure**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Configure - Projects | Yes (only your projects) | No | Yes (only your projects) |
| Configure � Cloud Zones | No | No | No |
| Configure � Flavor Mappings | Yes | No | No |
| Configure � Image Mappings | Yes | No | No |
| Configure � Network Profiles | Yes | No | No |
| Configure � Storage Profiles | Yes | No | No |
| Configure - Tags | Yes | No | No |
| Resources - Compute | Yes | No | No |
| Resources - Network | Yes | No | No |
| Resources - Storage | Yes | No | No |
| Resources - Machines | Yes (only your projects) | Yes | Yes (only your projects) |
| Resources - Volumes | | | |
| Activity - Requests | Yes (only your projects) | N/A | Yes (only your projects) |
| Activity - Events | Yes (only your projects) | N/A | Yes (only your projects) |
| Connections � Cloud Accounts | No | No | No |
| Connections - Integrations | | | |
| Connections � Cloud Proxies | | | |
| Cost � VMC Assessment | Yes | No | No |
| Cost - Private Clouds | Yes | No | No |
| Onboarding |

**Tab - Blueprints**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Blueprints | Yes (only your projects) | Yes (only your projects) | Yes (only your projects) |

**Tab - Deployments**

| Node or Area | View | Create | Modify/Delete |
| --- | --- | --- | --- |
| Deployments | Deployments | Yes (only your projects) | N/A | Yes (only your projects) |

##### Table 16 DHC CAS Project Roles
| Decision ID | Design Decision | Design Justification | Design Implication |
| --- | --- | --- | --- |
| CAS047 | CAS Cloud Project Administrator Role will be assigned to DHC Cloud Administrators also to the DHC IaaS Automation API Account to automate the customer onboarding process | Security increase | Detailed configuration is required |
| CAS048 | CAS Cloud Project Member Role will be assigned to DHC Cloud Provisioning Service Account also to the DHC Customer Provisioning Automation API Account to automate the machine provisioning process | Explicit control on the resource modifications and actions | NA |
Below Service account are created as part of design implementation.

##### Table 17 RBAC Groups
| Service Account Name | AD Group Membership | Member Type (local, Active Directory) |
| --- | --- | --- |
| SVC-DHC-<LocationCode>-CAS01 | DELG-DHC-AD-G-AdminPwdPolicy | Active Directory |
| SVC-DHC-<LocationCode>-CAS01 | DELG-DHC-AD-L-DenyLogonLocal | Active Directory |
| SVC-DHC-<LocationCode>-CAS01 | DELG-DHC-AD-L-LogOnAsServiceRights | Active Directory |
| SVC-DHC-<LocationCode>-CAS01 | APPL-DHC-VCS-L-Admin | Active Directory |
| SVC-DHC-<LocationCode>-CAS01 | APPL-DHC-NSX-L-Admin | Active Directory |

### 4.2.2.	Firewall

##### Table 18 Firewall Rules
| Service/Traffic Name | Source | Destination | Port(s) | Protocol |
| --- | --- | --- | --- | --- |
| Cloud Proxy (via internet proxy)| VMware Cloud Proxy Appliance | *.vmwareidentity.com | 443 | TCP |
| Cloud Proxy (via internet proxy)| VMware Cloud Proxy Appliance | gaz.csp-vidm-prod.com | 443 | TCP |
| Cloud Proxy (via internet proxy)| VMware Cloud Proxy Appliance | *.vmware.com | 443 | TCP |
| vCenter/HTTPS | VMware Cloud Proxy Appliance | vCenter Server | 443 | TCP |
| vCenter/ICMP | VMware Cloud Proxy Appliance | vCenter Server | ALL | ICMP |
| SDN| VMware Cloud Proxy Appliance | NSX-T Manager | 443 | TCP |
| Proxy| VMware Cloud Proxy Appliance | Proxy IP | {as defined per setup} | TCP |
| DNS| VMware Cloud Proxy Appliance | DHC Domian constrolers | 53 | TCP |

## 4.3.	Availability and Scalability

### 4.3.1.	Availability

##### Table 19 Availability details
| Component | Details |
| --- | --- |
| Cloud Proxy Appliance | Cloud Proxy appliance VM availability is managed by vSphere HA and DR configured on the DHC Management |
| VMware Cloud Automation Portal | CAS Public Portal availability is managed by VMware. CAS Public Portal will follow the standard DHC Service Availability i.e. 99.5 |

### 4.3.2.	Scalability

##### Table 20 Scalability details
| Parameter | Count | Managed Resources | Details |
| --- | --- | --- | --- |
| Number of Cloud Proxy | 1 per vCenter | 10000 | More Cloud Proxy Appliances needs to be deployed if the managed resource count would be reaching defined threshold |

# 5 Service Request capabilities

## 5.1 First day request capabilities

| Name | Scope | Description | Additional details |
| :---: | :--- | :--- | :--- |
| Create VM | Deployment | Create new deployment that consists one or more VMs and additional resources like storage or network | Additional VM resource property vmPlacement: {value} is introduced to manage VM placement group via ABX extensibility |
| Place VM in chosen AZ | Deployment | Create VM extention: Place VM in AZ in line with policy | In stretched cluster scenario locate VM in AZ that will be part of stretched cluster inline with location (dependent of group placement) |
| Provision VM DR protected| Deployment | Provision new deployment with VM resources DR protected (VM’s, disks) | Add VMs into protection groups in Site Recovery Manager (SRM). Report status of protection on VM level. Capabilities: recovery order, dependencies, shutdown and startup actions. IP customizations integrated with vRA Cloud network profiles. |

## 5.2 Second day request capabilities
| Name | Scope | Description | Additional details |
| :---: | :--- | :--- | :--- |
| Move deployment between AZ’s | Deployment | Migrate all VMs in deployment from hosts located in different AZ | In stretched cluster scenario vmotion VMs from current site to new one located in same cluster. Maintain location properties for CMDB purpose. Place VMs in VM group (Compute Policies? – that is already part of VMC, not sure if that is a way vCenter concept will go forward) assigned to AZ |
| Move VM between AZs | Resource | Migrate chosen VM in deployment from hosts located in different AZ | In stretched cluster scenario vmotion VM from current site to new one located in same cluster. Maintain location properties for CMDB purpose. Place VMs in VM group (Compute Policies?) assigned to AZ |
| Change VM DR protection| Deployment/resource | Add VM to DR/remove VM from DR/ Change protection group. Enable and disable VM for disaster recovery. | Allow automation that will have integration with SRM for 2 day activities on protected VM. ABX only. Capabilities: recovery order, dependencies, shutdown and startup actions. IP customizations integrated with vRA Cloud network profiles. |
| Add/remove disk| Resource | Add disk to DR protected VM | Extend existing 2nd day activities for storage management for DR capabilities. Nee disk added to VM that is configured for DR should be included by default in DR as well. |
