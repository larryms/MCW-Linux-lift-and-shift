![](images/HeaderPic.png "Microsoft Cloud Workshops")

# Linux lift and shift

## Hands-on lab step-by-step

## March 2018

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

## Contents
<!-- TOC -->

- [Linux lift and shift](#linux-lift-and-shift)
    - [Hands-on lab step-by-step](#hands-on-lab-step-by-step)
    - [March 2018](#march-2018)
    - [Contents](#contents)
    - [Linux lift and shift hands-on lab step-by-step](#linux-lift-and-shift-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Requirements](#requirements)
    - [Help References](#help-references)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Create a virtual machine to execute the lab](#task-1--create-a-virtual-machine-to-execute-the-lab)
        - [Task 2: Install the MySQL Workbench](#task-2--install-the-mysql-workbench)
    - [Exercise 1: Deploy the on-premises OsTicket VM application](#exercise-1--deploy-the-on-premises-osticket-vm-application)
        - [Task 1: Deploy the OnPremVM](#task-1--deploy-the-onpremvm)
        - [Task 2: Export the osticket database](#task-2--export-the-osticket-database)
    - [Exercise 2: Migrate to Azure IaaS VM Scale Sets and MySQL cluster](#exercise-2--migrate-to-azure-iaas-vm-scale-sets-and-mysql-cluster)
        - [Task 1: Deploy the MySQL HA cluster](#task-1--deploy-the-mysql-ha-cluster)
        - [Task 2: Connect to the MySQL cluster and restore the database](#task-2--connect-to-the-mysql-cluster-and-restore-the-database)
        - [Task 3: Deploy the Virtual Machine Scale Set for the OsTicket Application](#task-3--deploy-the-virtual-machine-scale-set-for-the-osticket-application)
        - [Task 4: Connect the MySQLVNet to the Scale Sets VNet](#task-4--connect-the-mysqlvnet-to-the-scale-sets-vnet)
        - [Task 5: Export the osticket database from the MySQL cluster](#task-5--export-the-osticket-database-from-the-mysql-cluster)
    - [Exercise 3: Migrate the OsTicket application from Azure IaaS to PaaS](#exercise-3--migrate-the-osticket-application-from-azure-iaas-to-paas)
        - [Task 1: Create the MySQL database](#task-1--create-the-mysql-database)
        - [Task 2: Restore the osticket database to MySQL PaaS](#task-2--restore-the-osticket-database-to-mysql-paas)
        - [Task 3: Create the Web App](#task-3--create-the-web-app)
        - [Task 4: Configure the OsTicket Web App](#task-4--configure-the-osticket-web-app)
    - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->

## Linux lift and shift hands-on lab step-by-step

## Abstract and learning objectives

In this step-by-step Microsoft Cloud Workshop, you will migrate a Linux based application to the Azure Cloud. This will include the use of Azure IaaS Virtual Machines and Virtual Machine Scale Sets. Azure PaaS will also be leveraged including: Azure App Services (Web App), and Azure Database for MySQL. The student will leverage Azure Resource Manager templates, the Linux custom script extension, Github and a Linux Docker Container in the App Service.

## Overview

In this hands-on step-by-step lab, you will migrate an on-premises based helpdesk application called OsTicket to Azure. This will be a two-phase project to lift and shift the application into Azure IaaS and then migrate it to Azure PaaS. The application is Linux based using Apache, PHP and MySQL (LAMP). During the process of these phases you will ensure zero data loss.

-   **Phase I:** Lift and shift the application from on-premises to Azure IaaS using an auto scaling Virtual Machine Scale Set and a MySQL cluster with 3 nodes.

-   **Phase II:** Migrate to PaaS using Azure App Services with a Linux Docker Container and Azure Database for MySQL.

**Phase I will result in an environment resembling this diagram:**

![The Phase I diagram has two Resource Groups, which are connected by VNet Peering. The first Resource Group is MySQLVNet, and has MySQL, bitnami, and Linux. The second Resource Group is OsticketVNet, and has Linux, Apache, PHP, and Osticket. ](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image3.png "Phase I diagram")

**Phase II will result in an environment resembling this diagram:**

![The Phase II diagram has /OsTicket with an arrow pointing to a Resource Group containing Linux, Docker, osTicket, a web client, and MySQL.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image4.png "Phase II diagram")

## Requirements

You must have a working Azure subscription to carry out this hands-on Lab step-by-step.

## Help References

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

## Before the hands-on lab

Duration: 30 Minutes

### Task 1: Create a virtual machine to execute the lab

1.  Launch a browser and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or just a Microsoft Account.

2.  Click on **+NEW**, and in the search box type in **Visual Studio Community 2017 on Windows Server 2016 (x64)** and press enter. Click the Visual Studio Community 2017 image running on Windows Server 2016 and with the latest update.

3.  In the returned search results, click the image name.

    ![In the Everything blade, the search field displays Visual Studio Community 2017 on Windows Server 2016 (x64). Under Results, Visual Studio Community 2017 on Windows Server 2016 (x64) is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image5.png "Everything blade")

4.  Click **Create**.

5.  Set the following configuration on the Basics tab and click **OK**.

    -   Name: **LABVM**

    -   VM disk type: **SSD**

    -   User name: **demouser**

    -   Password: **demo\@pass123**

    -   Subscription: **If you have multiple subscriptions choose the subscription to execute your labs in.**

    -   Resource Group: **OPSLABRG**

    -   Location: **Choose the closest Azure region to you.**

    ![Fields in the Basics blade display with the previously defined settings.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image6.png "Basics blade")

6.  Choose the **DS1\_V2 Standard** instance size on the Size blade.

**Note**: You may have to click the View All link to see the instance sizes.

![In the Choose a size blade, the DS1\_V2 Standard option is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image7.png "Choose a size blade")

**Note**: If the Azure Subscription you are using is [NOT]{.underline} a trial Azure subscription you may want to chose the DS2\_V2 to have more power in this LABMV. If you are using a Trial Subscription or one that you know has a restriction on the number of cores stick with the DS1\_V2.

7.  Click **Storage Account** *Configure required settings* to specify a storage account for your virtual machine if a storage account name is not automatically selected for you.

    ![In the Settings blade, the Storage account option is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image8.png "Settings blade")

8.  Click **Create New**

    ![Screenshot of the Create new button.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image9.png "Create new button")

9.  Specify a unique name for the storage account (all lower letters and alphanumeric characters) and ensure the green checkmark shows the name is valid.

    ![Next to the Name field, the Green checkmark icon is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image10.png "Green checkmark icon")

10. Click **OK** to continue.

11. Click **Diagnostics Storage Account** *Configure required settings* for the Diagnostics storage account if a storage account name is not automatically selected for you. Repeat the previous steps to select a unique storage account name. This storage account will hold diagnostic logs about your virtual machine that you can use for troubleshooting purposes.

    ![Screenshot of the Diagnostics Storage Account option.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image11.png "Diagnostics Storage Account option")

12. Accept the remaining default values on the Settings blade and click **OK**. On the Summary page click **Create**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    ![Screenshot of the Deploying Visual Studio icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image12.png "Deploying Visual Studio icon")

**Note**: Please wait for the LABVM to be provisioned prior to moving to the next step.

13. Move back to the Portal page on your local machine and wait for **LABVM** to show the Status of **Running**. Click **Connect** to establish a new Remote Desktop Session.

    ![The Connect button is selected on the Portal page top menu.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image13.png "Portal page top menu")

14. Depending on your remote desktop protocol client and browser configuration you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

15. Log in with the credentials specified during creation:

    a.  User: **demouser**

    b.  Password: **demo\@pass123**

16. You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click **Yes** to continue with the connection.

    ![The Remote Desktop Connection dialog box displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image14.png "Remote Desktop Connection dialog box")

17. When logging on for the first time you will see a prompt on the right asking about network discovery. Click **No**.

    ![A Network Diagnostics prompt displays, asking if you want to find PCs, devices, and content on this network and automatically connect.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image15.png "Network Diagnostics prompt")

18. Notice that Server Manager opens by default. On the left, click **Local Server**.

    ![On the Server Manager menu, Local Server is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image16.png "Server Manager menu")

19. On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![In the Essentials section, IE Enhanced Security Configuration is selected, and set to On.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image17.png "Essentials section")

20. Change to **Off** for Administrators and click **OK**.

    ![In the Internet Explorer Enhanced Security Configuration dialog box, Administrators is set to Off.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image18.png "Internet Explorer Enhanced Security Configuration dialog box")

### Task 2: Install the MySQL Workbench

1.  While logged into **LABVM** via remote desktop, open Internet Explorer and navigate to <https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-6.3.10-winx64.msi> this will download an executable. After the download is finished, click **Run** to execute it.

2.  Follow the directions of the installer to complete the installation of MySQL Workbench.

3.  After the installation is complete, **reboot** the machine.

You should follow all steps provided *before* attending the Hands-on lab.

## Exercise 1: Deploy the on-premises OsTicket VM application

Duration: 45 Minutes

In this exercise, you will deploy a VM using an ARM template that will act as the on-premises installation of the OsTicket application. This will consist of a single Ubuntu Linux 16.04-LTS VM with Apache, PHP and MySQL installed (LAMP). This application is a helpdesk management software that you will lift and shift into Azure. There is sample data in the application that will be retained throughout the two phases of the project.

![This Resource group for OnPremVNet includes the previously mentioned products.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image19.png "Resource group")

### Task 1: Deploy the OnPremVM

1.  From the Azure portal, click on the Cloud Shell icon on the top navigation.

    ![Screenshot of the Launch Cloud shell icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image20.png "Launch Cloud shell icon")

2.  Execute the following command to create a resource group that will contain the application. 

**Note**: You can also specify an alternate region.

    az group create --name OsTicketOnPremRG --location "East US"

3.  Execute the following command to deploy the ARM template.

    az group deployment create --name OsTicketOnPremRG --resource-group OsTicketOnPremRG --template-uri " https://cloudworkshop.blob.core.windows.net/linux-lift-shift/onpremvmdeploy.json" 

4.  This deployment will take about 5 minutes to complete. Wait until it has been deployed before moving on to the next step.

5.  Once the deployment has completed open the resource group **OsTicketOnPremRG** and review the deployment.

    ![In the Resource Group blade, Overview is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image21.png "Resource Group blade")

6.  Next click on the **onpremvmip** Public IP address. Locate the IP address and paste this into a new tab of your web browser. The Support Center OsTicket application should load.

    ![On the Microsoft Cloud Workshop tab, the Support Center displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image22.png "Microsoft Cloud Workshop tab")

7.  Click the Sign in link.

    ![Screenshot of the Sign In button.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image23.png "Sign In button")

8.  Locate **I'm an agent** and click the **sign in here** link.

    ![On the Sign in to Microsoft Cloud Workshop Registration page, the Sign in here link was selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image24.png "Sign in to Microsoft Cloud Workshop Registration page")

9.  At the OsTicket screen enter the **username** and **password** and click **Log In**.

    a.  Username: ***demouser***

    b.  Password: ***demo\@pass123***

    ![The osTicket log in webpage displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image25.png "osTicket log in webpage")

10. Once logged into the OsTicket system click **My Tickets**.

    ![On the osTicket system page, on the Tickets tab, My Tickets is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image26.png "osTicket system page")

11. On the **My Tickets** screen click through to one of the tickets.

    ![On the osTicket system page, on the Tickets tab, details for a specific ticket display.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image27.png "osTicket system page")

12. Next, Click the Users Tab and notice the users that are entered into the system.

    ![On the osTicket system page, the Users tab is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image28.png "osTicket system page")

13. Feel free to create new tickets or new users to add to your dataset.

### Task 2: Export the osticket database

1.  From your **LABVM**, connect to the Azure portal and then open the **OsTicketOnPremRG** resource group.

    ![Screenshot of the OsTicketOnPremRG option.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image29.png "OsTicketOnPremRG option")

2.  Locate the **onpremvmip** Public IP address and **take note of the address**.

    ![In the Public IP address blade, Overview is selected. Under Essentials, the IP address is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image30.png "Public IP address blade")

3.  Next on the LABVM click Start and then locate the MySQL Workbench.

    ![On the Start menu, MySQL Workbench 6.3 CE is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image31.png "Start menu")

4.  Click the Plus sign next to MySQL Connections on the Workbench.

    ![The plus sign on the My SQL Collections option is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image32.png "My SQL Collections option")

5.  Enter the following information to configure to connect to your OnPremVM**.**

    -   Connection Name: **OnPremVM**

    -   Connection Method: **Standard TCP/IP over SSH**

    -   SSH Hostname: **\<enter the Public ip address\>**

    -   SSH Username: **demouser**

    -   SSH Password: **Click Store in Vault: demo\@pass123**

    -   MySQL Hostname: **127.0.0.1**

    -   MySQL Server Port: **3306**

    -   Username: **osticket**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![The Setup New Connection page displays with fields set to the previously defined settings.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image33.png "Setup New Connection page")

6.  Once configured click **Test Connection**.

7.  A popup will appear with a notice that the **SSH Server Fingerprint Missing**, click **continue**.

    ![The MySQL Workbench popup displays, with the Continue button selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image34.png "MySQL Workbench popup")

8.  If configured correctly you will receive a message: **Successfully made the MySQL Connection**. Click OK.

    ![The MySQL Workbench popup displays, with the OK button selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image35.png "MySQL Workbench popup")

9.  The Connection will appear. Double-click to start a session with the MySQL instance running on the **OnPremVM**.

    ![Under MySQL Connections, the OnPremVM option is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image36.png "MySQL Connections ")

10. Once the Workbench loads, click **Server Status**. Review the details of the server.

    ![On the MySQL Workbench, under Management, Server Status is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image37.png "MySQL Workbench")

11. Next, click the **osticket** database under Schemas and expand to see the tables.

    ![Under Schemas, osticket is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image38.png "Schemas ")

12. Locate the **ost\_user** table, **right-click** and then click **Select Rows -- Limit 1000**.

    ![The ost\_user table is selected. From its menu, Select Rows - Limit 1000 is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image39.png "ost_user table")

13. This will launch a query in the Workbench and list all of the users on the system. Notice that these are the same users that you saw in the OsTicket UI.

    ![In the Workbench, a query displays with same users.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image40.png "Workbench query")

14. Click **Data Export**.

    ![Under Management, Data Export is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image41.png "Management section")

15. On the **Data Export** screen, select the **osticket**. Then select **Export to Self-Contained File**, and named the file **c:\\HOL\\onpremvm.sql** and click **Start Export**.

    ![In the Data Export window, under Tables to Export, the check box for osticket is selected. The Export to Self-Contained file is selected, and the address is C:\\HOL\\onpremvm.sql.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image42.png "Data Export window")

16. You will get a version mismatch warning, click **Continue Anyway**.

    ![In the MySQL Workbench popup, information displays about a version mismatch. The Continue Anyway button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image43.png "MySQL Workbench popup")

17. Once the export has completed you will receive this message.

    ![On the data Export page, Object Selection tab, a progress bar shows that the export has completed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image44.png "data Export page")

## Exercise 2: Migrate to Azure IaaS VM Scale Sets and MySQL cluster

Duration: 90 Minutes

In this exercise, you will deploy the OsTicket application to Azure IaaS. In the first task, you will deploy and configure a MySQL Cluster with multiple nodes. Next, you will restore the database that was exported using MySQL Workbench. After the data tier of your application has been configured you will deploy a Virtual Machine Scale Set. The OsTicket application will be installed on the instances using the Custom Linux Extension and will be self-configuring. The Scale Set will be setup to autoscale and the VMs that spin up will also self-configuring.

![The Phase I diagram has two Resource Groups, which are connected by VNet Peering. The first Resource Group is MySQLVNet, and has MySQL, bitnami, and Linux. The second Resource Group is OsticketVNet, and has Linux, Apache, PHP, and Osticket. ](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image3.png "Phase I diagram")

### Task 1: Deploy the MySQL HA cluster

1.  From the Azure portal, click on the Cloud Shell icon on the top navigation.

    ![Screenshot of the Launch Cloud shell icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image20.png "Launch Cloud shell icon")

2.  Execute the following command to create a resource group that will contain the MySQL HA environment. 

**Note**: Ensure you that use the same region as the OsTicket application.

    az group create --name OsTicketMySQLVMRG --location "East US"

3.  Execute the following command to deploy the ARM template.

    az group deployment create --name OsTicketMySQLVMRG --resource-group OsTicketMySQLVMRG --template-uri "https://cloudworkshop.blob.core.windows.net/linux-lift-shift/mysqlhadeploy.json" 

**Note**: The settings that are being deployed as part of the template will be referenced later in the lab.

-   OS User Name: **bitnami**

-   OS Admin Password: **demo\@pass123**

-   App Database: **osticket**

-   App Password: **demo\@pass123**

4.  This deployment will take about 15 minutes to complete. Wait until it has been deployed before moving on to the next step.

5.  Once the deployment has completed open the **OsTicketMySQLVMRG** and review the deployment. Notice that there are three VMs which are a part of a three node MySQL cluster.

    ![In the left pane of the Resource Group blade, Overview is selected. In the right pane, under Name, three VMs are circled: osticket0, osticket1, and osticket2.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image45.png "Resource Group blade")

6.  Locate the **osticketip** Public IP address and **take note of the address**. Notice that this is attached the VM **osticket0** which is the master node in the cluster.

    ![In the Public IP address blade, under Essentials, the IP address 52.179.81.179 is circled, and a callout points to the virtual machine name osticket0.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image46.png "Public IP address blade")

### Task 2: Connect to the MySQL cluster and restore the database

1.  On the **LABVM**, click Start and then locate the MySQL Workbench.

    ![On the Start menu, MySQL Workbench 6.3 CE is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image31.png "Start menu")

2.  Click the Plus sign next to MySQL Connections on the Workbench.

    ![The plus sign next to MySQL Connections is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image32.png "MySQL Connections option")

3.  Enter the following information to configure to connect to your MySQL master node.

    -   Connection Name: **MySQL Cluster**

    -   Connection Method: **Standard TCP/IP over SSH**

    -   SSH Hostname: **\<enter the Public ip address\>**

    -   SSH Username: **bitnami**

    -   SSH Password: Click Store in Vault: **demo\@pass123**

    -   MySQL Hostname: **127.0.0.1**

    -   MySQL Server Port: **3306**

    -   Username: **root**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![Fields in the Setup New Connection dialog box are set to the previously defined settings.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image47.png "Setup New Connection dialog box")

4.  Once configured click **Test Connection**.

5.  A popup will appear with a notice that the **SSH Server Fingerprint Missing**, click **continue**.

    ![A MySQL Workbench popup displays warning you that the SSH Server Fingerprint is missing. The Continue button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image48.png "MySQL Workbench warning popup")

6.  If configured correctly you will receive a message: **Successfully made the MySQL Connection**, Click **OK**.

    ![A MySQL Workbench success popup displays, stating that it successfully made the MySQL connection.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image49.png "MySQL Workbench success popup ")

7.  Click **OK** to save the connection that you just configured.

8.  The Connection will appear. Double-click to start a session with the MySQL Cluster instance running on the Azure IaaS.

    ![Screenshot of the MySQL Cluster connection.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image50.png "MySQL Cluster connection")

9.  Once the Workbench loads, click **Server Status**. Review the details of the cluster. Remember you are connected to the master for the cluster.

    ![The MySQL Workbench displays. In the left pane, under Management, Server Status is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image51.png "MySQL Workbench")

10. Click the New Query button.

    ![On the MySQL Workbench menu, the New Query button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image52.png "MySQL Workbench menu")

11. Execute the following query to show the status of the cluster nodes known as "Slaves". Click the **Lighting Bolt** to run the query. Notice that the two replication partners appear in the results window as well as their TCP port of 3306.
    ```
    SHOW SLAVE HOSTS
    ```
    ![On MySQL Workbench, the run query (lightening bolt) icon is selected. A callout points to the Query results, which contains the two replication partners.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image53.png "MySQL Workbench")

12. Next, click the **osticket** database under Schemas and expand to see the tables. This time notice that nothing is in the database yet since you have not restored it yet.

    ![Under Schemas, the osticket database is expanded, and Tables is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image54.png "Schemas section")

13. This is the part of the lift and shift where we will restore the existing database for the application. Click the **Data Import/Restore** button.

    ![Under Management, Data Import/Restore is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image55.png "Management section")

14. On the Data Import screen click the **Import from Self-Contained File**, and then select the **C:\\HOL\\onpremvm.sql** datafile.

    ![On the Data Import page, on the Import from Disk tab, the option to Import from self-contained file is selected, and the file location is c:\\HOL\\ompremvm.sql.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image56.png "Data Import page")

15. Next, set the Default Target Schema as **osticket** and click **Start Import**. This will restore the data from the **OnPremVM** to the MySQL cluster.

    ![On the Default Schema to be Imported To page, the Default Target Schema is set to osticket.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image57.png "Default Schema to be Imported To")

16. Once the restore is completed the following screen will appear.

    ![On the MySQL Cluster Data Import page, on the Import Progress tab, status shows as import completed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image58.png "MySQL Cluster Data Import page")

17. Move back to the Schemas area of the MySQL Workbench, and click the refresh icon.

    ![Under Schemas, ost\_user is selected, and from its right-click menu, Select Rows-Limit 1000 is selected..](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image59.png "Schemas section")

18. The tables of the database now appear since they have been restored. Locate the **ost\_user** table, **right-click** and then click **Select Rows -- Limit 1000**.

    ![The ost\_user table is selected. From its menu, Select Rows - Limit 1000 is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image39.png "ost_user table")

19. This will launch a query in the Workbench and list all the users on the system. Notice that now the data from the application has been Lifted and Shifted into a MySQL Cluster running in Azure IaaS. This means that there was zero data loss from the move to the cluster.

    ![On the Workbench, on the ost\_user tab, a callout points to the query results.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image40.png "Workbench query")

### Task 3: Deploy the Virtual Machine Scale Set for the OsTicket Application

1.  From the Azure portal, click on the Cloud Shell icon on the top navigation.

    ![Screenshot of the Launch Cloud shell icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image20.png "Launch Cloud shell icon")

2.  Execute the following command to create a resource group that will contain the MySQL HA environment. 

**Note**: Ensure you that use the same region as the OsTicket application.

    az group create --name OsTicketVMSSRG --location "East US"

3.  Execute the following command to deploy the ARM template. This command requires a parameter to be passed to the template. In the example, osticketXX is used. Replace that value with a unique value that is lowercase and less than 10 characters.

    az group deployment create --name OsTicketVMSSRG --resource-group OsTicketVMSSRG --template-uri "https://cloudworkshop.blob.core.windows.net/linux-lift-shift/scalesetdeploy.json" --parameters vmssName=osticketXX

Take note of the credentials for the VMSS.

-   Admin Username: **demouser**

-   OS Admin Password: **demo\@pass123**

4.  Once the deployment has completed open the **OsTicketVMSSRG** and review the deployment.

    ![In the OsTicketVMSSRG Resource Group blade, in the left pane, Overview is selected. In the right pane, four items display under Name: osticket1 (vm scale set), osticket11lb (load balancer), osticket11pip (public IP address), and osticket11vnet (virtual network).](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image60.png "Resource Group blade")

5.  Open the Virtual Machine Scale Set. Notice that there are two instances (to start out with), which are a part of a VM Scale Set and that autoscaling has been enabled. After some time, it will be scaled down to only one instance given the lack of traffic to the site.

    ![In the Virtual Machine Scale Set blade, under Essentials, a callout points to Autoscaling, which is currently On.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image61.png "Virtual Machine Scale Set blade")

6.  Take note of the Public IP address.

7.  If you browse to the IP address if a new tab you will not be able to connect to the webpage. This is due to the lack of connectivity to the MySQL Cluster.

8.  Click the Instances link in Settings.

    ![Under Settings, Instances is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image62.png "Settings section")

9.  Here you can take different actions on the Scale Set instances. Click on the instances, and review the information pane.

    ![Under Name, osticket11\_0 has a status of running.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image63.png "Scale Set instance")

10. In the information pane notice the "Latest model applied" information. This is where you can update the image being used for the OS. By selecting the instances and then clicking the update, you can apply system updates to the VMs that are currently running.

    ![The osticket11\_0 information pane displays. A callout points Latest model applied, which is set to Yes.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image64.png "Information pane")

11. Click the Scaling link in the Settings area of the Scale Set.

    ![Under Settings, Scaling is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image65.png "Settings section")

12. Review the Rules for Scale Out and Scale In.

    ![The rules for scale in and scale out display.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image66.png "Scale in and out rules")

13. Click the **Notify** tab on this page, click the **Email Administrators**, **Email co-administrators**, and add any additional emails you wish. Also, notice that a Webhook could be entered here. Anytime an event happens with autoscaling Azure will send a notification. Click **Save** to configure.

    ![On the Notify tab, administrators and co-administrators are both selected to be emailed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image67.png "Notify tab")

### Task 4: Connect the MySQLVNet to the Scale Sets VNet

The MySQL Cluster and the Scale Set and running in isolated VNets. To bring the OsTicket application online, you will need to create a peering between these VNets. This was the reason that both deployments needed to be in the same regions along with performance hit if they were in different regions.

1.  In the Azure portal, click on **Virtual Networks** followed by **MySQLVNet** and **Peerings**.

    ![In the Virtual networks blade, under Name, MySQL VNet is selected. In the Virtual Network blade, under Settings, Peerings is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image68.png "Virtual networks and Virtual Network blades")

2.  Click **+Add**.

    ![The Add button is selected in the Virtual Network blade.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image69.png "Virtual Network blade")

3.  Enter the name **OSTicketPeering** followed by **Choose a virtual network**. Select the VNet you created with your Scale Set. Click **OK**.

    ![In the Add peering blade, the Name field displays OSTICKETPeering, and the Virtual Network (Choose a virtual network) option is selected. In the Choose a virtual network blade, the osticket11vnet virtual network is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image70.png "Add peering and Choose a virtual network blades")

4.  Your peering will appear in the blade as Initiated (you may have to refresh the blade to see this update).

    ![In the Virtual network blade, a callout points out that OSTICKETPeering now has a peering status of initiated.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image71.png "Virtual network blade")

5.  In the Azure portal click on **Virtual Networks** followed by the name of your **Scale Set VNet** and **Peerings**. In the case of the example, the VNet is named **osticket11vnet**.

    ![In the Virtual networks blade, under Name, osticket11vnet is selected. In the Virtual Network blade, under Settings, Peerings is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image72.png "Virtual networks and virtual network blades")

6.  Click **+Add**.

    ![the Add button is selected in the Visual network blade.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image73.png "Visual network blade")

7.  Enter the name **OSTicketPeering** followed by **Choose a virtual network**. Select the VNet that you created with your Scale Set. Click **OK**.

    ![OSTICKETPeering displays in the Name field of the Add peering blade, and Virtual network, Choose a virtual network is selected. In the Choose virtual network blade, MySQLVNet is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image74.png "Add peering and Choose virtual network blades")

8.  Your peering will appear in the blade as Connected (you may have to refresh the blade to see this update).

    ![The Add button is selected in the Virtual Network blade.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image75.png "Virtual Network blade")

9.  Immediately, the two networks can see each other which means the Scale Set will be able to see the MySQL Cluster. Open a new browser tab and attempt to connect to the Public IP address of the Scale Set. You should see the Support Center website again.

    ![The Support Center website displays with two button options: Open a New Ticket, or Check Ticket Status.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image22.png "Support Center website")

10. Click the Sign in link.

    ![Screenshot of the Sign in link.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image23.png "Sign in link")

11. Locate **I'm an agent**, and click the **sign in here** link.

    ![On the Sign in to Microsoft Cloud Workshop page, next to I\'m an agent, the Sign in here link is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image24.png "Sign in to Microsoft Cloud Workshop page")

12. At the OsTicket screen enter the **username** and **password**, and click **Log In**.

    a.  Username: ***demouser***

    b.  Password: ***demo\@pass123***

    ![The osTicket log in webpage displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image25.png "osTicket log in webpage")

13. Once logged into the OsTicket system, click **My Tickets**.

    ![On the osTicket page, tickets tab, My Tickets (4) is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image26.png "osTicket page, tickets tab")

14. On the **My Tickets** screen, click through to one of the tickets. Once again, you see that the data from the on-premises installation of the OsTicket system is preserved which means that ***you have successfully lifted and shifted the application to Azure IaaS!***

    ![The My tickets page displays the details of a ticket.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image27.png "My tickets page")

### Task 5: Export the osticket database from the MySQL cluster

1.  Next on the **LABVM** click Start and then locate the MySQL Workbench.

    ![On the Start menu, MySQL Workbench 6.3 CE is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image31.png "Start menu")

2.  Double-click to start a session with the MySQL cluster.

    ![Screenshot of the MySQL Cluster connection.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image50.png "MySQL Cluster connection")

3.  Click **Data Export**.

    ![Under Management, Data Export is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image41.png "Management section")

4.  On the **Data Export** screen, select the **osticket**. Then select **Export to Self-Contained File**, and name the file **C:\\HOL\\mysqlcluster.sql** and click **Start Export**.

    ![On the Administration - Data Export page, on the Object selection tab, under Tables to Export, osticket is selected. Under Export options, Export to Self-Contained File is selected, and the file location is C:\\HOL\\mysqlcluster.sql.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image76.png "Administration - Data Export page")

5.  You will get a version mismatch warning, click **Continue Anyway**.

    ![A MySQL Workbench warning popup displays letting you know there is a version mismatch. The continue anyway button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image43.png "MySQL Workbench warning popup")

6.  Once the export has completed, you will receive this message.

    ![On the Administration - Data Export page, on the Export Progress tab, the status shows that export has completed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image77.png "Administration - Data Export page")

## Exercise 3: Migrate the OsTicket application from Azure IaaS to PaaS

Duration: 60 Minutes

In this exercise, you will implement Phase II of the migration to Azure. Here you will retire the Azure IaaS implementation and move to Azure App Service and Azure Database for MySQL. The first steps will be to build the MySQL DB and again migrate the data using MySQL Workbench. Then, you will create the Azure Web App and connect it to GitHub Repo to download the app using a Docker Container with PHP 7.0.

![The Phase II diagram has /OsTicket with an arrow pointing to a Resource Group containing Linux, Docker, osTicket, a web client, and MySQL.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image4.png "Phase II diagram")

### Task 1: Create the MySQL database

1.  From the Azure portal, click on the Cloud Shell icon on the top navigation.

    ![Screenshot of the Launch Cloud shell icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image20.png "Launch Cloud shell icon")

2.  Execute the following command to create a resource group to contain the MySQL DB.

    az group create --name OsTicketPaaSRG --location "East US"

3.  Execute the following command to create a MySQL Database. 
  
**Note**: You must choose a unique name for the MySQL server. Replace **osticketsrv01** with a more unique value.

    az mysql server create --resource-group OsTicketPaaSRG --name osticketsrv01 --location "East US" --admin-user demouser --admin-password demo@pass123 --performance-tier Basic --storage-size 51200 --ssl-enforcement Disabled

4.  Add an open firewall rule to the database by executing the following command. Ensure you replace the server name with the unique value from the previous step.

    az mysql server firewall-rule create --resource-group OsTicketPaaSRG --server-name osticketsrv01 --name Internet --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255

5.  Once the MySQL database has been deployed, locate and open it from the **OsTicketPaaSRG** resource group using the Azure Portal.

6.  Click **Connection Strings**.

    ![Under Settings, Connection strings is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image78.png "Settings section")

7.  Locate the Web App script, and press the **Click the copy** button.

    ![The Web App script\'s copy button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image79.png "Web App script")

8.  Open a new notepad window and paste this into a new file to retain this string and more information in the next few steps. Update the **database** section to **osticket** and the **password** section to **demo\@pass123.**

    ![In the Notepad window, the Web App Script displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image80.png "Notepad window")

9.  Click Overview for the MySQL server.

    ![On the Azure Database for MySQL Server blade, Overview is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image81.png "Azure Database for MySQL Server blade")

10. Notice the **Server name** and **Server Admin Login** name. You can compare them the connection string that you copied into the text file (they should be the same).

    ![Under Essentials, the Server name and Server admin login name are circled. The Server name is osticketmysql.mysql.database.azure.com, and the login name is demouser\@osticketmysql](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image82.png "Essentials section")

11. Scroll down, and notice that there are currently four databases that are running on your server.

    ![In the Databases section, MySQL databases hsa a number 4.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image83.png "Databases section")

### Task 2: Restore the osticket database to MySQL PaaS

1.  On the **LABVM**, click Start and then, locate the MySQL Workbench.

    ![On the Start menu, MySQL Workbench 6.3 CE is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image31.png "Start menu")

2.  Click the Plus sign next to MySQL Connections on the Workbench.

    ![The plus sign next to MySQL Connections is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image32.png "MySQL Connections option")

3.  Enter the following information to configure to connect to your Server**.**

    -   Connection Name: **\<enter your MySQL Server DNS Name -- found in the connection string \>**

    -   Connection Method: **Standard TCP/IP**

    -   MySQL Hostname: **\<enter your MySQL Server DNS Name -- found in the connection string \>**

    -   MySQL Server Port: **3306**

    -   Username: **\<enter your user name -- found in the connection string\>**

    -   Password: **Click Store in Vault: demo\@pass123**

    ![Fields in the Setup New Connection dialog box are set to the previously defined settings.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image84.png "Setup New Connection dialog box")

4.  Once configured, click the Test Connection Button.

    ![Screenshot of the Test Connection button.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image85.png "Test Connection button")

5.  If configured correctly you will receive a message: **Successfully made the MySQL Connection**, Click **OK**.

    ![A MySQL Workbench success popup displays, informing you that the MySQL connection was successfully made.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image86.png "MySQL Workbench success popup")

6.  Click **OK** to save the connection that you just configured.

7.  The Connection will appear. Double-click to start a session with the MySQL database server running on the Azure PaaS.

    ![Screenshot of the Connection option.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image87.png "Connection option")

8.  Once the Workbench loads, click **Server Status**. Review the details of the MySQL PaaS Server.

    ![On the MySQL Workbench, under Management, Server Status is selected. In the right pane, details for osticketmysql.mysql.database.azure.com display.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image88.png "MySQL Workbench")

9.  This is the part of the lift and shift where we will restore the existing database for the application. Click the **Data Import/Restore** button.

    ![Under Management, Data Import/Restore is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image55.png "Management section")

10. On the Data Import screen, click the **Import from Self-Contained File**, and select the **c:\\HOL\\mysqlcluster.sql** datafile.

    ![On the Data Import page, on the Import from Disk tab, under Import options, Import from Self-Contained File is selected, and the location is C:\\HOL\\mysqlcluster.sql.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image89.png "Data Import page")

11. Click New, next to the **Default Schema to be Imported To**.

    ![On the Data Import page, on the Import from Disk tab, the New button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image90.png "Data Import page")

12. On the Create Schema menu, type **osticket** and click OK.

    ![Name of schema to create field, osticket is typed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image91.png "Name of schema to create field")

13. MySQL Workbench will create the Schema (database), on the server for you and select it as the Default Target Schema for the restore.

    ![The Default Target Schema field is set to osticket.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image92.png "Default Target Schema field")

14. Click **Start Import** after reviewing the screen.

    ![On the Data Import page, on the Import from Disk tab, the Start Import button is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image93.png "Data Import page")

15. Once the restore is completed, the following screen will appear.

    ![On the Data Import page, Import Progress tab, the status shows as import completed.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image94.png "Data Import page")

16. Move back to the Schemas area of the MySQL Workbench, and click the refresh icon.

    ![Under Schemas, both osticket and the refresh icon are selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image59.png "Schemas section")

17. The tables of the database now appear since they have been restored. Locate the **ost\_user** table, **right-click**, and click **Select Rows -- Limit 1000**.

    ![In the List of database tables, ost\_user is selected, and from its right-click menu, Select Rows - Limit 1000 is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image39.png "List of database tables")

18. This will launch a query in the Workbench and list all of the users on the system. Notice that now the data from the application has been Lifted and Shifted into a MySQL server running in Azure PaaS. This means there was zero data loss from the move to PaaS.

    ![On the ost\_user tab, the query displays in the pane above, and results display in the Results Grid below.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image40.png "ost_user query results")

19. Move back to the Azure portal, and click Overview for the MySQL server.

    ![On the Azure Database for MySQL server blade, Overview is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image81.png "Azure Database for MySQL server blade")

20. Scroll down and notice now, there are five databases and the addition of the **osticket**.

    ![In th eDatabases section, under MySQL databases, the number 5 now displays. Under Name, the following five databases are listed: information\_schema, mysql, osticket, performance\_schema, and sys.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image95.png "Databases section")

### Task 3: Create the Web App

1.  From the Azure portal, click on the Cloud Shell icon on the top navigation.

    ![Screenshot of the Launch Cloud shell icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image20.png "Launch Cloud shell icon")

2.  Execute the following command to create a Linux-based App Service Plan for the new web app.

    az appservice plan create -n OsTicket -g OsTicketPaaSRG --is-linux -l "East US 2" --sku S1 --number-of-workers 1

3.  Execute the following command to create a new web app configured for PHP 7.0 inside of the new app service plan. The name of the web app must be unique, so specify some numbers at the end to make it a more unique value.

    az webapp create -n osticketsystem -g OsTicketPaaSRG -p OsTicket -r "php|7.0"

4.  Once the deployment has completed, open the **OsTicketPaaSRG** resource group. Notice there are now three objects: **MySQL database, Linux App Service Plan** and the **Web App**.

    ![On the Resource group blade, Overview is selected. Under Essentials, a callout points to the OsTicket App Service plan.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image96.png "Resource group blade")

### Task 4: Configure the OsTicket Web App

1.  Open the Web App using the Azure portal. Notice the details of the application including the **URL**.

    ![On the App Service blade, Overview is selected. Under Essentials, a callout points to the URL live link http://osticketsystem.azurewebsites.net.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image97.png "App Service blade")

2.  If you click the **URL**, the default webpage will load.

3.  In the Azure portal, click **Application settings** in the Settings area.

    ![Under Settings, Application settings is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image98.png "Settings section")

4.  Locate the Connection Strings section. Enter the name **osticket**, and copy the string into the **value area** from notepad. Select **MySQL** in the dropdown list next to the string. Click **Save**.

    ![Under Connection strings, the osticket connection string displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image99.png "Connection strings section")

5.  Open a new browser tab and connect to <https://github.com/opsgility/osTicket>. This is a public repo for the OsTicket software. Sign in to your GitHub account or create a new one.

    ![On the GitHub webpage, a code tab displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image100.png "GitHub webpage")

6.  On this page locate and then click the **Fork** button.

    ![Screenshot of the Fork button.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image101.png "Fork button")

7.  If prompted, select your personal account when prompted with **"Where should we fork this repository?"**

    ![Under Forking opsgility/osticket, a refresh button displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image102.png "Refresh icon")

8.  After the repo is forked to your GitHub account, scroll down and locate the **include** folder and click it.

    ![In a list of folders, the Include folder is selected..](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image103.png "List of folders")

9.  Once in the **include** folder, scroll down and locate the file named **ost-config.php**.

    ![In a list of files, ost-config.php is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image104.png "list of files")

10. The file will open in the browser. Click the **Pencil** icon to edit this file.

    ![Screenshot of the Pencil (edit) icon.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image105.png "Edit icon")

11. The file with open in an editor. Scroll down to the Database Options area of the file. Update the text in this file with your MySQL database settings from your notepad file. The **DBHOST** name and the **DBUSER** should be updated. See below for the before and after comparison.

    Before:

    ![Screenshot of the Github Before window. At this time, we are unable to capture all of the information in the Github window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image106.png "Github Before window")

    After:

    ![Screenshot of the Github After window. At this time, we are unable to capture all of the information in the Github window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image107.png "Github After window")

12. Once you have updated the text, scroll down enter a command and click **Commit changes**.

    ![The text field under Commit changes reads, \"Updated MySQL Server Settings.\" The option to Commit directly to the master branch is selected, as is the Commit Changes button at the bottom. ](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image108.png "Commit changes section")

13. Move back to the Azure portal on the Web App and click **Deployment options** under the Deployment area.

    ![Under Deployment, Deployment options is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image109.png "Deployment section")

14. Click **Choose Source**, *Configure required settings*

    ![Screenshot of the Choose Source, Configure required settings option.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image110.png "Choose Source option")

15. Click GitHub.

    ![Under Choose source, GitHub is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image111.png "Choose source section")

16. Click Authorization if you have not connected your GitHub account to the Azure portal follow the prompts.

17. Click Choose your organization if your GitHub personal account is not shown.

18. Click Choose project.

    ![Screenshot of the Choose Project, Configure required settings option.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image112.png "Choose project option")

19. Select the **osticket** repo.

    ![On the Choose project blade, osticket is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image113.png "Choose project blade")

20. Configure your selections and then click **OK.**

    ![The Deployment option blade displays, ](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image114.png "Deployment option blade")

21. The OsTicket application will be downloaded from the GitHub account. First, it will show as Pending and then Active. It may take a minute for it to appear in Deployment Options.

    ![In the App Service blade, the Fetch status is downloading.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image115.png "App Service blade")

    ![In the App Service blade, a message displays saying that the MySQL Server Settings are now updated.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image116.png "App Service blade")

22. Click back to the **Overview** page, and then click the **URL** for the Web App.

    ![The Web App URL http://osticketsystem.azurewebsites.net displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image117.png "Web App URL")

23. Immediately the Web App will load you should see the Support Center website again.

    ![The Support Center website displays with two button options: Open a New Ticket, or Check Ticket Status.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image22.png "Support Center webpage")

24. Click the Sign in link.

    ![Screenshot of the Sign in link.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image23.png "Sign in link")

25. Locate **I'm an agent** and click the **sign in here** link.

    ![On the Sign in to Microsoft Cloud Workshop page, next to I\'m an agent, the link to sign in here is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image24.png "Sign in page")

26. At the OsTicket screen, enter the **username** and **password** and click **Log In**.

    a.  Username: ***demouser***

    b.  Password: ***demo\@pass123***

    ![The osTicket log in webpage displays.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image25.png "osTicket log in webpage")

27. Once logged into the OsTicket system click **My Tickets**.

    ![On the osTicket page, tickets tab, My Tickets (4) is selected.](images/Hands-onlabstep-by-step-Linuxliftandshiftimages/media/image26.png "osTicket page, tickets tab")

28. On the **My Tickets** screen, click through to one of the tickets. Once again, you see that the data from the IaaS installation of the OsTicket system is preserved which means that you have successfully lifted and shifted the application to Azure PaaS!

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Linux Lift & Shift Azure hands-on lab step-by-step, you will want to delete the Resource Groups. This will free up your subscription from future charges.

-   OPSLABRG

-   OsTicketMySQLVMRG

-   OsTicketOnPremRG

-   OsTicketPaaSRG

-   OsTicketVMSSRG

You should follow all steps provided *after* attending the Hands-on lab.

