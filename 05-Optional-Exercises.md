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

You can enable/disable the following secure TLS protocol versions on web server (pre-connected-winvm) to experience the changes in **Compliance state**.
```
    SSL 2.0  
    SSL 3.0  
    TLS 1.0
    TLS 1.1
    TLS 1.2 
    PCT 1.0 
    Multi-Protocol Unified Hello 
```
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
   
1. Now, click on the **Policies** and check the **Compliance** state for the server. It will be in **Non-compliant** State. To check, why it is Non-compliant click on **Audit Windows web servers that are not using secure communication protocols**.

   ![](./images/arc-1000.png)
   
1. Now, click on **Show audit results from Windows web servers that are not using secure communication protocols**.
   
   ![](./images/arc-1005.png)
   
1. You will see **Compliance reason** coloumn for **pre-connected-winvm**, under that click on **Details**.

   ![](./images/arc-1006.png)
   
1. Click on the link shown below **Existence condition**. It will open a new tab in browser. 

   ![](./images/arc-1007.png)
      
1. Switch to the newly opened tab in your web browser, you can see the following Reason listed. Similarly, you can check the Non-compliant reason for rest of the policies in this exercise.

   **Reason**
   ```
      Could not find any secure TLS protocol version enabled on this web server. 
      Displaying current status of protocols: 
      SSL 2.0 - Absent 
      SSL 3.0 - Absent 
      TLS 1.0 - Absent 
      PCT 1.0 - Absent 
      Multi-Protocol Unified Hello - Absent 
      TLS 1.1 - Absent 
      TLS 1.2 - Absent 
   ```
   ![](./images/arc-1008.png)
   
1. To change the **Compliance state** to **Complaint**, you can enable the following secure TLS protocol version on web server (pre-connected-winvm).
    * SSL 2.0  
    * SSL 3.0  
    * TLS 1.0
    * TLS 1.1
    * TLS 1.2 
    * PCT 1.0 
    * Multi-Protocol Unified Hello 


## Windows VMs on which the specified services are not installed and 'Running' 

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
1. If it is **Non-Compliant**, then you can go to the details and check the reason of compliance state using the exactly same steps which you used in the previous Policy.
1. If it is in compliant state it won't show the reason. It means application is installed and running.

## Windows VMs that are not joined to the specified domain

This initiative deploys the policy requirements and audits Windows virtual machines that are not joined to the specified domain. You need to define **domain name** in parameter when you **Assign initiative**.

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **certificates**
   * You will see **Windows VMs that are not joined to the specified domain**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * Define **FQDN value** in **Parameter** for **Domain Name (FQDN)** and then click on **Next** from buttom.
     * Domain Name (FQDN) : ``` contoso.com ```
     
     ![](./images/arc-1013.png)
     
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
1. Now, you can go to the details and check the **Non-Compliant** reason. 
   
   Reason: The Domain Name the VM is currently joined to 'WORKGROUP' does not match the one specified by the User 'archost' ..
   
   ![](./images/arc-1014.png)   

## Audit Windows VMs in which the Administrators group contains any of the specified members

This initiative deploys the policy requirements and audits Windows virtual machines in which the Administrators group contains any of the specified members. 
1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **certificates**
   * You will see **Audit Windows VMs in which the Administrators group contains any of the specified members**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * Define **Members to exclude value** in **Parameter** and then click on **Next** from buttom.
     * Members to exclude : ``` Administrator ```
     
      ![](./images/arc-1004.png)
     
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
1. It should show the **pre-connected-winvm** as **compliant** becouse user/member **Administrator** is a member of **Administrators group**

   ![](./images/arc-1015.png)   

## Windows VMs with a pending reboot

This initiative deploys the policy requirements and audits Windows virtual machines with a pending reboot.

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **certificates**
   * You will see **Audit Windows VMs that contain certificates expiring within the specified number of days**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On **Parameters** tab, leave default and click on **Next**.     
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.

1. You can now go to individual Arc Machines (**pre-connected-winvm** and **winvm**) and then click on policy to see the **Compliance state**. If it is **Non-compliant** then go to the details and check the reason on Non-compliance.

2. Reason could be be anything like windows update, any application and service installation reboot is pending.
      
   ![](./images/arc-1016.png)   

# Additional Audit Policies

Here is the few additional policies that you can perform.
  * Audit Windows VMs that contain certificates expiring within the specified number of days
  * Audit on password policy on the machine 
  * Audit Windows VMs that do not have the specified applications installed
  * Audit Windows VMs that have the specified applications installed
  * Inherit a tag from the resource group if missing
  
## Audit Windows VMs that contain certificates expiring within the specified number of days

