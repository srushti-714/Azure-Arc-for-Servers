# Exercise 4: Azure Governance for Arc Connected Machine
In this exercise, you will perform Role assignment, Policy assignment, Tag the Hybrid compute machines and check Activity logs of resource group and servers.

Using custom roles you can manage the access to the Azure Arc servers and assign the access of Azure Arc servers to any server auditor, Onboard Arc servers, Monitor Admin to the person in your company.

You can assign the In-Build policies to monitor guest level oprations on Azure Arc servers. You can filter the servers and apply the policies based on Tags.

## Task 1: RBAC + Security
## Theory of least permission
Security best practices specify that a user should be given the lowest permission level needed to perform her or his job. As an example, in the previous section a service principal is used for bulk onboarding of servers. The service principal is given ‘Hybrid Server Onboarding’ permission which is limited to onboarding ARC servers. A new user named Auditor has been added and tasked with periodically checking policy compliance. This user only needs read access to a specific set of resource types.
## Custom role
1. Save the role definition as ServerAuditor.json to C:\LabFiles directory in ARCHOST VM. You can use Visual studio code or notepad to edit and save the file. Provide the Subscription Id and resourceGroupsName of your lab environment in following custom role definition. You can get the values on lab details page.

   ![](./images/azure-arc-1221.png)   

    ```
    {
      "Actions": [
         "Microsoft.Authorization/policyassignments/read",
         "Microsoft.Authorization/policydefinitions/read",
         "Microsoft.Authorization/policysetdefinitions/read",
         "Microsoft.Compute/virtualMachines/read",
         "Microsoft.HybridCompute/machines/read",
         "Microsoft.PolicyInsights/*/read"
         ],
      "AssignableScopes": [
         "/subscriptions/<Subscription Id>/resourceGroups/<resourceGroupsName>"
         ],
      "Description": "Can audit server compliance.",
      "Name": "Server Auditor" 
    }
    ```

2. Once you save the file, it will look like below.
	
   ![](./images/azure-arc-1222.png)

3. Run the PowerShell commands to create a role definition.
 
     ```
     #Import Creds
     CD C:\LabFiles
     $credsfilepath = ".\creds.txt"
     $creds = Get-Content $credsfilepath | Out-String | ConvertFrom-StringData
     $AppID = "$($creds.AppID)"
     $AppSecret = "$($creds.AppSecret)"
     $TenantID = "$($creds.TenantID)"
     $SubscriptionId = "$($creds.SubscriptionId)"
     $passwd = ConvertTo-SecureString $AppSecret -AsPlainText -Force
     $pscredential = New-Object System.Management.Automation.PSCredential($AppID, $passwd)
     Connect-AzAccount -ServicePrincipal -Credential $pscredential -Tenant $tenantId
     New-AzRoleDefinition -InputFile .\ServerAuditor.json 
    ```

   ![](./images/azure-arc-1225.png)

4. Now, go to your resource group in Azure portal and click on **Access control (IAM)** and then click on **+ Add** button to assign the role to self which you just created.

   ![](./images/azure-arc-1226.png)

5. Now, click on **Add role assignment**.

   ![](./images/azure-arc-1227.png)

6. Click on **Select a role** and search for **Server Auditor** and select it from the list.

   ![](./images/azure-arc-1228.png)

7. Now, in Select option search for your **azure account**, click on that and then select the **save** button to **assign the Server Auditor role** to self.

   ![](./images/azure-arc-1229.png)

8. Once the **role assignment** succeed you will see the **notification** on bell icon.

   ![](./images/azure-arc-1230.png)

9. You can review the role assigned to self by going to **Role assignments**.

   ![](./images/azure-arc-1551.png)

## Task 2: Apply Policy
Policies can be applied to ARC servers the same way they are applied to Microsoft Azure virtual machines. Policies can be applied to ensure the Azure resources are compliant with established practices such as ensuring that all resources are tagged with an owner. Initiatives can be applied to ensure the server operating systems are compliant such as ensuring the time zone is set correctly on a Microsoft Windows server or a software package is installed on a Linux server. The initiatives use a publish policy to deploy a configuration requirement and an audit policy to check if the requirement has been met.

1. From the **Machine -Azure Arc** menu for the Hybrid Server you are working with, click on **Policies**

   ![](./images/azure-arc-2221.png)

2. Click **Assign policy**.

   ![](./images/azure-arc-2222.png)

3. To the right of **Basics** click on the ellipses (…) to the right of **Initiative definition**.

   ![](./images/azure-arc-2223.png)

