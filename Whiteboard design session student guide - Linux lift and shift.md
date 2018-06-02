![](images/HeaderPic.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Linux lift and shift
</div>

<div class="MCWHeader2">
 Whiteboard design session student guide
</div>

<div class="MCWHeader3">
March 2018
</div>



Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Linux lift and shift whiteboard design session student guide](#linux-lift-and-shift-whiteboard-design-session-student-guide)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Step 1: Review the customer case study](#step-1-review-the-customer-case-study)
        - [Customer situation](#customer-situation)
        - [Customer needs](#customer-needs)
        - [Customer objections](#customer-objections)
        - [Infographic for common scenarios](#infographic-for-common-scenarios)
    - [Step 2: Design a proof of concept solution](#step-2-design-a-proof-of-concept-solution)
    - [Step 3: Present the solution](#step-3-present-the-solution)
    - [Wrap-up](#wrap-up)
    - [Additional references](#additional-references)

<!-- /TOC -->

#  Linux lift and shift whiteboard design session student guide

## Abstract and learning objectives 

In this whiteboard design session, attendees will learn how to migrate an existing Linux based deployment into Microsoft Azure and configure it for availability, connectivity, monitoring and general best practices with Azure Virtual Machines (VMs), Virtual Machine Scale Sets and Azure Web Apps with Linux.

-   Configure Linux VMs and VM Scale Sets in Azure for availability, storage, and connectivity

-   Migrate data from on-premises to Microsoft

-   Establish connectivity between multiple regions and on-premises to Azure

-   How to deploy and scale applications to Azure Web Apps on Linux

## Step 1: Review the customer case study 

**Outcome** 

Analyze your customer’s needs.
Time frame: 15 minutes 
Directions: With all participants in the session, the facilitator/SME presents an overview of the customer case study along with technical tips. 
1.  Meet your table participants and trainer 
2.  Read all of the directions for steps 1–3 in the student guide 
3.  As a table team, review the following customer case study


### Customer situation

Fabrikam Global Manufacturing & Operations Corporation (FGMO), based in Japan, provides product design, manufacturing, and repair services of domestic individual or industrial electronics, as well as global support for their customers. To avoid any impact from restructured support operations, executives decided to migrate on-premises customer support systems into Microsoft Azure. The hope is that running Linux VMs on Azure should enable FGMO to lower costs while sustaining or even increasing availability of the application.

Migration to the cloud has been of keen interest to General Manager Kato Takahashi of FGMO. The notable advantages include the cloud's user friendliness, potential for reducing costs, and especially its ability to elastically scale up and down. "Our desire to use the cloud is in alignment with FGMO's corporate strategy", says Takahashi.

To ensure a smooth migration to the cloud, FGMO turned to A. Datum, Inc., a solution integrator and Microsoft partner, with system design, operation services, and enterprise-scale datacenter operational knowledge and cloud experience. A. Datum proposed a migration of the customer support system infrastructure, which was hosted on-premises, to the cloud to reduce costs. A. Datum proposed using the Azure platform to enable an efficient and secure migration.

The following objectives were put in place:

-   Migrate customer support systems from on-premises to Azure and achieve cost reduction without lowering the quality-level of Fabrikam support operations.

-   With Azure Linux support, leverage infrastructure as a service (IaaS) or platform as a service (PaaS) approaches for an on-schedule, agile migration to the cloud.

-   Implement Azure ExpressRoute and its highly secured connection to migrate systems that had proven difficult to migrate to the cloud previously.

After completing a cloud readiness assessment of the application for Fabrikam, A. Datum has gathered the following information:

-   Helpdesk software is deployed on a single VM running a traditional LAMP stack (Linux, Apache, MySQL & PHP).

-   Linux Ubuntu 16.04, Apache 2, MySQL 5.6 (100GB DB), and PHP 7.0 are the current versions.

-   The VM is hosted on a VMWare infrastructure using 8vCPUs and 16GB of RAM. When asked about why the applications required 8vCPUs, the FGMO team stated that during busy periods the server can get overloaded. They decided to scale up the VM to handle the load when needed.

-   The FGMO team stated there has been downtime due to the web server not responding. When this happens, they are forced to reboot the VM (every few weeks). They have noticed error OST98744 in event log when this happens.

-   The VMWare infrastructure is deployed in their Tokyo Datacenter. Customers and Partners access the system via the Internet while FGMO Support teams access the application from the internal network.

    ![In this Customer Situation diagram, Tokyo vCenter datacenter uses the following products: Linux, Apache, MySQL php, and osTicket. FGMO Customers and Partners use the internet to access the datacenter, while the FGMO Support Teams use the intranet. ](images/Whiteboarddesignsessiontrainerguide-Linuxliftandshiftimages/media/image2.png "Customer Situation diagram")

### Customer needs

1.  Migrate their existing support application to Microsoft Azure with minimal changes and minimal disruption to their service.

2.  Ensure that the Linux instances deployed in Azure are configured for high availability, cost optimization, performance and best practices.

3.  Establish connectivity between their on-premises environment and the virtual network where the support application is deployed.

4.  Configure monitoring for the support application to ensure that if an issue comes up that the support team is alerted, and a maintenance script is executed to mitigate known issues.

5.  Ensure that only administrators of the support application will have access to the new infrastructure. It is important that any security methodologies and accounts leverage their current Active Directory Domain Services installation in the current datacenter.

6.  Fabrikam is interested in an offering from A. Datum that focuses on Optimization Services. After the initial migration, they would like to consider solutions to cost optimize the web farm for the support applications by only spinning up resources as needed as well as look at options for several of the smaller supporting services.

### Customer objections

1.  FGMO has a detailed support history of their client's environments. This information cannot be lost during the migration. Any data loss would be considered a migration failure.

2.  FGMO's data needs to be encrypted at rest and in transit. The current system is not configured in this manor, but when moving to the cloud they need to ensure their clients privacy.

3.  There should be no single point of failure in the system once it is in the cloud.

4.  This is a mission critical system and FGMO needs to be sure that they have SLAs from the cloud provider. This SLA should be no less than 99.9%.

5.  FGMO is ready for the cloud but would like to move forward in a conservative manor. They are looking for a phased approach with a move to IaaS and then to PaaS later.

### Infographic for common scenarios

VM Scale Set

![The Virtual Machine Scale Set diagram begins with the Internet, which uses a Public IP to access an Azure load balancer. A virtual network encompasses a Subnet using network security groups. The Availability Set is made up of a VM Scaleset. the Azure load balancer points to this VM Scaleset. Outside of the Virtual network are Managed disks and a Storage account including diagnostic logs.](images/Whiteboarddesignsessiontrainerguide-Linuxliftandshiftimages/media/image3.png "Virtual Machine Scale Set diagram")

ExpressRoute with VPN Failover

![At a high level, the ExpressRoute with VPN Failover flowchart is as follows: Computers use a gateway to access local edge routers, which use an ExpressRoute circuit to reach Microsoft Edge routers. At this point, the following items are in a virtual network: The Gateway subnet is made up of an ExpressRoute Gateway, and a VPN Gateway. The VPN Gateway points to a Management subnet, which has NSG and a Jumpbox. The Microsoft edge routers points to the ExpressRoute Gateway in the Gateway subnet, which points to the router in the Web tier, which distributes the information to three VMs, which then pass on the information to the same setup in the Business tier, and finally the Data tier.](images/Whiteboarddesignsessiontrainerguide-Linuxliftandshiftimages/media/image4.png "ExpressRoute with VPN Failover flowchart")

Azure App Service

![The Azure App Service diagram begins wtih the internet, which points to both Azure Active Directory and App Service app, using Authentication. A Resource group encompasses an App Service Plan and an Azure SQL Database. The App Service Plan is made up of multiple App Service apps, and the Azure SQL Database encompasses A logical SQL server, and two SQL databases.](images/Whiteboarddesignsessiontrainerguide-Linuxliftandshiftimages/media/image5.png "Azure App Service diagram")

## Step 2: Design a proof of concept solution

**Outcome** 
Design a solution and prepare to present the solution to the target customer audience in a 15-minute chalk-talk format. 

Time frame: 60 minutes

**Business needs**

Directions: With all participants at your table, answer the following questions and list the answers on a flip chart. 
1.  Who should you present this solution to? Who is your target customer audience? Who are the decision makers? 
2.  What customer business needs do you need to address with your solution?

**Design** 
Directions: With all participants at your table, respond to the following questions on a flip chart.


*Migration Phases*:

1.  Provide a high-level diagram for the two phases of the migration of the FGMO helpdesk application to Azure. The first phase should use an IaaS approach followed by the second leveraging PaaS technologies.

2.  For each phase capture the following aspects of the Lift & Shift:

    -   How will the database be migrated to Azure?

    -   How will the App and Data tiers be split and secured?

    -   Insure that the designs are built for high availability, cost optimization, performance and using best practices.

3.  Describe how you will leverage Azure access control methods to ensure that only administrators of the support application will have access to the new infrastructure.

4.  Detail how monitoring for the support application will be configured as well as how automation could enable the availability to mitigate known issues.

*Network Design*:

1.  Hybrid cloud: Provide a high-level hybrid network design to connect FGMO to Azure using ExpressRoute with a VPN Failover

2.  Make sure to show your Network Security Groups

*Customer Objections*:

1.  Provide details on how your will address each of the objections that were put forward by the client.


**Prepare**

Directions: With all participants at your table: 

1.  Identify any customer needs that are not addressed with the proposed solution. 
2.  Identify the benefits of your solution. 
3.  Determine how you will respond to the customer’s objections. 

Prepare a 15-minute chalk-talk style presentation to the customer. 

## Step 3: Present the solution

**Outcome**
 
Present a solution to the target customer audience in a 15-minute chalk-talk format.

Time frame: 30 minutes

**Presentation** 

Directions:
1.  Pair with another table.
2.  One table is the Microsoft team and the other table is the customer.
3.  The Microsoft team presents their proposed solution to the customer.
4.  The customer makes one of the objections from the list of objections.
5.  The Microsoft team responds to the objection.
6.  The customer team gives feedback to the Microsoft team. 
7.  Tables switch roles and repeat Steps 2–6.


## Wrap-up

Time frame: 15 minutes

-   Tables reconvene with the larger group to hear a SME share the preferred solution for the case study.

##  Additional references

|    |            |       
|----------|:-------------:|
| Azure and Linux | <https://docs.microsoft.com/en-us/azure/virtual-machines/linux/overview/> |
| Linux on distributions endorsed by Azure | <https://docs.microsoft.com/en-us/azure/virtual-machines/linux/endorsed-distros/> |
| What are Azure Virtual Machine Scale Sets | <https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview/> |
| Deploy your application on Virtual Machine Scale Sets | <https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app/> |
| Azure Database for MySQL Documentation | <https://docs.microsoft.com/en-us/azure/mysql/> |
| Create and manage Azure Database for MySQL server using Azure portal | <https://docs.microsoft.com/en-us/azure/mysql/howto-create-manage-server-portal/> |
| Create and manage Azure Database for MySQL firewall rules by using the Azure portal | <https://docs.microsoft.com/en-us/azure/mysql/howto-manage-firewall-using-portal/> |
| Connect Azure Web App to Azure Database for MySQL | <https://docs.microsoft.com/en-us/azure/mysql/howto-connect-webapp/> |
| Azure Database for MySQL: Use MySQL Workbench to connect and query data | <https://docs.microsoft.com/en-us/azure/mysql/connect-workbench/> |
| Download MySQL Workbench | <https://dev.mysql.com/downloads/workbench/> |
| App Service for Linux | <https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro/> |