This initiative deploys the policy requirements and audits Windows virtual machines that contain certificates expiring within the specified number of days.

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **certificates**
   * You will see **Audit Windows VMs that contain certificates expiring within the specified number of days**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On **Parameters** tab, change **Include expired certificates** value from **false** to **true** from dropdown and then click on **Next** from buttom. 
   
     ![](./images/arc-1010.png)
     
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
1. You will see the audits are **Non-compliant**. When you will check the reason of **non-compliant**, you will find it is becouse of already expired certificates.

   ![](./images/arc-1011.png)
   
2. To change the **compliace state** to **Compliant**, edit the intiative and change **Include expired certificates** value to **false**. So it whould not consider the already expired certificates and state would change to **Compliant** in approx 30 minutes, you may have to create new remediation.

     ![](./images/arc-1012.png)

## Audit VMs with insecure password security settings

This initiative deploys the policy requirements and audits virtual machines with insecure password security settings.

You can Assign this initiative very similar to the very first initiative which you have assigned in this exercise. This initiative is combination of more then 3-4 policies.

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **password**
   * You will see **Audit VMs with insecure password security settings**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On **Parameters** tab, again click on **Next**.
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
1. If it is **Non-compliant** then go to the details and check the reason on Non-compliance. Lets check the reason for policy **Show audit results from Windows VMs that do not have a maximum password age of 70 days**. Click on this polcy name as shown in resource group.

   ![](./images/arc-1018.png)


## Audit Windows VMs that do not have the specified applications installed

This initiative deploys the policy requirements and audits Windows virtual machines that do not have the specified applications installed. Here you need to define  application name in parameter for the initiative to audit if the application is not installed in the VMs. You can check for **Notepad++** becouse it is installed in pre-connected-winvm already.

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **password**
   * You will see **Audit Windows VMs that do not have the specified applications installed**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On **Parameters** tab, provide the application name for which you want to audit; ie: ``` Notepad++ ```
   * Now, click on **Next**.
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
1. If application is installed in the system, then it shoud show compliant.
   * In **pre-connected-winvm Notepad++** is already installed, so it will show the pre-connected-winvm as compliant for this policy.
   * In **winvm Notepad++** is not installed so it will show the result as **Non-compliant**. Check the reason of **winvm**.
     
     ![](./images/arc-1017.png)

1. You can also install and uninstall to make the change in compliant state, it can take approx 30 minues after duing these chnages in server.

## Audit Windows VMs that have the specified applications installed

This initiative deploys the policy requirements and audits Windows virtual machines that have the specified applications installed. It should give the opposite Compliace state in complare to previous one if you are checking for the same application **Notepad++**

1. Go the the Resource group, then click on the **Policies**, then click on **Assign initiative**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Initiative definition**
   * Then from **Available Definitions** search box type **password**
   * You will see **Audit Windows VMs that have the specified applications installed**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On Parameters tab, provide the application name for which you want to audit; ie: ``` Notepad++ ```
   * Now, click on **Next**.
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
2. Compliance State of this would be exactly opposite to the [Audit Windows VMs that do not have the specified applications installed](#audit-windows-vms-that-do-not-have-the-specified-applications-installed)

## Inherit a tag from the resource group if missing

It is not an intiative, it is a single policy. So, you need to go in Assign Policy and search for this to apply on resource group.

Adds the specified tag with its value from the parent resource group when any resource missing this tag is created or updated. Existing resources can be remediated by triggering a remediation task. If the tag exists with a different value it will not be changed.

1. For this Policy first go to the **resource Group** and add a **Tag** with following key/value pair. So, when we apply this policy it will automatically apply this tag to all the resources under this resource group.
   * Tag Name: ``` Owner ```
   * Tag Value:  ``` Your Name ```

1. Go the the Resource group, then click on the **Policies**, then click on **Assign policy**. 
   * Leave the **Scope** and **Exclusions** default
   * Under basic, choose ellipse ... for selecting **Policy definition**
   * Then from **Available Definitions** search box type **Inherit a tag from the resource group if missing**
   * You will see **Inherit a tag from the resource group if missing**, click on that and then choose the Select button.
   * Now, from the buttom of the **Basics** page click on the **Next button**.
   * On **Parameters** tab, give the Tag name which you want to inherit to resources from resource group and then click on **Next**.
      * Tag Name: ``` Owner ```
      
       ![](./images/arc-1009.png)
     
   * On **Remediation** tab, click on the **Create a remediation task** to mark the checkbox.
   * Now, click on **Review + create** and then **Create**.
   
 2. After 5-10 Minutes check the resource Tags and you will see the tag **Owner** is replicated.