4. In the **Search** window for available definitions, type “Time ” and select the one called **Show Audit results from Windows VMs that are not set to the specified time zone**.  Click the blue **Select** button below.

   ![](./images/azure-arc-2224.png)

5. Click **Next** at the bottom of the window.

   ![](./images/azure-arc-2225.png)

6. From the **Time zone** drop down menu select **“(UTC) Coordinated Universal Time”**. Click **Next**
7. Read the description and then select the checkbox for **Create a remediation task**. This ensures that the policy will apply to existing resources after the policy is assigned.  If that box is not selected, then the policy only applies to newly created resources.
8. Select the **Create a Managed Identity** check box and the click **Next** again

   ![](./images/azure-arc-2228.png)

9. Then at the bottom of the **Assign Policy** window click on **Create**.

   ![](./images/azure-arc-2229.png)

10. Click on the new policy just created **Audit Windows VMs that are not set to the specified time zone**
11. Click **Create a Remediation Task** at the top right

    ![](./images/azure-arc-3331.png)

12. Confirm that the **Scope** is showing the correct Resource Group – should default to …/Azure-ARC-171289

    ![](./images/azure-arc-3333.png)

      a. Click on the **ellipses** …to the right to select all options to include the server as shown to the right

13. Select the Checkbox beneath **Re-evaluate resource compliance before remediating**
14. In the **Locations** drop-down list, select the location where you installed your ARC Server
      * If you are not sure or can’t remember, look on the **Overview** window for your server
15. Click **Remediate** at the bottom of the **New Remediation Task** window

    ![](./images/azure-arc-3335.png)

In the next window at the bottom you will see a blue circle beside **Evaluating**. When it is successful and completed, the circle will turn green and it will say **Complete**. NOTE: if you had many ARC servers, you could evaluate the all at once but changing the scope to select one of more locations or all within a resource group.

   ![](./images/azure-arc-3336.png)

Optional initiatives to try… repeat the steps above to test some other policies such as:

**Operational compliance**
  * Micro-segmentation to ensure servers are connected to the right network (remote host connection status doesn't match the specified one)
  * Certificate about to expire
  * Application installed: ensure backup is installed
  * Machines are not restarted after 45 days, indicator that it has been forgotten, running but not used

**Security compliance**
  * Password policy
  * Application installed or not installed (like diagnostic tools used for investigation, but make sure it is removed after troubleshooting)
  * TLS 1.2 monitoring => part of PCI DSS (data security standard)

16. You can check your server is **compliant** or **not** against **“[Preview]: Configure time zone on Windows machines”** policy you assigned in previous step. Click on the **policies** from winvm options from left hand pane and then look for **Compliance** state. You can see **winvm** is compliant against this policy.

    ![](./images/azure-arc-3337.png)

**Note**: If you find **winvm non-compliant** means time zone of winvm is different from the time zone you provided in policy. You can change the time zone of winvm using following script and after sometime you will see the winvm complaint state changed to **Compliant**

   ```
      $ap = "demo@pass123" 
      $cred = New-Object -ArgumentList "Administrator",(ConvertTo-SecureString -AsPlainText -Force -String $ap) -    TypeName System.Management.Automation.PSCredential 
      $ip = "192.168.0.5" 
      Invoke-Command -ComputerName $ip -Credential $cred -ScriptBlock {Set-TimeZone -Id '(UTC) Coordinated Universal Time'} 
   ```
    
## Task 3: Tag your ARC server
1. Open the Azure portal page. Click on this link to go to the Azure ARC machine(s) you have built
2. Click on your machine that you want to tag

   ![](./images/azure-arc-3338.png)

3. After clicking on it, Click on **Tags** in the center blade

   ![](./images/azure-arc-3339.png)

4. Create the following tags, for owner enter your Azure account unique id and then Save them
   
   ![](./images/tag.png)

5. After saving them they should look like this.&nbsp;&nbsp;

   ![](./images/azure-arc-4441.png)

## Task 4: Activity Logs
In last task, you added **tags** to one of the **Hybrid Computes machines**, check the **activity logs** for that.
1. In this step, click on the **Activity logs**, you will see **Write Azure Arc Machines** operation is performed. Expand the operation and click on the one of **expanded operation**.

   ![](./images/azure-arc-4442.png)

2. Now, click on the **Change history (Preview)**, you can see multiple Tags inserted. In same way you can check more **Tags**.

   ![](./images/azure-arc-4443.png)
