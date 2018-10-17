
![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Linux lift and shift
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
September 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Linux lift and shift before the hands-on lab setup guide](#linux-lift-and-shift-before-the-hands-on-lab-setup-guide)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Create a virtual machine to execute the lab](#task-1-create-a-virtual-machine-to-execute-the-lab)
        - [Task 2: Install the MySQL Workbench](#task-2-install-the-mysql-workbench)
        - [Task 3: Download hands-on lab step-by-step attendee files](#task-3-download-hands-on-lab-step-by-step-attendee-files)

<!-- /TOC -->

# Linux lift and shift before the hands-on lab setup guide

## Requirements

- You must have a working Azure subscription to carry out this hands-on lab step-by-step.

## Before the hands-on lab

Duration: 30 Minutes

### Task 1: Create a virtual machine to execute the lab

1.  Launch a browser and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or just a Microsoft Account.

2.  Click on **+NEW**, and in the search box type in **Visual Studio Community 2017 on Windows Server 2016 (x64)** and press enter. Click the Visual Studio Community 2017 image running on Windows Server 2016 and with the latest update.

3.  In the returned search results, click the image name.

    ![In the Everything blade, the search field displays Visual Studio Community 2017 on Windows Server 2016 (x64). Under Results, Visual Studio Community 2017 on Windows Server 2016 (x64) is selected.](images/Setup/image4.png "Everything blade")

4.  In the Marketplace solution blade, at the bottom of the page keep the deployment model set to **Resource Manager**, and click **Create**.

    ![Resource Manager displays in the Select a deployment model field.](images/Setup/image5.png "Select a deployment model field")

5.  Set the following configuration on the Basics tab and click **OK**:

    -   Name: **LABVM**

    -   VM disk type: **SSD**

    -   User name: **demouser**

    -   Password: **demo\@pass123**

    -   Subscription: **If you have multiple subscriptions choose the subscription to execute your labs in.**

    -   Resource Group: **OPSLABRG**

    -   Location: **Choose the closest Azure region to you**.

    ![Fields in the Basics blade display with the previously defined settings.](images/Setup/image6.png "Basics blade")

6.  Choose the **DS1\_V2 Standard** or **DS2\_V2** instance size on the Size blade.

    > **Note**: You may have to click the **View All** link to see the instance sizes.

    ![In the Choose a size blade, the DS1\_V2 Standard option is selected.](images/Setup/image7.png "Choose a size blade")

    > **Note**: If the Azure Subscription you are using is [NOT]{.underline} a trial Azure subscription you may want to chose the DS2\_V2 to have more power in this LABMV. If you are using a trial subscription or one that you know has a restriction on the number of cores stick with the DS1\_V2.

7.  Click **Configure required settings** to specify a storage account for your virtual machine if a storage account name is not automatically selected for you.

    ![In the Settings blade, the Storage account option is selected.](images/Setup/image8.png "Settings blade")

8.  Click **Create New**.

    ![Screenshot of the Create new button.](images/Setup/image9.png "Create new button")

9.  Specify a unique name for the storage account (all lower letters and alphanumeric characters) and ensure the green checkmark shows the name is valid.

    ![Next to the Name field, the Green checkmark icon is selected.](images/Setup/image10.png "Green checkmark icon")

10. Click **OK** to continue.

    ![Screenshot of the OK button.](images/Setup/image11.png "OK button")

11. Click **Configure required settings** for the Diagnostics storage account if a storage account name is not automatically selected for you. Repeat the previous steps to select a unique storage account name. This storage account will hold diagnostic logs about your virtual machine that you can use for troubleshooting purposes.

    ![Screenshot of the Diagnostics Storage Account option.](images/Setup/image12.png "Diagnostics Storage Account option")

12. Accept the remaining default values on the Settings blade and click **OK**. On the Summary page click **OK**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    ![Screenshot of the Deploying Visual Studio icon.](images/Setup/image13.png "Deploying Visual Studio icon")

>**Note**: Please wait for the LABVM to be provisioned prior to moving to the next step.

13. Move back to the Portal page on your local machine and wait for **LABVM** to show the Status of **Running**. Click **Connect** to establish a new Remote Desktop Session.

    ![The Connect button is selected on the Portal page top menu.](images/Setup/image14.png "Portal page top menu")

14. Depending on your remote desktop protocol client and browser configuration you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

15. Log in with the credentials specified during creation:

    -   User: **demouser**

    -   Password: **demo\@pass123**

16. You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click **Yes** to continue with the connection.

    ![The Remote Desktop Connection dialog box displays.](images/Setup/image15.png "Remote Desktop Connection dialog box")

17. When logging on for the first time you will see a prompt on the right asking about network discovery. Click **No**.

    ![A Network Diagnostics prompt displays, asking if you want to find PCs, devices, and content on this network and automatically connect.](images/Setup/image16.png "Network Diagnostics prompt")

18. Notice that Server Manager opens by default. On the left, click **Local Server**.

    ![On the Server Manager menu, Local Server is selected.](images/Setup/image17.png "Server Manager menu")

19. On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![In the Essentials section, IE Enhanced Security Configuration is selected, and set to On.](images/Setup/image18.png "Essentials section")

20. Change to **Off** for Administrators and click **OK**.

    ![In the Internet Explorer Enhanced Security Configuration dialog box, Administrators is set to Off.](images/Setup/image19.png "Internet Explorer Enhanced Security Configuration dialog box")

### Task 2: Install the MySQL Workbench

1.  While logged into **LABVM** via remote desktop, open Internet Explorer and navigate to <https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-6.3.10-winx64.msi> this will download an executable. After the download is finished, click **Run** to execute it.

2.  Follow the directions of the installer to complete the installation of MySQL Workbench.

3.  After the installation is complete, **reboot** the machine.

### Task 3: Download hands-on lab step-by-step attendee files

1.  After the reboot has completed, download the zipped hands-on lab step-by-step attendee files by clicking on this link:  
https://cloudworkshop.blob.core.windows.net/linux-lift-shift/MCW-linux-lift-shift.zip.

2.  Extract the downloaded files into the directory **C:\\HOL**.

You should follow all steps provided *before* performing the Hands-on lab.
