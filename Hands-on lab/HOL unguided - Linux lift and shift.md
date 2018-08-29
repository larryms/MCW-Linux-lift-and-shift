![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png  "Microsoft Cloud Workshops")

<div class="MCWHeader1">
 Linux lift and shift
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
June 2018
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Linux lift and shift hands-on lab unguided](#linux-lift-and-shift-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Requirements](#requirements)
    - [Help references](#help-references)
    - [Exercise 1: Deploy the on-premises OsTicket VM application](#exercise-1-deploy-the-on-premises-osticket-vm-application)
        - [Task 1: Deploy the OnPremVM](#task-1-deploy-the-onpremvm)
        - [Task 2: Export the osticket database](#task-2-export-the-osticket-database)
    - [Exercise 2: Migrate to Azure IaaS VM Scale Sets and MySQL cluster](#exercise-2-migrate-to-azure-iaas-vm-scale-sets-and-mysql-cluster)
        - [Task 1: Deploy the MySQL HA Cluster](#task-1-deploy-the-mysql-ha-cluster)
        - [Task 2: Connect to the MySQL cluster and restore the database](#task-2-connect-to-the-mysql-cluster-and-restore-the-database)
        - [Task 3: Deploy the Virtual Machine Scale Set for the OsTicket application](#task-3-deploy-the-virtual-machine-scale-set-for-the-osticket-application)
        - [Task 4: Connect the MySQLVNet to the Scale Sets VNet](#task-4-connect-the-mysqlvnet-to-the-scale-sets-vnet)
        - [Task 5: Export the osTicket database from the MySQL cluster](#task-5-export-the-osticket-database-from-the-mysql-cluster)
    - [Exercise 3: Migrate the OsTicket application from Azure IaaS to PaaS](#exercise-3-migrate-the-osticket-application-from-azure-iaas-to-paas)
        - [Task 1: Create the MySQL Database](#task-1-create-the-mysql-database)
        - [Task 2: Restore the osticket database to MySQL PaaS](#task-2-restore-the-osticket-database-to-mysql-paas)
        - [Task 3: Create the Web App](#task-3-create-the-web-app)
        - [Task 4: Configure the OsTicket Web App](#task-4-configure-the-osticket-web-app)
    - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->

# Linux lift and shift hands-on lab unguided

## Abstract and learning objectives

In this hands-on lab, you will migrate an on-premises based help desk application called OsTicket, to Azure. This will be a two-phase project to lift-and-shift the application into Azure IaaS and then migrate it to Azure PaaS. The application is Linux based using Apache, PHP and MySQL (LAMP). During the process of these phases, you will ensure zero data loss.

By the end of the hands-on lab you will be better able to configure Linux VMs and VM Scale Sets in Azure for availability, storage, and connectivity. You will also be better prepared to migrate data from on-premises to Azure, establish connectivity between multiple regions and on-premises to Azure. You will also learn how to deploy and scale applications to Azure Web Apps on Linux.

## Overview

Fabrikam Global Manufacturing & Operations Corporation, based in Japan, provides product design, manufacturing, and repair services of domestic individual or industrial electronics, as well as global support for their customers. 

To avoid any impact from restructured support operations, executives decided to migrate on-premises customer support systems into Microsoft Azure. The hope is that running Linux VMs on Azure should enable Fabrikam to lower costs while sustaining or even increasing availability of the application.

-   **Phase I:** Lift and shift the application from on-premises to Azure IaaS using an auto scaling Virtual Machine Scale Set and a MySQL cluster with 3 nodes

-   **Phase II:** Migrate to PaaS using Azure App Services with a Linux Docker Container and Azure Database for MySQL

**Phase I will result in an environment resembling this diagram:**

![The Phase I diagram has two Resource Groups, which are connected by VNet Peering. The first Resource Group is MySQLVNet, and has MySQL, bitnami, and Linux. The second Resource Group is OsticketVNet, and has Linux, Apache, PHP, and Osticket. ](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image2.png "Phase I diagram")

**Phase II will result in an environment resembling this diagram:**

![The Phase II diagram has /OsTicket with an arrow pointing to a Resource Group containing Linux, Docker, osTicket, a web client, and MySQL.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image3.png "Phase II diagram")

## Requirements

You must have a working Azure subscription to carry out this hands-on lab step-by-step.

## Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| Azure Database for MySQL | <https://docs.microsoft.com/en-us/azure/mysql/> |
| Azure Database for MySQL | <https://docs.microsoft.com/en-us/azure/mysql/howto-create-manage-server-portal/> |
| Azure Database for MySQL | <https://docs.microsoft.com/en-us/azure/mysql/howto-manage-firewall-using-portal/> |
| Connect Azure Web App to Azure Database for MySQL | <https://docs.microsoft.com/en-us/azure/mysql/howto-connect-webapp/> |
| Azure Virtual Machine Scale Sets | <https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/> |
| Using VM Extensions with Azure Virtual Machine Scale Sets | <https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-scaling-app-template/> |
| App Service for Linux | <https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro/> |
| Azure CLI | <https://docs.microsoft.com/en-us/cli/azure/install-azure-cli/> |




## Exercise 1: Deploy the on-premises OsTicket VM application

Duration: 45 Minutes

In this exercise, you will deploy a VM using an ARM template that will act as the on-premises installation of the OsTicket application. This will consist of a single Ubuntu Linux 16.04-LTS VM with Apache, PHP and MySQL installed (LAMP). This application is a helpdesk management software that you will lift and shift into Azure. There is sample data in the application that will be retained throughout the two phases of the project.

![This Resource group for OnPremVNet includes the previously mentioned products.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image20.png "Resource group")

### Task 1: Deploy the OnPremVM

1.  Using the portal or the Azure Cloud Shell deploy the following template into a resource group named: **OsTicketOnPremRG**: <https://cloudworkshop.blob.core.windows.net/linux-lift-shift/onpremvmdeploy.json>

2.  Using the Azure portal locate the Public IP address and browse this address. The Support Center OsTicket application should load. Click the **Sign in** link, locate **I'm an agent**, and click the **sign in here** link. At the OsTicket screen, enter the username and password and click **Log In**.

    -   Username: ***demouser***

    -   Password: ***demo\@pass123***

3.  Once logged into the OsTicket system click **My Tickets** and click through to one of the tickets. Next, Click the **Users Tab** and notice the user's names that are entered into the system. Feel free to create new tickets or new users to add to your dataset.

*Exit Criteria:*

-   The OnPremVM has been deployed to Azure

-   A successful connection to the website has been made and there is current data in the application

### Task 2: Export the osticket database

1.  Start and connect to the **LABVM** using Remote Desktop. Connect to the MySQL Database using MySQL Workbench.

2.  Click the Plus sign next to MySQL Connections on the Workbench

    ![The plus sign on the My SQL Collections option is selected.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image21.png "My SQL Collections option")

3.  Enter the following information to configure to connect to your OnPremVM:

    -   Connection Name: **OnPremVM**

    -   Connection Method: **Standard TCP/IP over SSH**

    -   SSH Hostname: **\<enter the Public ip address\>**

    -   SSH Username: **demouser**

    -   SSH Password: **Click Store in Vault: demo\@pass123**

    -   MySQL Hostname: **127.0.0.1**

    -   MySQL Server Port: **3306**

    -   Username: **osticket**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![The Setup New Connection page displays with fields set to the previously defined settings.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image22.png "Setup New Connection page")

4.  Connect to the data base and the review the **osticket** database. Query the database to see a list of all the users on the system in the **ost\_user** table. Notice that these are the same names that you saw in the OsTicket UI.

5.  Export the data from the **osticket** database using the **Export to Self-Contained File** format and name the file **c:\\HOL\\onpremvm.sql**

**Note**: You will get a version mismatch warning, click **Continue Anyway**.

*Exit Criteria:*

-   A connection using MySQL Workbench was made and the osticket database has been successfully exported

## Exercise 2: Migrate to Azure IaaS VM Scale Sets and MySQL cluster

Duration: 90 Minutes

In this exercise, you will deploy the OsTicket application to Azure IaaS. In the first task, you will deploy and configure a MySQL Cluster with multiple nodes. Next, you will restore the database that was exported using MySQL Workbench. After the data tier of your application has been configured you will deploy a Virtual Machine Scale Set. The OsTicket application will be installed on the instances using the Custom Linux Extension and will be self-configuring. The Scale Set will be setup to autoscale and the VMs that spin up will also self-configuring.

![The Phase I diagram has two Resource Groups, which are connected by VNet Peering. The first Resource Group is MySQLVNet, and has MySQL, bitnami, and Linux. The second Resource Group is OsticketVNet, and has Linux, Apache, PHP, and Osticket. ](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image2.png "Phase I diagram")

### Task 1: Deploy the MySQL HA Cluster

1.  Using the portal or the Azure Cloud Shell deploy the following template into a resource group named: **OsTicketMySQLVMRG**: <https://cloudworkshop.blob.core.windows.net/linux-lift-shift/mysqlhadeploy.json>

2.  Take note of the Settings which has the Admin Username and Admin Password:

    -   OS User Name: **bitnami**

    -   OS Admin Password: **demo\@pass123**

    -   App Database: **osticket**

    -   App Password: **demo\@pass123**

Note: It is very important to note change any of these parameters.

1.  Locate the **osticketip** Public IP address and take note of the address

*Exit Criteria:*

-   The MySQL Cluster was deployed successfully into Azure IaaS and the Public IP address of the master node was noted

### Task 2: Connect to the MySQL cluster and restore the database

1.  On the **LABVM**, connect to the master node of the MySQL cluster using MySQL Workbench

2.  Enter the following information to configure to connect to your MySQL master node:

    -   Connection Name: **MySQL Cluster**

    -   Connection Method: **Standard TCP/IP over SSH**

    -   SSH Hostname: **\<enter the Public ip address\>**

    -   SSH Username: **bitnami**

    -   SSH Password: Click Store in Vault: **demo\@pass123**

    -   MySQL Hostname: **127.0.0.1**

    -   MySQL Server Port: **3306**

    -   Username: **root**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![In the Setup New Connection dialog box, fields are set to the previously defined settings.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image23.png "Setup New Connection dialog box")

3.  Once the Workbench loads, click **Server Status**. Review the details of the cluster. Remember you are connected to the master for the cluster. Run the following query to see the status of the replica nodes in the cluster
    ```
    SHOW SLAVE HOSTS
    ```
4.  Restore the database that you exported from the OnPremVM to the MySQL cluster. Make sure to use the **Import from Self-Contained File** option.

5.  Run a query to on the **ost\_user** table to show that the data was restore to the **osticket** schema (database)

*Exit Criteria:*    

-   A connection using MySQL Workbench was made and the osticket database has been successfully restored

### Task 3: Deploy the Virtual Machine Scale Set for the OsTicket application

1.  Using the portal or the Azure Cloud Shell deploy the following template into a resource group named: **OsTicketVMSSRG**: <https://cloudworkshop.blob.core.windows.net/linux-lift-shift/scalesetdeploy.json>

**Note**: It is critical that the deployment be to the same Azure Region (location), as your MySQL cluster deployment. If you select the wrong region you will have to delete and redeploy your Scale Set.

2.  Take note of the settings which has the Admin Username and Admin Password:

    -   Admin Username: **demouser**

    -   OS Admin Password: **demo\@pass123**

3.  Open the Virtual Machine Scale Set in the Azure portal. Review the number of instances and the state of the Autoscaling. Take note of the Public IP address.

4.  Browse to the Public IP address of the Scale Set in a new browser tab. What happens?

5.  Enable Scaling Notification to request emails to Administrators and Co-Administrators of this subscription

*Exit Criteria:*

-   The Virtual Machine Scale Set was deployed with the OsTicket application installed, but when browsing to the application is failed to load with a HTTP/500 error (this is due to lack of connectivity to the MySQL Database)

-   The Scale Set has 1-2 instances currently deployed and is configured with rules to autoscale both up and down based on the percentage of processor used

-   The Administrators and Co-Administrators of this subscription will receive an email when a scaling event occurs on this Scale Set

### Task 4: Connect the MySQLVNet to the Scale Sets VNet

1.  The MySQL Cluster and the Scale Set are running in isolated VNets. To bring the OsTicket application online you will need to create a peering between these VNets.

2.  Once the VNet peering has been connected immediately the two networks can see each other. Verify connectivity between the Scale Set and the MySQL Cluster. Open a new browser tab and attempt to connect to the Public IP address of the Scale Set. You should see the Support Center website again.

    ![On the Microsoft Cloud Workshop tab, the Support Center Ticket Center page displays, and Support Center Home is selected. Two buttons display: Open a New Ticket, and Check Ticket Status.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image24.png "Microsoft Cloud Workshop tab")

3.  Sign in as an Agent and review the My Tickets section of the OsTicket application. Do you see the correct data?

    -   Username: ***demouser***

    -   Password: ***demo\@pass123***

*Exit Criteria:*

-   A connection between the MySQLVNet and the OSTICKETXXVNET was made using VNet peering

-   When this peering connection was established the ScaleSet can now address the database server

### Task 5: Export the osTicket database from the MySQL cluster

1.  Next on the **LABVM** use MySQL Workbench to connect to the MySQL cluster and export the osticket database. Make sure to use **Export to Self-Contained File**, and name the file **C:\\HOL\\mysqlcluster.sql**.

*Exit Criteria:*

-   A connection using MySQL Workbench was made to the MySQL Cluster and the osticket database has been successfully exported

## Exercise 3: Migrate the OsTicket application from Azure IaaS to PaaS

Duration: 60 Minutes

In this exercise, you will implement Phase II of the migration to Azure. Here you will retire the Azure IaaS implementation and move to Azure App Service and Azure Database for MySQL. The first steps will be to build the MySQL DB and again migrate the data using MySQL Workbench. Then you will create the Azure Web App and connect it to GitHub Repo to download the app using a Docker Container with PHP 7.0.

![The Phase 2 diagram has /OsTicket with an arrow pointing to a Resource Group containing Linux, Docker, osTicket, a web client, and MySQL.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image3.png "Phase 2 migration diagram")

### Task 1: Create the MySQL Database

1.  Use the Azure portal to create a new Azure Database for MySQL in a new resource group named **OsTicketPaaSRG**. The database server a name must be globally unique in Azure. Enter the name **demouser** and password **demo\@pass123** for the credentials.

2.  Once the database server is deployed take note of the server FQDN and the Server Admin Login Name

3.  Next configure the MySQL server to be ready for the **osticket** database that you will migrate by changing the Enforce SSL connection to DISABLED. Next, add a firewall rule that allows all IP addresses to connect (0.0.0.0 -- 255.255.255.255).

4.  Locate the Connection Strings for the Web App and save it to a text file. Make sure to update it based on your configuration if needed

*Exit Criteria:*

-   An Azure Database for MySQL Server was created

-   The FQDN, Server Admin Login Name and Connection string were located and saved for use in future steps

-   The Enforce SSL connection setting was changed to DISABLED

-   A firewall rule was created to allow connections from all IP addresses

### Task 2: Restore the osticket database to MySQL PaaS

1.  On the **LABVM**, connect to the Azure Database for MySQL server you created

2.  Enter the following information to configure to connect:

    -   Connection Name: **\<enter your MySQL Server DNS Name -- found in the connection string \>**

    -   Connection Method: **Standard TCP/IP**

    -   MySQL Hostname: **\<enter your MySQL Server DNS Name -- found in the connection string \>**

    -   MySQL Server Port: **3306**

    -   Username: **\<enter your user name -- found in the connection string\>**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![Fields in the Setup New Connection dialog box are set to the previously defined settings.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image25.png "Setup New Connection dialog box")

3.  Once the Workbench loads, click **Server Status**. Review the details of the MySQL PaaS Server.

4.  Restore the database that you exported from the MySQL cluster. Make sure to use the **Import from Self-Contained File** option.

5.  Run a query to on the **ost\_user** table to show that the data was restore to the osticket schema (database). Notice that now the data from the application has been Lifted and Shifted into a MySQL server running in Azure PaaS. This means that there was zero data loss from the move to PaaS.

6.  Move back to the Azure portal and ensure that the osticket database shows in up in the database list

*Exit Criteria:*

-   The osticket database that was exported from the MySQL Cluster was restored to the Azure Database for MySQL Server

-   A query was run showing that there was no data loss

### Task 3: Create the Web App

1.  In the Azure portal, create a new Web App in a new resource group named **OsTicketPaaSRG.** Make sure to choose Linux for the OS and select the PHP 7.0 Docker Container.

*Exit Criteria:*

-   A new Web App was created using the Azure portal. The OS selected was Linux and the Docker Container that was used is PHP 7.0.

### Task 4: Configure the OsTicket Web App

1.  Using the Azure portal, locate and browse to the new Web App

2.  Configure the Connection string of the new Web App to connect to the MySQL osticket database on the PaaS Service

3.  Using your browser and connect to <https://github.com/opsgility/osTicket>. This is a public repo for the OsTicket software. Sign in to your GitHub account or create a new one. Next, Fork the repo to your personal account.

4.  After the repo is forked to your GitHub locate the **include** folder and edit the file named **ost-config.php**. Scroll down to the Database options area of the file and update the text in this file with your MySQL database settings The DBHOST name and the DBUSER should be updated. Once you have updated the text, scroll down enter a command and click **Commit changes**.

    Before:

    ![The configuration options in ost-config.php.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image26.png "Github Before window")

    After:

    ![The configuration options in ost-config.php after the edit.](images/Hands-onlabunguided-Linuxliftandshiftimages/media/image27.png "Github After window")

5.  Configure the Web App to deploy the application using your GitHub osticket repo

6.  Connect again to the **URL** for the Web App and the OsTicket System should open. Sign In as an Agent using:

    -   Username: ***demouser***

    -   Password: ***demo\@pass123***

7.  On the **My Tickets** screen click through to one of the tickets. Once again you see that the data from the IaaS installation of the OsTicket system is preserved which means that you have successfully lifted and shifted the application to Azure PaaS!

*Exit Criteria:*

-   The Connection String was successfully configured on the Web App. This connects the Web App to the MySQL DB.

-   The OsTicket Repo was forked to your Github account. The **ost-config.php** file in the **include** directory of this repo was updated with the MySQL connection information for the server and database. These changes where committed to the GitHub repo.

-   The Web App was configured with a Deployment Option to use the your GitHub repo

-   After a successful deployment from GitHub and the proper database connection configurations the Web App loads in a web browser

-   After Signing in to the Web App, you were able to locate the data and verify that the move to PaaS on Azure was successful with zero data loss

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Linux lift & shift Azure hands-on lab, you will want to delete the Resource Groups. This will free up your subscription from future charges.

-   OsTicketMySQLVMRG

-   OsTicketOnPremRG

-   OsTicketPaaSRG

-   OsTicketVMSSRG

You should follow all steps provided *after* attending the Hands-on lab.

