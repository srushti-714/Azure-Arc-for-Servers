## Azure Arc for Servers: Hands-on Lab

Lab time: 2 Hours

Azure Arc for servers (preview) allows you to manage your Windows and Linux machines hosted outside of Azure on your corporate network or other cloud provider, similarly to how you manage native Azure virtual machines. When a hybrid machine is connected to Azure, it becomes a connected machine and is treated as a resource in Azure. Each connected machine has a Resource ID, is managed as part of a resource group inside a subscription, and benefits from standard Azure constructs such as Azure Policy and applying tags.

To deliver this experience with your hybrid machines hosted outside of Azure, the Azure Connected Machine agent needs to be installed on each machine that you plan on connecting to Azure. This agent does not deliver any other functionality, and it doesn't replace the Azure Log Analytics agent. The Log Analytics agent for Windows and Linux is required when you want to proactively monitor the OS and workloads running on the machine, manage it using Automation runbooks or solutions like Update Management, or use other Azure services like Azure Security Center.


# Exercise 1: Getting started with Azure Governance 

In this exercise, you will walk through some of the Azure Governance capabilities including Azure Activity Logs, Resource tags and policies. We’ll be trying out these capabilities with Azure resources and then extend to Azure Arc later during the lab.  
 
# Exercise 2: Getting started with Azure Arc
In the provided lab environment, you would already have one Windows and one Ubuntu Server running on-prem in a Hyper-V host pre-connected through Azure Arc. In this exercise, we’ll explore this pre-connected Azure Arc resources.

# Exercise 3: Connect On-Prem Servers to Azure with Arc
Azure Arc extends Azure Resource Manager capabilities to Linux and Windows servers, as well as Kubernetes clusters on any infrastructure across on-premises, multi-cloud, and edge. With Azure Arc, customers can also run Azure data services anywhere, realizing the benefits of cloud innovation, including always up-to-date data capabilities, deployment in seconds (rather than hours), and dynamic scalability on any infrastructure. Azure Arc for servers is currently in public preview.

In this exercise, you will browse through the on-prem servers which is hosted on Hyper-V and connect/onboard those servers to Azure Arc using the existing scripts loaded in the ARCHOST VM and later in next exercise.

# Exercise 4: Azure Governance for Azure Arc Connected Machine
In this exercise, you will perform Role assignment, Policy assignment, Tag the Hybrid compute machines and check Activity logs of resource group and servers.

Using custom roles you can manage the access to the Azure Arc servers and assign the access of Azure Arc servers to any server auditor, Onboard Arc servers, Monitor Admin to the person in your company.

You can assign the In-Build policies to monitor guest level oprations on Azure Arc servers. You can filter the servers and apply the policies based on Tags.
