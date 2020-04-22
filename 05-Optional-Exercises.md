# Guest Management Audit Policies
  * [Windows web servers that are not using secure communication protocols (requires IIS on nodes)](#windows-web-servers-that-are-not-using-secure-communication-protocols-requires-iis-on-nodes)
  * [Windows VMs on which the specified services are not installed and 'Running'](#windows-vms-on-which-the-specified-services-are-not-installed-and-running)
  * [Windows VMs that are not joined to the specified domain](#windows-vms-that-are-not-joined-to-the-specified-domain)
  * [Windows VMs in which the Administrators group contains any of the specified members](#audit-windows-vms-in-which-the-administrators-group-contains-any-of-the-specified-members)
  * [Windows VMs with a pending reboot](#windows-vms-with-a-pending-reboot)
  
# Additional Audit Policies

  * [Audit Windows VMs that contain certificates expiring within the specified number of days](#audit-windows-vms-that-contain-certificates-expiring-within-the-specified-number-of-days)
  * [Audit on password policy on the machine](#audit-vms-with-insecure-password-security-settings)
  * [Audit Windows VMs that do not have the specified applications installed](#audit-windows-vms-that-do-not-have-the-specified-applications-installed)
  * [Audit Windows VMs that have the specified applications installed](#audit-windows-vms-that-have-the-specified-applications-installed)
  * [Inherit a tag from the resource group if missing](#inherit-a-tag-from-the-resource-group-if-missing)
  
## Terms used in this exercise:
   * Intiative
   * Remediation tasks: 
   * Compliant
   * Non-compliant

## Windows web servers that are not using secure communication protocols (requires IIS on nodes)

In this task, you will create initialive assignment **Audit Windows web servers that are not using secure communication protocols**, which will audit the VM as **Compliant** and **Non-compliant**. This initiative have two policy definations one is to install the pre-requsite to the guest os and other is to audit the compliance status.
For this IIS server is required in the VMS, if IIS is installed and **https** is configured for IIS server then it will show **Compliant**, if only **http** is configured no https, in that case it will show **Non-compliant**.

You will apply the **Initiatives** at resource group and it will audit all the guest VMs within that resource group.

1. In your Azure portal, browse through the **Resource groups**. From the navigate panel, select **Resource groups**.

   ![](./images/lunchResourceGroup.png)

1. You will see one resource group like **Azure-ARC-170523**, where 170523 is unique id and it may be different for your lab
environment. Select **Azure-ARC-170523**.

   ![](./images/azure-arc-170523.png) 
   
1. Now, select the **Policies** then select **Compliance** and then select **Assign initiative** to assign the in-build initiative.
   
   ![](./images/gotointiative.png)
   
1. Leave default value for the **Scope**, **Exclusions** and for **Basics** click on the ellipses (…) to the right of **Initiative definition**.

   ![](./images/selectdefination.png)
   
1. In the Search window for available definitions, type **secure** and select the one called **Audit Windows web servers that are not using secure communication protocols**. Click the **Select** button below.

   ![](./images/selectsecurecommunicationprotocols.png)
   
1. Click **Next** at the bottom of the window and on **Parameteres** tab leave default value and move to the next tab by selecting the **Next** button.

   ![](./images/nexttoparameter.png)
    
1. On **Remediation** blade, read the description and then select the **checkbox** for **Create a remediation task**. This ensures that the policy will apply to existing resources after the initiative is assigned. If that box is not selected, then the initiative only applies to newly created resources.

1. Select the **Create a Managed Identity** check box and the click **Next** again.

   ![](./images/createremediationtask.png)
   
1. Then at the bottom of the **Assign initiative** window click on **Create**.

   ![](./images/createsecureinitiative.png)
   
1. Click on the **initiative** which you just created **Audit Windows web servers that are not using secure communication protocols**.

   ![](./images/selectintiativeforcreatingremediationtask.png)
   
1. Click **Create a Remediation Task** at the top right, it will start the Remediation if it is not started already, if it is already started then it can give error like one remediation task is already running, you can ignore that error and proceed.

   ![](./images/remediation.png)
   
1. Check mark the Re-evalute resource compliance before remediating, select the all locations and then click on **Remediate**.

   ![](./images/newremediation.png)
   
1. In the next window at the bottom you will see a blue circle beside **Evaluating**. When it is successful and completed, the circle will turn green and it will say **Complete**. NOTE: if you had many ARC servers, you could evaluate the all at once but changing the scope to select one of more locations or all within a resource group.

   ![](./images/evaluating.png)
    
1. Pre-connected-winvm is already configured with IIS web server and winvm is still don't have IIS web server. Here, it will show **Pre-connected-winvm** as **Non-compliant** and **winvm** as **Compliant**. You can go to each windows Arc servers from Azure portal and check the policy compliance.

1. Search for Azure Arc from search box and then select the Machines – Azure Arc.

   ![](./images/azure-arc-1778.png)
 
1. You will see, following four machines are in **Connected** state. Click on the **slect-pre-connected-winvm** to check the **Compliance state**.

   ![](./images/slect-pre-connected-winvm.png)
   
1. Now, click on the **Policies** and check the **Compliance** state for the server. It should be in **Non-compliant** State, because IIS is installed but https is not configured for this server.

   ![](./images/arc-1000.png)
   
1. To change the **Compliance state**, you can configure the **https** for IIS, run the remediation task again and then compliance state will be changed to **Compliant**.
   
1. Similarly, you can check the **Compliance** state for the Pre-connected-winvm server and it should be **Non-compliant**, becouse IIS server is installed in this. 


### Windows VMs on which the specified services are not installed and 'Running' 

This initiative deploys the policy requirements and audits Windows virtual machines on which the specified services are not installed and 'Running'.

1. You can Assign this intialtive very simillar way of previous one, in this you have to provide the windows **service name** in **parameter**, services like:
   
   * **SecurityHealthService:** Windows Security Service handles unified device protection and health information)
   * **WSearch:** Provides content indexing, property caching, and search results for files, e-mail, and other content.
   * **Dnscache:** The DNS Client service (dnscache) caches Domain Name System (DNS) names and registers the full computer name for this computer. If the service is stopped, DNS names will continue to be resolved. However, the results of DNS name queries will not be cached and the computer's name will not be registered. If the service is disabled, any services that explicitly depend on it will fail to start.
   * **WinRM:** Windows Remote Management (WinRM) service implements the WS-Management protocol for remote management. WS-Management is a standard web services protocol used for remote software and hardware management. The WinRM service listens on the network for WS-Management requests and processes them. The WinRM Service needs to be configured with a listener using winrm.cmd command line tool or through Group Policy in order for it to listen over the network. The WinRM service provides access to WMI data and enables event collection. Event collection and subscription to events require that the service is running. WinRM messages use HTTP and HTTPS as transports. The WinRM service does not depend on IIS but is preconfigured to share a port with IIS on the same machine.  The WinRM service reserves the /wsman URL prefix. To prevent conflicts with IIS, administrators should ensure that any websites hosted on IIS do not use the /wsman URL prefix.
   * **vmictimesync**: **Hyper-V Time Synchronization Service** Synchronizes the system time of this virtual machine with the system time of the physical computer.
   
1. Repeat the steps from 1-6, which you performed to assign previous intiative, in step 5, search for **Audit Windows VMs on which the specified services are not installed and 'Running'** initiative and click on select.

1. On **Parameters** blade provide **vmictimesync** to audit if **vmictimesync** service is not running. After entering the service name parameter click on the next button. Now, you can create the remediation task and everything using the same steps which you have used previously during assignment of **Windows web servers that are not using secure communication protocols (requires IIS on nodes)**

   ![](./images/arc-1002.png)
   
1. If the **vmictimesync** is running in the guest os, then it should show **Non-compliant** otherwise **Compliant**. 

### Windows VMs that are not joined to the specified domain

This initiative deploys the policy requirements and audits Windows virtual machines that are not joined to the specified domain. You need to define **domain name** in parameter when you **Assign initiative**.

1. Similar to previous intiative, you can provide the **FQDN (Fully Qualified Domain Name)** in parameter for auditing the VMs that are not joined to the specified domain. 

   ![](./images/arc-1003.png)

### Audit Windows VMs in which the Administrators group contains any of the specified members

This initiative deploys the policy requirements and audits Windows virtual machines in which the Administrators group contains any of the specified members. 
1. Define **Member/Members name** in parameter when you **Assign initiative**.

   ![](./images/arc-1004.png)

### Windows VMs with a pending reboot

This initiative deploys the policy requirements and audits Windows virtual machines with a pending reboot.

You can see the **Compliance state** for all the Initiatives assigned above by going to the Arc Server Manager. Search for **Azure Arc** from the Azure resources, services search box from the top of the portal and then click on Azure Arc. After this, select **Manage Servers**.

You can now go to individual Arc Machines and then click on policy to see the **Compliance state**.

   ![](./images/checkallinitiatives.png)

# Additional Audit Policies

Here is the few additional policies that you can perform.
  * Audit Windows VMs that contain certificates expiring within the specified number of days
  * Audit on password policy on the machine 
  * Audit Windows VMs that do not have the specified applications installed
  * Audit Windows VMs that have the specified applications installed
  * Inherit a tag from the resource group if missing
  
### Audit Windows VMs that contain certificates expiring within the specified number of days

This initiative deploys the policy requirements and audits Windows virtual machines that contain certificates expiring within the specified number of days.

### Audit VMs with insecure password security settings

This initiative deploys the policy requirements and audits virtual machines with insecure password security settings.

### Audit Windows VMs that do not have the specified applications installed

This initiative deploys the policy requirements and audits Windows virtual machines that do not have the specified applications installed. Here you need to define  application name in parameter for the initiative to audit if the application is not installed in the VMs. You can check for **Notepad++** becouse it is installed in pre-connected-winvm already.

### Audit Windows VMs that have the specified applications installed

This initiative deploys the policy requirements and audits Windows virtual machines that have the specified applications installed. It should give the opposite Compliace state in complare to previous one if you are checking for the same application **Notepad++**

### Inherit a tag from the resource group if missing

It is not an intiative, it is a single policy. So, you need to go in Assign Policy and search for this to apply on resource group.

Adds the specified tag with its value from the parent resource group when any resource missing this tag is created or updated. Existing resources can be remediated by triggering a remediation task. If the tag exists with a different value it will not be changed.




 


