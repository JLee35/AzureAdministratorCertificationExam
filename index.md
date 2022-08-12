# Azure Administrator Associate Exam Notes (AZ-104)

## Prerequisites

Azure Administrator Associate assumes that you have completed the AZ-900 Azure Fundamentals certification. Having not completed AZ-900 will not prevent you from testing for AZ-104, but you are expected to know the material from AZ-900. Here is the list of prerequisites and their associated learning paths on docs.microsoft.com:

- [Azure Fundamentals Part 1: Describe core Azure concepts](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-cloud-concepts/)
- [Azure Fundamentals Part 2: Describe core Azure services](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-core-azure-services/)
- [Azure Fundamentals Part 3: Describe core solutions and management tools on Azure](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-core-solutions-management-tools-azure/)
- [Azure Fundamentals Part 4: Describe general security and network security features](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-general-security-network-security-features/)
- [Azure Fundamentals Part 5: Describe identity, governance, privacy, and compliance features](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-identity-governance-privacy-compliance-features/)
- [Azure Fundamentals Part 6: Describe Microsoft Cost Management and service level agreements](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-azure-cost-management-service-level-agreements/)
- [Prerequisites for Azure administrators](https://docs.microsoft.com/en-us/learn/paths/az-104-administrator-prerequisites/)

<hr>
<hr>

## Prerequisites for Azure administrators

## Configure Azure resources with tools

Azure Administrators have many tools when it comes to managing resources. These tools include the Azure portal, Azure Cloud Shell, Azure PowerShell, and Azure CLI. 

Manage resources with the Azure portal by navigating to portal.azure.com. The Azure Cloud Shell can be accessed via the Azure portal. Azure PowerShell is accessible via PowerShell on Windows, and the Azure CLI can be installed on Windows, Linux, and Mac. 

<hr>

## Azure Resource Manager

## Objectives
- Identify the features and usage cases for Azure Resource Manager
- Describe each Azure Resource Manager component and its usage
- Organize your Azure resources with resource groups
- Apply Azure Resource Manager locks
- Move Azure resources between groups, subscriptions, and regions
- Remove resources and resource groups
- Apply and track resource limits

<hr>

## Review Azure Resource Manager benefits
Azure Resource Manager enables you to work with the resources in your solution as a group. You can deploy, update, or delete all the resources for your solution in a single, coordinated operation. You use a template for deployment and that template can work for different environments such as testing, staging, and production. Azure Resource Manager provides security, auditing, and tagging features to help you manage your resources after deployment.

## Consistent management layer
Azure Resource Manager provides a consistent management layer to perform tasks through Azure PowerShell, Azure CLI, Azure portal, REST API, and client SDKs. Choose the tools and APIs that work best for you.

The following image shows how all the tools interact with the same Azure Resource Manager API. The API passes requests to the Azure Resource Manager service, which authenticates and authorizes the requests. Azure Resource Manager then routes the requests to the appropriate resource providers.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/use-azure-resource-manager/media/resource-manager-016a1bac.png">

## Benefits
Azure Resource Manager provides several benefits:

- You can deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
- You can repeatedly deploy your solution through the development lifecycle and have confidence your resources are deployed in a consistent state.
- You can manage your infrastructure through declarative templates rather than scripts.
- You can define the dependencies between resources so they're deployed in the correct order.
- You can apply access control to all services in your resource group because Role-Based Access Control (RBAC) is natively integrated into the management platform.
- You can apply tags to resources to logically organize all the resources in your subscription.

## Guidance
The following suggestions help you take full advantage of Azure Resource Manager when working with your solutions.

- Define and deploy your infrastructure through the declarative syntax in Azure Resource Manager templates, rather than through imperative commands.
- Define all deployment and configuration steps in the template. You should have no manual steps for setting up your solution.
- Run imperative commands to manage your resources, such as to start or stop an app or machine.
- Arrange resources with the same lifecycle in a resource group. Use tags for all other organizing of resources.

<hr>

## Review Azure resource terminology
If you're new to Azure Resource Manager, there are some terms you might not be familiar with.

- **resource**: A manageable item that is available through Azure. Some common resources are a virtual machine, storage account, web app, database, and virtual network, but there are many more.
- **resource group**: A container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.
- **resource provider**: A service that supplies the resources you can deploy and manage through Resource Manager. Each resource provider offers operations for working with the resources that are deployed. Some common resource providers are Microsoft.Compute, which supplies the virtual machine resource, Microsoft.Storage, which supplies the storage account resource, and Microsoft.Web, which supplies resources related to web apps.
- **template**: A JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group. It also defines the dependencies between the deployed resources. The template can be used to deploy the resources consistently and repeatedly.
- **declarative syntax**: Syntax that lets you state "Here is what I intend to create" without having to write the sequence of programming commands to create it. The Resource Manager template is an example of declarative syntax. in the file, you define the properties for the infrastructure to deploy to Azure.

## Resource providers
Each resource provider offers a set of resources and operations for working with an Azure service. For example, if you want to store keys and secrets, you work with the **Microsoft.KeyVault** resource provider. This resource provider offers a resource type called vaults for creating the key vault.

The name of a resource type is in the format: **{resource-provider}/{resource-type}**. For example, the key vault type is **Microsoft.KeyVault/vaults**.

Note: Before deploying your resources, you should gain an understanding of the available resource providers. Knowing the names of resource providers and resources helps you define resources you want to deploy to Azure. Also, you need to know the valid locations and API versions for each resource type.

<hr>

## Create resource groups
Resources can be deployed to any new or existing resource group. Deployment of resources to a resource group becomes a job where you can track the template execution. If deployment fails, the output of the job can describe why the deployment failed. Whether the deployment is a single resource to a group or a template to a group, you can use the information to fix any errors and redeploy. Deployments are incremental; if a resource group contains two web apps and you decide to deploy a third, the existing web apps will not be removed.

## Considerations
Resource Groups are at their simplest a logical collection of resources. There are a couple of small rules for resource groups.

- Resources can only exist in one resource group.
- Resource Groups cannot be renamed.
- Resource Groups can have resources of many different types (services).
- Resource Groups can have resources from any different regions.

## Creating resource groups
There are some important factors to consider when defining your resource group:

- All the resources in your group should share the same lifecycle. You deploy, update, and delete them together. If one resource, such as a database server, needs to exist on a different deployment cycle it should be in another resource group.
- You can add or remove a resource to a resource group at any time.
- You can move a resource from one resource group to another group.
- A resource group can be used to scope access control for administrative actions.
- A resource can interact with resources in other resource groups. This interaction is common when the two resources are related but don't share the same lifecycle (for example, web apps connecting to a database).

When creating a resource group, you need to provide a location for that resource group. You may be wondering, "Why does a resource group need a location? And, if the resources can have different locations than the resource group, why does the resource group location matter at all?" The resource group stores metadata about the resources. Therefore, when you specify a location for the resource group, you're specifying where that metadata is stored. For compliance reasons, you may need to store your data in a particular region.

Note: By scoping permissions to a resource group, you can add/remove and modify resources easily without having to recreate assignments and scopes.

<hr>

## Create Azure Resource Manager locks
Resource Manager locks allow organizations to put a structure in place that prevents the accidental deletion of resources in Azure.

- You can associate the lock with a subscription, resource group, or resource.
- Locks are inherited by child resources.

## Lock types
There are two types of resource locks.
- **Read-Only lock**, which prevent any changes to the resource.
- **Delete locks**, which prevent deletion.

Note: Only the Owner and User Access Administrator roles can create or delete management locks.

<hr>

## Reorganize Azure Resources
When moving resources, both the source group and the target group are locked during the operation. Write and delete operations are blocked on the resource groups until the move completes. This lock means you can't add, update, or delete resources in the resource groups. Locks don't mean the resources aren't available. For example, if you move a VM to an new resource group, an application can still access the VM.

## Limitations
Before beginning this process be sure to read the [Move operation support for resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources) page. This page details what resources can be moved between resources group, subscriptions, and regions.

## Implementation
To move resources, select the resource group containing those resources, and then select the **Move** button. Select the resources to move and the destination resource group. Acknowledge that you need to update scripts.

Note: Just because a service can be moved doesn't mean there aren't restrictions. For example, you can move a virtual network, but you must also move its dependent resources, like gateways.

<hr>

## Remove resources and resource groups
Use caution when deleting a resource group. Deleting a resource group deletes all the resources contained within it. That resource group might contain resources that resources in other resource groups depend on.

## Using PowerShell to delete resource groups
To remove a resource group use **Remove--AzResourceGroup**. In this example, we are removing the ContosoRG01 resource group from the subscription. The cmdlet prompts you for confirmation and returns no output.

    Remove-AZResourceGroup -Name "ContosoRG01"

## Removing resources
You can also delete individual resources within a resource group. For example, here we are deleting a virtual network. Notice you can change the resource group on this page.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/use-azure-resource-manager/media/delete-resources-7ac5596a.png">

<hr>

## Determine resource limits
Azure lets you view resource usage against limits. This is helpful to track current usage, and plan for future use.

- The limits shown are the limits for your subscription
- When you need to increase a default limit, there is a Request Increase link.
- All resources have a maximum limit listed in Azure limits.
- If you are at the maximum limit, the limit can't be increased.

<hr>

## Knowledge check
1. A new project has several resources that need to be administered together. Which of the following strategies would provide a good solution? -> Azure resource groups
2. Which of the following situations would be a good example of when to use a resource lock? -> An ExpressRoute circuit with connectivity back to the on-premises network (pretty important)
3. Which of the following is true about resource groups? -> Resources can be in only one resource group

<hr>

## Summary
Azure Resource Manager is the deployment and management service for Azure. It provides a management layer that enables you to create, update, and delete resources in your Azure account. You use management features, like access control, locks, and tags, to secure and organize your resources after deployment.

<hr>

## Configure resources with Azure Resource Manager templates

## Objectives
- List the advantages of Azure templates.
- Identify the Azure template schema components.
- Specify Azure template parameters.
- Locate and use Azure Quickstart Templates.

<hr>

## Review Azure Resources Manager template advantages
An **Azure Resource Manager template** precisely defines all the Resource Manager resources in a deployment. You can deploy a Resource Manager template into a resource group as a single operation.

Using Resource Manager templates will make your deployments faster and more repeatable. For example, you no longer have to create a VM in the portal, wait for it to finish, and then create the next VM. Resource Manager template takes care of the entire deployment for you.

## Template benefits

- **Templates improve consistency**. Resource Manager templates provide a common language for you and others to describe your deployments. Regardless of the tool or SDK that you use to deploy the template, the structure, format, and expressions inside the template remain the same.
- **Templates help express complex deployments**. Templates enable you to deploy multiple resources in the correct order. For example, you wouldn't want to deploy a virtual machine prior to creating an operating system disk or network interface. Resource Manager maps out each resource and its dependent resources, and creates dependent resources first. Dependency mapping helps ensure that the deployment is carried out in the correct order.
- **Templates reduce manual, error-prone tasks**. Manually creating and connecting resources can be time consuming, and it's easy to make mistakes. Resource Manager ensure that the deployment happens the same way every time.
- **Templates are code**. Templates express your requirements through code. Think of a template as a type of Infrastructure as Code that can be shared, tested, and versioned similar to any other piece of software. Also, because templates are code, you can create a "paper trail" that you can follow. The template code documents the deployment. Most users maintain their templates under some kind of revision control, such as GIT. When you change the template, its revision history also documents how the template (and your deployment) has evolved over time.
- **Templates promote reuse**. Your template can contain parameters that are filled in when the template runs. A parameter can define a username or password, a domain name, and so on. Template parameters enable you to create multiple versions of your infrastructure, such as staging and production, while still using the exact same template.
- **Templates are linkable**. You can link Resource Manager templates together to make the templates themselves modular. You can write small templates that each define a piece of a solution, and then combine them to create a complete system.
- **Templates simplify orchestration**. You only need to deploy the template to deploy all of your resources. Normally this would take multiple operations.

<hr>

## Explore the Azure Resource Manager template schema
Azure Resource Manager templates are written in JSON, which allows you to express data stored as an object (such as a virtual machine) in text. A JSON document is essentially a collection of key-value pairs. Each key is a string, whose value can be:

- A string
- A number
- A Boolean expression
- A list of values
- An object (which is a collection of other key-value pairs)

A Resource Manager template can contain sections that are expressed using JSON notation, but are not related to the JSON language itself:

    {
        "$schema": "http://schema.management.​azure.com/schemas/2019-04-01/deploymentTemplate.json#",​
        "contentVersion": "",​
        "parameters": {},​
        "variables": {},​
        "functions": [],​
        "resources": [],​
        "outputs": {}​
    }

See Microsoft documentation about what each of these elements are: [https://docs.microsoft.com/en-us/learn/modules/configure-resources-arm-templates/3-explore-template-schema](https://docs.microsoft.com/en-us/learn/modules/configure-resources-arm-templates/3-explore-template-schema)

<hr>

## Explore the Azure Resource Manager template parameters
In the parameters section of the template, you specify which values you can input when deploying the resources. The available properties for a parameter are:

    "parameters": {
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
            "description": "<description-of-the parameter>"
            }
        }
    }

Here's an example that illustrates two parameters: one for a virtual machine's username, and one for its password:

    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
            "description": "Username for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
            "description": "Password for the Virtual Machine."
            }
        }
    }

Note: You're limited to 256 parameters in a template. You can reduce the number of parameters by using objects that contain multiple properties.

<hr>

## Consider Bicep templates
Azure Bicep is a domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. It provides concise syntax, reliable type safety, and support for code reuse.

You can use Bicep instead of JSON to develop your Azure Resource Manager templates (ARM templates). The JSON syntax to create an ARM template can be verbose and required complicated expressions. Bicep syntax reduces that complexity and improves the development experience. Bicep is a transparent abstraction over ARM template JSON and doesn't lose any of the JSON template capabilities.

## How does Bicep work?
When you deploy a resource or series of resources to Azure, you submit the Bicep template to Resource Manager, which still requires JSON templates. The tooling that's built into Bicep converts your Bicep template into a JSON template. This process is known as transpilation. Transpilation is the process of converting source code written in one language into another language.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-resources-arm-templates/media/bicep.png">

Bicep provides many improvements over JSON for template authoring, including:

- **Simpler syntax**: Bicep provides a simpler syntax for writing templates. You can reference parameters and variables directly, without using complicated functions. String interpolation is used in place of concatenation to combine values for names and other items. You can reference the properties of a resource directly by using its symbolic name instead of complex reference statements. These syntax improvements help both with authoring and reading Bicep templates.

- **Modules**: You can break down complex template deployments into smaller module files and reference them in a main template. These modules provide easier management and greater reusability.

- **Automatic dependency management**: In most situations, Bicep automatically detects dependencies between your resources. This process removes some of the work involved in template authoring.

- **Type validation and IntelliSense**: The Bicep extension for Visual Studio Code features rich validation and IntelliSense for all Azure resource type API definitions. This feature helps provide an easier authoring experience.

<hr>

## Review QuickStart templates
Azure QuickStart Templates are Azure Resource Manager templates provided by the Azure community.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-resources-arm-templates/media/quickstart-templates-2d65475a.png">

Some templates provide everything you need to deploy your solution, while others might serve as a starting point for your template. Either way, you can study these templates to learn how to best author and structure your own templates.

- The README.md file provides an overview of what the template does.
- The azuredeploy.json file defines the resources that will be deployed.
- The azuredeploy.parameters.json file provides the values the template needs.

<hr>

## Knowledge check
1. What is an Azure Resource Manager template? -> A JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for the deployment.
2. Which of the following is an element in the template schema? -> Outputs
3. What happens if the same template is run a second time? -> Azure Resource Manager won't make any changes to the deployed resources.

<hr>

## Summary
To implement infrastructure as code for your Azure solutions, use Azure Resource Manager templates. The template is a JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for your project. The template uses declarative syntax, which lets you state what you intend to deploy with having to write the sequence of programming commands to create it. In the template, you specify the resources to deploy and the properties for those resources.

<hr>

## Automate Azure tasks using scripts with PowerShell

## Objectives
- Decide if Azure PowerShell is the right tool for your Azure administration tasks
- Install Azure PowerShell on Linux, macOS, and/or Windows
- Connect to an Azure subscription using Azure PowerShell
- Create Azure resources using Azure PowerShell

<hr>

## Decide if Azure PowerShell is right for your tasks
Azure provides three administration tools:
- The Azure portal
- The Azure CLI
- Azure PowerShell

They all offer approximately the same amount of control; any task that you can do with one of the tools, you can likely do with the other two. All three are cross-platform, running on Windows, macOS, and Linux. They differ in syntax, setup requirements, automation support.

## What is the Azure portal?
The Azure portal is a website that lets you create, configure, and alter the resources in your Azure subscription. The portal is a Graphical User Interface (GUI) that makes it convenient to locate the resource you need and execute any required changes. It also guides you through complex administrative tasks by providing wizards and tooltips.

The portal does not provide any way to automate repetitive tasks. For example, to set up 15 VMs, you would need to create them one by one, completing the wizard for each VM. This can be time consuming, and is error prone for complex tasks.

## What is the Azure CLI?
The Azure CLI is a cross-platform command-line program to connect to Azure and execute administrative commands on Azure resources. For example, to create a VM, you would use a command like the following:

    az vm create \
    --resource-group CrmTestingResourceGroup \
    --name CrmUnitTests \
    --image UbuntuLTS
    ...

The Azure CLI is available two ways: inside a browser via the Azure Cloud Shell, or with a local install on Linux, Mac, or Windows. In both cases, you can use it interactively, or use it with scripts to automate tasks. For interactive use, you'd first launch a shell such as `cmd.exe` on Windows, or Bash on Linux or macOS, then issue the commands at the shell prompt. To automate repetitive tasks, you'd assemble the commands into a shell script using the script syntax of your chosen shell, then execute the script.

## What is Azure PowerShell?
Azure PowerShell is a module you add to PowerShell to let you connect to your Azure subscription and manage resources. Azure PowerShell requires PowerShell to function. PowerShell provides services like the shell window, command parsing, and so on. The Azure Az PowerShell module adds Azure-specific commands.

For example, Azure PowerShell provides the **New-AzCM** command, which creates a virtual machine for you in your Azure subscription. To use it, you would launch the PowerShell application, then issue a command like the following:

    New-AzVm `
        -ResourceGroupName "CrmTestingResourceGroup" `
        -Name "CrmUnitTests" `
        -Image "UbuntuLTS"
        ...

Azure PowerShell is also available two ways: inside a browser via the Azure Cloud Shell, or with a local install on Linux, Mac, or Windows. In both cases, you have two modes to choose from. You can use it in interactive mode, in which you manually issue one command at a time, or in scripting mode, where you execute a script that consists of multiple commands.

## How to Choose an administrative tool
There is approximate parity between the portal, the Azure CLI, and Azure PowerShell with respect to the Azure objects they can administer and the configurations they can create. They are also all cross-platform. This means you will typically consider several other factors when making your choice:

- **Automation**: Do you need to automate a set of complex or repetitive tasks? Azure PowerShell and the Azure CLI support his, while Azure portal does not.
- **Learning curve**: Do you need to complete a task quickly without learning new commands or syntax? The Azure portal does not require you to learn syntax or memorize commands. In Azure PowerShell and teh Azure CLI, you must know the detailed syntax for each command you use.
- **Team skillset**: Does your team have existing expertise? For example, your team may have used PowerShell to administer Windows. If so, they will quickly become comfortable using Azure PowerShell.

<hr>

## Install PowerShell
Suppose you've chosen Azure PowerShell as your automation solution. Your administrators prefer to run their scripts locally rather than in the Azure Cloud Shell. The team uses machines that run Linux, macOS, and Windows. You need to get Azure PowerShell working on all their devices.

## What must be installed?
There are two components that make up Azure PowerShell.

- **The base PowerShell product** comes in two variants: Windows PowerShell and PowerShell 7.x, which can be installed on Windows, macOS, and Linux.
- **The Azure Az PowerShell module** is an extra module that must be installed to add the Azure-specific commands to PowerShell.

## How to install PowerShell
On both Linux and macOS, you'll use a package manager to install PowerShell Core. The recommended package manager differs by OS and distribution.

Note: PowerShell is available in the Microsoft repository, so you'll first need to add that repository to your package manager.

On Linux, the package manager will change based on the Linux distribution you choose. On macOS, you'll use `Homebrew` to install PowerShell.

<hr>

## Create an Azure Resource using scripts in Azure PowerShell
PowerShell lets you write commands and execute them immediately. This is known as **interactive mode**.

When you enter a command into PowerShell, PowerShell matches the command to cmdlet, and PowerShell then performs the requested action. We'll look at some common commands you can use, then we'll look into installing the Azure support for PowerShell.

## What are PowerShell cmdlets?
A PowerShell command is called a **cmdlet**. A cmdlet is a command that manipulates a single feature.

The base PowerShell product ships with cmdlets that work with features such as sessions and background jobs. You can add modules to your PowerShell installation to get cmdlets that manipulate other features. For example, there are third-party modules to work with ftp, administer your operating system, access the file system, and so on.

Cmdlets follow a verb-noun naming convention; for example, `Get-Process`, `Format-Table`, and `Start-Service`. There is also a convention for verb choice: "get" to retrieve data, "set" to insert or update data, "format" to format data, "out" to direct output to a destination, and so on.

Cmdlet authors are encouraged to include a help file for each cmdlet. The `Get-Help` cmdlet displays the help file for any cmdlet. For example, to get help on the `Get-ChildItem` cmdlet, enter the following statement in a Windows PowerShell session:

    Get-Help -Name Get-ChildItem -Detailed

## What is a PowerShell module?
Cmdlets are shipped in modules. A PowerShell Module is a DLL that includes the code to process each available cmdlet. You'll load cmdlets into PowerShell by loading the module in which they're contained. You can get a list of loaded modules using the `Get-Module` command. This will output something like:

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
    Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
    Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
    Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
    Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...

## What is the Az PowerShell module?
**Az** is the formal name for the Azure PowerShell module, which contains cmdlets to work with Azure features. It contains hundreds of cmdlets that let you control nearly every aspect of every Azure resource. You can work with resource groups, storage, virtual machines, Azure Active Directory, machine learning, and so on. This module is an open-source component available on GitHub.

## Install the Az PowerShell module
The Az PowerShell module is available from a global repository called the PowerShell Gallery. You can install the module onto your local machine through the `Install-Module` cmdlet.

To install the latest Azure Az PowerShell module, run the following commands:

1. Open **PowerShell** and run the following command followed by `Enter`.
    
        Install-Module -Name Az -Scope CurrentUser -Repository PSGallery

## Update a PowerShell module
If you get a warning or error message indicating that a version of the Azure PowerShell module is already installed, you can update to the latest version by issuing the following command:

        Update-Module -Name Az

As with the `Install-Module` cmdlet, answer **Yes** or **Yes to All** when prompted to trust the module. You can also use the `Update-Module` command to reinstall a module if you're having trouble with it.

## Example: How to create a resource group with Azure PowerShell
Once you've installed the Azure module, you can begin working with Azure. Let's do a common task: creating a Resource Group. As you know, we use resource groups to administer related resources together. Creating a new resource group is one of the first tasks you'll do when starting a new Azure solution.

There are four steps you need to perform:

1. Import the Azure cmdlets.
2. Connect to your Azure subscription.
3. Create the resource group.
4. Verify that creation was successful.

Each step corresponds to a different cmdlet.

## Import the Azure cmdlets
Beginning with PowerShell 3.0, modules are loaded automatically when you use a cmdlet within the module. It's no longer necessary to manually import PowerShell modules unless you've changed the default module autoloading settings.

## Connect
When you're working with a local install of Azure PowerShell, you'll need to authenticate before you can execute Azure commands. The `Connect-AzAccount` cmdlet prompts for your Azure credentials, then connects to your Azure subscription. It has many optional parameters, but if all you need is an interactive prompt, you don't need any parameters:

        Connect-AzAccount

## Work with subscriptions
If you're new to Azure, you probably only have a single subscription. But if you've been using Azure for a while, you might have created multiple Azure subscriptions. You can configure Azure PowerShell to execute commands against a particular subscription.

You can only be in one subscription at a time. Use the `Get-AsContext` cmdlet to determine which subscription is active. If it's not the correct one, you can change subscriptions using another cmdlet.

1. Get a list of all subscription names in your account with the `Get-AzSubscription` command.
2. Change the subscription by passing the name of the one to select.

        Set-AzContext -Subscription '00000000-0000-0000-0000-000000000000'

If you need to look up the **Subscription ID**, open Azure and select **Subscriptions** on the home page.

## Get a list of all resource groups
You can retrieve a list of all Resource Groups in the active subscription.

        Get-AzResourceGroup

To get a more concise view, you can send the output from the `Get-AzResourceGroup` to the `Format-Table` cmdlet using a pipe '|'.

        Get-AzResourceGroup | Format-Table

The output will look something like this:

        ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
        -----------------                  --------       ----------------- ---- --------- ----------
        cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/00000000-0000-0000...
        ExerciseResources                  eastus         Succeeded                        /subscriptions/00000000-0000-0000...

## Create a resource group
A resource group is often one of the first things you'll create when starting a new application.

You can create resource groups by using the `New-AzResourceGroup` cmdlet. You must specify a name and location. The name must be unique within your subscription. The location determines where the metadata for your resource group will be stored (which may be important to you for compliance reasons). You use strings like "West US", "North Europe", or "West India" to specify the location. As with most of the Azure cmdlets, `New-AzResourceGroup` has many optional parameters. However, the core syntax is:

        New-AzResourceGroup -Name <name> -Location <location>

## Verify the resources
The `Get-AzResource` lists your Azure resources, which is useful here to verify the resources were created and the resource group creation was successful.

        Get-AzResource

Like the `Get-AzResourceGroup` command, you can get a more concise view through the `Format-Table` cmdlet;

        Get-AzResource | Format-Table

You can also filter it to specific resource groups to only list resources associated with that group:

        Get-AzResource -ResourceGroupName ExerciseResources

## Create an Azure Virtual Machine
Another common task you can do with PowerShell is to create VMs.

Azure PowerShell provides the `New-AzVm` cmdlet to create a virtual machine. The cmdlet has many parameters to let it handle the large number of VM configuration settings. Most of the parameters have reasonable default values, so we only need to specify five things:

- **ResourceGroupName**: The resource group into which the new VM will be placed.
- **Name**: The name of the VM in Azure.
- **Location**: Geographic location where the VM will be provisioned.
- **Credential**: An object containing the username and password for the VM admin account. We'll use the `Get-Credential` cmdlet. This cmdlet will prompt for a username and password and package it into a credential object.
- **Image**: The operating system image to use for the VM, which is typically a Linux distribution or Windows Server.

        New-AzVm
            -ResourceGroupName <resource group name>
            -Name <machine name>
            -Credential <credentials object>
            -Location <location>
            -Image <image name>

You can supply these parameters directly to the cmdlet as shown above. Alternatively, you can use other cmdlets to configure the virtual machine, such as `Set-AzVMOperatingSystem`, `Set-AzVMSourceImage`, `Add-AzVMNetworkInterace`, and `Set-AzVMOSDisk`.

Here's an example that strings the `Get-Credential` cmdlet together with the `-Credential` parameter:

        New-AzVM -Name MyVM -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...

The `AzVM` suffix is specific to VM-based commands in PowerShell. There are several other you can use:

- `Remove-AzVM`: Deletes an Azure VM.
- `Start-AzVM`: Start a stopped VM.
- `Stop-AzVM`: Stop a running VM.
- `Restart-AzVM`: Restart a VM.
- `Update-AzVM`: Updates the configuration for a VM.

## Example: Getting information for a VM
You can list the VMs in your subscription using the `Get-AzVM -Status` command. This command also supports entering a specific VM by including the -Name property. Here, we'll assign it to a PowerShell variable:

        $vm = Get-AzVM -Name MyVM -ResourceGroupName ExerciseResources

The interesting thing is that now your VM is an object with which you can interact. For example, you can makes changes to that object, then push changes back to Azure by using the `Update-AzVM` command:

        $ResourceGroupName = "ExerciseResources"
        $vm = Get-AzVM -Name MyVM -ResourceGroupName $ResourceGroupName
        $vm.HardwareProfile.vmSize = "Standard_DS3_v2"

        Update-AzVM -ResourceGroupName $ResourceGroupName -VM $vm

<hr>

## Create and save scripts in Azure PowerShell
In this section, you will see how to write and execute an Azure PowerShell script that meets these requirements.

## What is a PowerShell script?
A PowerShell script is a text file containing commands and control constructs. The commands are invocations of cmdlets. The control constructs are programming features like loops, variables, parameters, comments, etc., supplied by PowerShell. PowerShell script files have a **.ps1** file extension.

## PowerShell techniques
PowerShell has many features found in typical programming languages. You can define variables, use branches and loops, capture command-line parameters, write functions, add comments, and so on. We will need three features for our script: variables, loops, and parameters.

## Variables
As you saw in the last unit, PowerShell supports variables. Use $ to declare a variable and = to assign a value. For example:

        $loc = "East US"
        $iterations = 3

Variables can hold objects. For example, the following definition sets the **adminCredential** variable to the object returned by the **Get-Credential** cmdlet.

        $adminCredential = Get-Credential

To obtain the value stored in a variable, use the $ prefix and its name, as in the following:

        $loc = "East US"
        New-AzResourceGroup -Name "MyResourceGroup" -Location $loc

## Loops
PowerShell has several loops: **For, Do...While, For...Each**, and so on.

The core syntax is shown below; the example runs for two iterations and prints the value of **i** each time. The comparison operators are written **-lt** for "less than", **-le** "less than or equal", **-eq** for equal, **-ne** for "not equal", etc.

        For ($i = 1; $i -lt 3; $i++) 
        {
            $i
        }

## Parameters
When you execute a script, you can pass arguments on the command line. You can provide names for each parameter to help the script extract the values. For example:

        .\setupEnvironment.ps1 -size 5 -location "East US"

Inside the script, you'll capture the values into variables. In this example, the parameters are matched by name:

        param([string]$location, [int]$size)

You can omit the names from the command line. For example:

        .\setupEnvironment.ps1 5 "East US"

Inside the script, you'll rely on position for matching when the parameters are unnamed:

        param([int]$size, [string]$location)

We could take these parameters as input and use a loop to create a set of VMs from the given parameters.

<hr>

## Knowledge check
1. True or false: The Azure portal, the Azure CLI, and Azure PowerShell offer significantly different services, so it is unlikely that all three will support the operation you need. -> False
2. Suppose you are building a video-editing application that will offer online storage for user-generated video content. You will store the videos in Azure Blobs, so you need to create an Azure storage account to contain the blobs. Once the storage account is in place, it is unlikely you would remove and recreate it because this would delete all the user videos. Which tool is likely to offer the quickest and easiest way to create the storage account? -> Azure portal
3. What needs to be installed on your machine to let you execute Azure PowerShell cmdlets locally? -> The base PowerShell product and the Az module.

<hr>

## Control Azure services with the CLI
Install the Azure CLI locally and use it to manage Azure resources.

## Objectives
- Install the Azure CLI on Linux, macOS, and/or Windows
- Connect to an Azure subscription using the Azure CLI
- Create Azure resources using the Azure CLI

<hr>

## What is the Azure CLI?
The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources. It runs on Linux, macOS, and Windows, and allows administrators and developers to execute their commands through a terminal or command-line prompt (or script) instead of a web browser.

The Azure CLI provides cross-platform command-line tools for managing Azure resources, and you can easily install it locally on Linux, Mac, or Windows computers. You can also use the Azure CLI from a browser through the Azure Cloud Shell. In both cases, you can use it interactively or scripted. For interactive use, you'll first launch a shell (such as cmd.exe on Windows or Bash on Linux or macOS) and then issue the command at the shell prompt. To automate repetitive tasks, you'll assemble the CLI commands into a shell script using the script syntax of your chosen shell, then execute the script.

## Using the Azure CLI in scripts
The Azure CLI must be installed before you can use it to manage Azure resources from a local computer.

<hr>

## What Azure resources can be managed using the Azure CLI?
The Azure CLI lets you control nearly every aspect of every Azure resource. You can work with resource groups, storage, virtual machines, Azure Active Directory, containers, machine learning, and so on.

Commands in the CLI are structured in groups and subgroups. Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings. For example, the `storage` group contains subgroups including **account, blob**, and **queue**.

So, how do you find the particular commands you need? Once way is to use `az find`, the AI robot that uses the Azure documentation to tell you more about commands, the CLI and more.

**Example**: find the most popular commands related to the word **blob**.

        az find blob

**Example**: find the most popular command for an Azure CLI command group, such as `az vm`.

        az find "az vm"

**Example**: Find the most popular parameters and subcommands for an Azure CLI command.

        az find "az vm create"

If you already know the name of the command you want, the `--help` argument for that command will get you more detailed information on the command, and for a command group, a list of the available subcommands. So, with our storage example, here's how you can get a list of the subgroups and command for managing blob storage:

        az storage blob --help

## How to create an Azure resource
When you're creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful. 

## Connect
Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.

        az login

The Azure CLI will typically launch your default browser to open the Azure sign-in page. After a successful sign-in, you'll be connected to your Azure subscription.

## Create
You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.

The Azure CLI **group create** command creates a resource group. You must specify a name and location. The name must be unique within your subscription. The location determines where the metadata for your resource group will be stored. You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia. The core syntax is:

        az group create --name <name> --location <location>

## Verify
For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details. For example, the Azure CLI **group list** command lists your Azure resource groups. This is useful to verify whether the resource group was successfully created:

        az group list

To get a more concise view, you can format the output as a simple table:

        az group list --output table

<hr>

## Summary
The Azure CLI is a good choice for anyone new to Azure command line and scripting. Its simple syntax and cross-platform compatibility help reduce the risk of errors when performing regular and repetitive tasks.

## Knowledge check
1. What do you need to install on your machine to let you execute Azure CLI commands locally? -> Only the Azure CLI
2. True or false: The Azure CLI can be installed on Linux, macOS, and Windows, and the CLI commands you use are the same in all platforms. -> True
3. Which parameter value can you add to most CLI commands to get concise, formatted output? -> table

<hr>

## Deploy Azure infrastructure by using JSON ARM templates
Write JSON Azure Resource Manager templates (ARM templates) by using Visual Studio Code to deploy your infrastructure to Azure consistently and reliably.

## Objectives
- Implement a JSON ARM template by using Visual Studio Code
- Declare resources and add flexibility to your template by adding resources, parameters, and outputs.

<hr>

## What is infrastructure as code?
With Infrastructure as code, you can maintain both your application code and everything you need to deploy your application in a central code repository. The advantages to infrastructure as code are:
- Consistent configurations
- Improved scalability
- Faster deployments
- Better traceability

## What is an ARM template?
ARM templates are JavaScript Object Notation (JSON) files that define the infrastructure and configuration for your deployment. The template uses a *declarative syntax*. The declarative syntax is a way of building the structure and elements that outline what resources will look like without describing its control flow. Declarative syntax is different than *imperative syntax*, which uses commands for the computer to perform. Imperative scripting focuses on specifying each step in deploying the resources.

ARM templates allow you to declare what you intend to deploy without having to write the sequence of programming commands to create it. In an ARM template, you specify the resources and the properties for those resources. Then Azure Resource Manager uses that information to deploy the resources in an organized and consistent manner.

## Benefits of using ARM templates
ARM templates allow you to automate deployments and use the practice of infrastructure as code (IaC). The template code becomes part of your infrastructure and development projects. Just like application code, you can store the IaC files in a source repository and version it.

ARM templates are *idempotent*, which means you can deploy the same template many times and get the same resource types in the same state.

Resource Manager orchestrates the deployment of the resources so they're created in the correct order. When possible, resources will also be created in parallel, so ARM template deployments finish faster than scripted deployments.

Resource Manager also has built-in validation. It checks the template before starting the deployment to make sure the deployment will succeed.

If your deployments become more complex, you can break your ARM templates into smaller, reusable components. You can link these smaller templates together at deployment time. You can also nest templates inside other templates.

In the Azure portal, you can review your deployment history and get information about the state of the deployment. The portal displays values for all parameters and outputs.

You can also integrate your ARM templates into continuous integration and continuous deployment (CI/CD) tools like Azure Pipelines, which can automate your release pipelines for fast and reliable application and infrastructure updates. By using Azure DevOps and ARM template tasks, you can continuously build and deploy your projects.

## ARM template file structure
When writing an ARM template, you need to understand all the parts that make up the template and what they do. ARM template files are made up of the following elements:

- **schema**: A required section that defines the location of the JSON schema file that describes the structure of JSON data. The version number you use depends on the scope of the deployment and your JSON editor.
- **contentVersion**: A required section that defines the version of your template (such as 1.0.0.0). You can use this value to document significant changes in your template to ensure your're deploying the right template.
- **apiProfile**: An optional section that defines the collection of API versions for resource types. You can use this value to avoid having to specify API versions for each resource in the template.
- **parameters**: An optional section where you define values that are provided during deployment. These values can be provided by a parameter file, by command-line parameters, or in the Azure portal.
- **variables**: An optional section where you define values that are used to simplify template language expressions.
- **functions**: An option section where you can define user-defined functions that are available within the template. User-defined functions can simplify your template when complicated expressions are used repeatedly in your template.
- **resources**: A required section that defines the actual items you want to deploy or update in a resource group or a subscription.
- **output**: An optional section where you specify the values that will be returned at the end of the deployment.

## Deploy an ARM template to Azure
You can deploy an ARM template to Azure in one of the following ways:
- Deploy a local template.
- Deploy a linked template.
- Deploy in a continuous deployment pipeline.

First, sign in to Azure by using the Azure CLI or Azure PowerShell.

        Connect-AzAccount

Next, define your resource group. You can use an already-defined resource group or create a new one with the following command. You can obtain available location values from `Get-AzLocation`.

        New-AzResourceGroup -Name {name of your resource} -Location "{location}"

To start a template deployment at the resource group, use either the Azure CLI command `az deployment group create` or the Azure PowerShell command `New-AzResourceGroupDeployment`.

Both commands require the resource group, the region, and the name for the deployment so you can easily identify it in the deployment history.

## Add resources to the template
To add a resource to your template, you'll need to know the resource provider and its types of resources. The syntax for this combination is in the form of *{resource-provider}/{resource-type}*. For example, to add a storage account resource to your template, you'll need the *Microsoft.Storage* resource provider. One of the types for this provider is *storageAccount*. So your resource type will be displayed as *Microsoft.Storage/storageAccounts*. You can use a list of resource providers for Azure services to find the providers you need.

After you've defined the provider and resource type, you need to understand the properties for each resource type you want to use. For details, see Define resources in Azure Resource Manager templates. View the list in the left column to find the resource. Notice that the properties are sorted by API version.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/create-azure-resource-manager-template-vs-code/media/2-resource-type-properties.png">

Here's an example of some of the listed properties from the Storage Accounts page:

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/create-azure-resource-manager-template-vs-code/media/2-storage-account-properties.png">

For our storage example, your template might look like this:

        {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.1",
        "apiProfile": "",
        "parameters": {},
        "variables": {},
        "functions": [],
        "resources": [
            {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "learntemplatestorage123",
            "location": "westus",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": true
            }
            }
        ],
        "outputs": {}
        }

<hr>

## Add flexibility to your Azure Resource Manager template by using parameters and outputs
In this unit, you learn about the *parameters* and *outputs* sections of the template.

## ARM template parameters
ARM template parameters enable you to customize the deployment by providing values that are tailored for a particular environment. For example, you pass in different values based on whether you're deploying to an environment for development, test, production, or others. For example, the previous template uses the *Standard_LRS* storage account SKU. You can reuse this template for other deployments that create a storage account by making the name of the storage account SKU a parameter. Then, you pass in the name of the SKU you want for this particular deployment when the template is deployed. You can do this step either at the command line or by using a parameter file.

In the `parameters` section of the template, you specify which values you can input when you deploy the resources. You're limited to 256 parameters in a template. Parameter definitions can use most template functions.

The available properties for a parameter are:

        "parameters": {
            "<parameter-name>": {
                "type": "<type-of-parameter-value>",
                "defaultValue": "<default-value-of-parameter>",
                "allowedValues": [
                "<array-of-allowed-values>"
                ],
                "minValue": <minimum-value-for-int>,
                "maxValue": <maximum-value-for-int>,
                "minLength": <minimum-length-for-string-or-array>,
                "maxLength": <maximum-length-for-string-or-array-parameters>,
                "metadata": {
                "description": "<description-of-the-parameter>"
                }
            }
        }

The allowed types of parameters are:
- string
- secureString
- integers
- boolean
- object
- secureObject
- array

## Recommendations for using parameters
Use parameters for settings that vary according to the environment, for example, SKU, size, or capacity. Also use parameters for resource names that you want to specify yourself for easy identification or to comply with internal naming conventions. Provide a description for each parameter, and use default values whenever possible.

For security reasons, never hard code or provide default values for usernames and/or passwords in templates. Always use parameters for usernames and passwords (or secrets). Use *secureString* for all passwords and secrets. If you pass sensitive data in a JSON object, use the secureObject type. Template parameters with *secureString* or *secureObject* types can't be read or harvested after the deployment of the resource.

## Use parameters in an ARM template
In the parameters section of the ARM template, specify the parameters that can be input when you deploy the resources. You're limited to 256 parameters in a template.

Here's an example of a template file with a parameter for the storage account SKU defined in the parameters section of the template. You can provide a default for the parameter to be used if no value is specified at execution.

    "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS",
            "Premium_LRS"
            ],
            "metadata": {
            "description": "Storage Account type"
            }
        }
    }

Then, use the parameter in the resource definition. The syntax is `[parameters('name of the parameter')]`. You use the `parameters` function.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "learntemplatestorage123",
            "location": "[resourceGroup().location]",
            "sku": {
            "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {
            "supportsHttpsTrafficOnly": true
            }
        }
    ]

When you deploy the template, you can give a value for the parameter. Notice the last line of the following command:

        $templateFile="azuredeploy.json"
        New-AzResourceGroupDeployment `
            -Name testdeployment1 `
            -TemplateFile $templateFile `
            -storageAccountType Standard_LRS

## Arm template outputs
In the outputs section of your ARM template, you can specify values that will be returned after a successful deployment. Here are the elements that make up the outputs section.

        "outputs": {
            "<output-name>": {
                "condition": "<boolean-value-whether-to-output-value>",
                "type": "<type-of-output-value>",
                "value": "<output-value-expression>",
                "copy": {
                "count": <number-of-iterations>,
                "input": <values-for-the-variable>
                }
            }
        }
    
- **output-name**: Must be a valid JavaScript identifier.
- **condition**: (Optional) A Boolean value that indicates whether this output value is returned. When true, the value is included in the output for the deployment. When false, the output value is skipped for this deployment. When not specified, the default value is true.
- **type**: The type of the output value.
- **value**: (Optional) A template language expression that's evaluated and returned as an output value.
- **copy**: (Optional) Copy is used to return more than one value for an output.

## Use outputs in an ARM template
Here's an example to output the storage account's endpoints.

    "outputs": {
        "storageEndpoint": {
            "type": "object",
            "value": "[reference('learntemplatestorage123').primaryEndpoints]"
        }
    }

Notice the `reference` part of the expression. This function gets the runtime state of the storage account.

## Deploy an ARM template again
Recall that ARM templates are idempotent, which means you can deploy the template to the same environment again and if nothing was changed in the template, nothing will change in the environment. If a change was made to the template, for example, you changed a parameter value, only that change will be deployed. Your template can contain all of the resources you need for your Azure solution, and you can safely execute a template again. Resources will be created only if they didn't already exist and updated only if there's a change.

<hr>

## Knowledge check
1. What is an Azure Resource Manager template? -> A JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for your deployment.
2. What is not an element of an Azure Resource Manager template? -> idempotent.
3. Azure Resource Manager templates are idempotent. This means that if you run a template with no changes a second time: Azure Resource Manager won't make any changes to the deployed resources.

<hr>

## Manage identities and governance in Azure

## Configure user and group accounts

## Objectives
- Configure users accounts and user account properties
- Create new user accounts
- Import bulk user accounts with a template
- Configure group accounts and assignment types

<hr>

## Create user accounts
Azure AD defines users in three ways:
- **Cloud identities**: These users exist only in Azure AD. Examples are administrator accounts and users that you manage yourself. Cloud identities can be in Azure Active Directory or an external Azure Active Directory, if the user is defined in another Azure AD instance. When these accounts are removed from the primary directory, they are deleted.
- **Directory-synchronized identities**: These users exist in an on-premises Active Directory. A synchronization activity that occurs via Azure AD Connect brings these users in to Azure. Their source is Windows Server AD. 
- **Guest users**: These users exist outside Azure. Examples are accounts from other cloud providers and Microsoft accounts such as an Xbox Live account. Their source is Invited user. This type of account is useful when external vendors or contractors need access to your Azure resources. Once their help is no longer necessary, you can remove the account and all of their access.

<hr>

## Manage user accounts
There are multiple ways to add cloud identities to Azure AD.

## Azure portal
You can add new users through the Azure portal. In addition to Name and User name, there is profile information like Job Title and Department.

Things to consider when managing users:
- User profile (picture, job, contact info) is optional.
- Deleted users can be restored for 30 days.
- Sign in and audit og information is available.

Note: Users can also be added to Azure AD through Microsoft 365 Admin Center, Microsoft Intune admin console, and the CLI.

<hr>

## Create bulk user accounts
Azure Active Directory (Azure AD) supports bulk user create and delete operations and supports downloading lists of users. Just fill out the comma-separated values (CSV) template. You can download the template from the Azure AD portal. To create users in the Azure portal, you must be signed in as a Global administrator or User administrator.

## Things to consider when using the template
- **Naming conventions**: Establish or implement a naming convention for usernames, display names, and aliases. For example, a user name could consist of last name, period, first name: 'Smith.John@contoso.com'.
- **Passwords**: Implement a convention for the initial password of the newly created user. Figure out a way for the new users to receive their password in a secure way. Methods commonly used include generating a random password and emailing it to the new user or their manager.

Note: PowerShell is also available for bulk user uploads.

<hr>

## Create group accounts
Azure AD allows you to define two different types of groups.
- **Security groups**: Security groups are used to manage member and computer access to shared resources for a group or users. For example, you can create a security group for a specific security policy. By doing it this way, you can give a set of permissions to all the members at once, instead of having to add permissions to each member individually. This option requires an Azure AD administrator.
- **Microsoft 365 groups**: Microsoft 365 groups provide collaboration opportunities by giving members access to a shared mailbox, calendar, files, SharePoint site, and more. You can give people outside of your organization access to the group. Bot users and admins can use Microsoft 365 groups.

## Adding members to groups
There are different ways you can assign access rights:

- **Assigned**: Lets you add specific users to be members of this group and to have unique permissions.
- **Dynamic User**: Lets you add dynamic membership rules to automatically add and remove members. When a member's attributes change, Azure reviews the dynamic group rules for that directory. If the member meets the rule requirements, they're added. If the member no longer meets the rules requirements, they're removed.
- **Dynamic Device (Security groups only)**: Lets you use dynamic group rules to automatically add and remove devices. If a device's attributes change, Azure reviews the dynamic group rules for that directory. If the device meets the rule requirements, they're added. If the device no longer meets the rules requirements, they're removed.

<hr>

## Create administrative units
It can be useful to restrict administrative scope by using administrative units in organizations that are made up of independent divisions of any kind.

## Example
Consider the example of a large university that's made up of many autonomous schools (School of Business, School of Engineering, and so on). Each school has a team of IT admins who control access, manage users, and set polices for their school.

A central administrator could:
- Create a role with administrative permissions over only Azure AD users in the business school administrative unit.
- Create an administrative unit for the School of Business.
- Populate the administrative unit with only the business school students and staff.
- Add the business school IT team to the role, along with its scope.

## Considerations
- You can manage administrative units by using the Azure portal, PowerShell cmdlets and scripts, or Microsoft Graph.
- In the portal, you can manage administrative units if you are a Global Administrator or a Privileged Role Administrator.
- Administrative units apply scope only to management permissions. They don't prevent members or administrators from using their default user permissions to browse other users, groups, or resources outside the administrative unit.

<hr>

## Knowledge check
1. Which of the following roles has full access to manage all resources but cannot assign roles? -> Contributor
2. What type of user account would be created to allow an external organization easy access? -> A guest user account for each member of the external team.
3. Which Azure Active Directory role will allow a use to manage all the groups in your Teams tenants and be able to assign other administrator roles? -> Global administrator

<hr>

## Configure subscriptions
You'll learn how to configure subscriptions. This includes getting a subscription, implementing cost management, and applying resource tags.

## Objectives
- Determine the correct region to locate Azure services.
- Identify features and usage cases for Azure subscriptions.
- Identify how to obtain an Azure subscription.
- Understand billing and features for different Azure subscriptions.
- Use the Cost Management product for cost analysis.
- Determine when to use resource tagging.
- Identify ways to reduce costs.

<hr>

## Identify regions
Microsoft Azure is made up of datacenters located around the globe. These datacenters are organized and made available to end users by region. A region is a geographical area on the planet containing at least one, but potentially multiple datacenters. The datacenters are in close proximity and networked together with a low-latency network.

A few examples of regions are *West US*, *Canada Central*, *West Europe*, *Australia East*, and *Japan West*. Azure is generally available in 60+ regions and available in 140 countries.

## Things to know about regions
- Azure has more global regions than any other cloud provider.
- Regions provide customers the flexibility and scale needed to bring applications closer to their users.
- Regions provide data residency and offer comprehensive compliance and resiliency options for customers.
- For most Azure services, when you deploy a resource in Azure, you choose the region where you want your resource to be deployed.
- Some services or VM features are only available in certain regions, such as specific VM sizes and storage types.
- Some global Azure services do not require you to select a region. These services include Azure AD, Microsoft Azure Traffic Manager, and Azure DNS.
- Each Azure region is paired with another region within the same geography, together making a regional pair. The exception is Brazil South, which is paired with a region outside its geography.

## Things to know about regional paris
- **Physical isolation**: Azure prefers  at least 300 miles of separation between datacenters in a regional pair, although this isn't practical or possible in all geographies. Physical datacenter separation reduces the likelihood of natural disasters, civil unrest, power outages, or physical network outages affecting both regions at once.
- **Platform-provided replication**: Some services such as Geo-Redundant Storage provide automatic replication to the paired region.
- **Region recovery order**: Planned Azure system updates are rolled out to paired regions sequentially (not at the same time). Rolling updates minimize downtime, reduces bugs, and logical failures in the rare event of a bad update.
- **Data residency**: A region resides within the same geography as its pair (except for Brazil South) to meet data residency requirements for tax and law enforcement jurisdiction purposes.

<hr>

## Implement Azure subscriptions
An Azure subscription is a logical unit of Azure services that is linked to an Azure account. Billing for Azure services is done on a per-subscription basis. If your account is the only account associated with a subscription, then you are responsible for billing.

Subscriptions help you organize access to cloud service resources. They also help you control how resource usage is reported, billed, and paid for. Each subscription can have a different billing and payment setup, so you can have different subscriptions and different plans by department, project, regional office, and so on. Every cloud service belongs to a subscription, and the subscription ID may be required for programmatic operations.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-subscriptions/media/azure-subscriptions-e855533e.png">

## Azure accounts
Subscriptions have accounts. An Azure account is simply an identity in Azure Active Directory (Azure AD) or in a directory that is trusted by Azure AD, such as a work or school organization. If you don't belong to one of these organizations, you can sign up for an Azure account by using your Microsoft Account, which is also trusted by Azure AD.

## Getting access to resources
Every Azure subscription can be associated with an Azure Active Directory. Users and services will then authenticate with Azure Active Directory before accessing resources.

<hr>

## Obtain a subscription
There are several ways to get an Azure subscription: Enterprise agreements, Microsoft resellers, Microsoft partners, and a personal free account.

## Enterprise agreements
Any Enterprise Agreement customer can add Azure to their agreement by making an upfront monetary commitment to Azure. That commitment is consume throughout the year by using any combination of the wide variety of cloud services Azure offers. Enterprise agreements have a 99.95% monthly SLA.

## Reseller
Buy Azure through the Open Licensing program, which provides a simple, flexible way to purchase cloud services from your Microsoft reseller.

## Partners
Find a Microsoft partner who can design and implement your Azure cloud solution. These partners have the business and technology expertise to recommend solutions that meet the unique needs of your business.

## Personal free account
With a free trial account, you can get started using Azure right away and you won't be charged until you choose to upgrade.

<hr>

## Identify subscription usage
Azure offers free and paid subscription options to suite different needs and requirements. The most commonly used subscriptions are:
- Free
- Pay-As-You-Go
- Enterprise Agreement
- Student

## Azure free subscription
An Azure free subscription includes a monetary credit to spend on any service for the first 30 days, free access to the most popular Azure products for 12 months, and access to more than 25 products that are always free. An Azure free subscription is an excellent way for new users to get started. To set up a free subscription, you need a phone number, a credit cad, and a Microsoft account.

## Azure pay-as-you-go subscription
A Pay-As-You-Go (PAYG) subscription charges you monthly for the services you used in that billing period. This subscription type is appropriate for a wide range of users, from individuals to small businesses, and many large organizations as well.

## An Enterprise Agreement
An Enterprise Agreement provides flexibility to buy cloud services and software licenses under one agreement, with discounts for new licenses and Software Assurance. It's targeted at enterprise-scale organizations.

## Azure for students subscription
An Azure for Students subscription includes a monetary credit to be used within the first 12 months. Also students can select free services without requiring a credit card at sign-up. You must verify your student status through your organizational email address.

<hr>

## Implement cost management
With Azure products and services, you only pay for what you use. As you create and use Azure resources, you are charged for the resources. You use Microsoft Cost Management and Billing features to conduct billing administrative tasks and manage billing access to costs. You also use its features to monitor and control Azure spending and to optimize Azure resource usage.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-subscriptions/media/cost-management-67750831.png">

Cost Management shows organizational cost and usage patterns with advanced analytics. Reports in Cost Management show the usage-based costs consumed by Azure services and third-party Marketplace offerings. Costs are based on negotiated prices and factor in reservation and Azure Hybrid Benefit discounts. Collectively, the reports show your internal and external costs for usage and Azure Marketplace charges. Other charges, such as reservation purchases, support, and taxes are not yet shown in reports. The reports help you understand your spending and resource use and can help find spending anomalies. Predictive analytics are also available. Cost Management uses Azure management groups, budgets, and recommendations to show clearly how your expenses are organized and how you might reduce costs.

You can use the Azure portal or various APIs for export automation to integrate cost data with external systems and processes. Automated billing data export and scheduled reports are also available.

## Plan and control expenses
The ways that Cost Management help you plan for and control your costs include: Cost analysis, budgets, recommendations, and exporting cost management data. 

- **Cost analysis**: You use cost analysis to explore and analyze your organizational costs. You can view aggregated costs by organization to understand where costs are accrued and to identify spending trends. You can see accumulated costs over time to estimate monthly, quarterly, or even yearly cost trends against a budget.
- **Budgets**: Budgets help you plan for and meet financial accountability in your organization. They help prevent cost thresholds or limits from being surpassed. Budgets can also help you inform others about their spending to proactively manage costs. And with them, you can see how spending progresses over time.
- **Recommendations**: Recommendations show how you can optimize and improve efficiency by identifying idle and underutilized resources. Or, they can show less expensive resource options. When you act on the recommendations, you change the way you use your resources to save money. To act, you first view cost optimization recommendations to view potential usage inefficiencies. Next, you act on a recommendation to modify your Azure resource use to a more cost-effective option. Then you verify the action to make sure that the change you make is successful. 
- **Exporting cost management data**: If you use external systems to access or review cost management data, you can easily export the data from Azure. You can set a daily scheduled export in CSV format and store the data files in Azure storage. Then, you can access the dat from your external system.

<hr>

## Apply resource tagging
You can apply tags to your Azure resources to logically organize them by categories. Each tag consists of a name and a value. For example, you can apply the name *Environment* and the value *Production* or *Development* to your resources. After creating your tags, you associate them with the appropriate resources.

With tags in place, you can retrieve all the resources in your subscription with the tag name and value. This means, you can retrieve related resources from different resource groups.

Perhaps one of the best uses of tags is to group billing data. When you download the usage CSV for services, the tags appear in the Tags column. You could then group virtual machines by cost center and production environment.

## Considerations
There are a few things to remember about tagging:
- Each resource or resource group can have a maximum of 50 tag name/value pairs.
- Tags applied to a resource group are not inherited by the resources in that resource group.

Note: When you need to create a lot of resource tags you will want to do that programmatically. You can use PowerShell or the CLI.

<hr>

## Apply cost savings
**Reservations** help you save money by paying ahead. You can pay for one year or three years of virtual machine, SQL Database compute capacity, Azure Cosmos DB throughput, or other Azure resources. Pre-paying allows you to get a discount on the resources you use. Reservations can significantly reduce your virtual machine, SQL database compute, Azure Cosmos DB, or other resource costs up to 72% on pay-as-you-go prices. Reservations provide a billing discount and don't affect the runtime state of your resources.

**Azure Hybrid Benefits** is a pricing benefit for customers who have licenses with Software Assurance. Azure Hybrid Benefits helps maximize the value of existing on-premises Windows Server or SQL Server license investments when migrating to Azure. There's an Azure Hybrid Benefit Savings Calculator to help you determine your savings.

**Azure Credits** is a monthly credit benefit that allows you to experiment with, develop, and test new solutions on Azure. For example, as a Visual Studio subscriber, you can use Microsoft Azure at no extra charge. With your monthly Azure credit, Azure is you personal sandbox for dev/test.

**Budgets** help you plan for and drive organizational accountability. With budgets, you can account for the Azure services you consume or subscribe to during a specific period. They help you inform others about their spending to proactively manage costs, and to monitor how spending progresses over time. When the budge threshold you've created are exceeded, only notifications are triggered. Noe of your resources are affected and your consumption isn't stopped. You can use budgets to compare and track spending as you analyze costs.

**Pricing calculator** provides estimates in all areas of Azure including compute, networking, storage, web, and databases.

<hr>

## Knowledge check
1. The company financial controller wants to be notified whenever the company is half-way to spending the money allocated for cloud services. Which of the following is the best approach to meeting this requirement? -> Create a budget and a spending threshold.
2. The company financial controllers wants to identify which billing department each Azure resource belongs to. Which of the following is the best approach to meeting this requirement? -> Apply a tag to each resource that includes the associated billing department
3. Which of the following preserves data residency and offers comprehensive compliance and resiliency options? -> Regions

<hr>

## Configure Azure policy
You will learn how to configure Azure Policy to implement compliance requirements.

## Objectives
- Create management groups to target policies and spend budgets.
- Implement Azure policy with policy and initiative definitions.
- Scope Azure policies and determine compliance.

## Create management groups
Azure management groups provide a level of scope above subscriptions. You organize subscriptions into containers called *management groups* and apply your governance conditions to the management groups. Management groups enable:
- Organizational alignment for your Azure subscriptions through custom hierarchies and grouping.
- Targeting of policies and spend budgets across subscriptions and inheritance down the hierarchies.
- Compliance and cost reporting by organization (business/teams).

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-policy/media/management-groups-aa92c04a.png">

All subscriptions within a management group automatically inherit the conditions applied to the management group. For example, you can apply policies to a management group that limits the regions available for VM creation. This policy would be applied to all management groups, subscriptions, and resources under that management group by only allowing VMs to be created in that region.

## Adding management groups
You can create the management group by using the portal, PowerShell, or Azure CLI.

- The **Management Group ID** is the directory unique identifier that is used to submit commands on this management group. This identifier is not editable after creation as it is used throughout the Azure system to identify this group.
- The **Display Name** field is the name that is displayed within the Azure portal. A separate display name is an option field when creating the management group and can be changed at any time.

<hr>

## Implement Azure policies
Azure Policy is a service in Azure that you use to create, assign, and manage policies. These policies enforce different rules over your resources, so those resources stay compliant with your corporate standards and service level agreements. Azure Policy runs evaluations and scans for resources that are not compliant.

The main advantages of Azure policy are in the areas of enforcement and compliance, scaling, and remediation.

- **Enforcement and compliance**: Turn on built-in policies or build custom ones for all resource types, real-time policy evaluation and enforcement, periodic and on-demand compliance evaluation.
- **Apply policies at scale**: Apply policies to a Management Group with control across your entire organization. Apply multiple policies and aggregate policy states with policy initiative. Define and exclusion scope.
- **Remediation**: Real-time remediation, and remediation on existing resources.

Azure Policy will be important to you if your team runs an environment where you need to govern:
- Multiple engineering teams (deploying to and operating in the environment)
- Multiple subscriptions
- Need to standardize/enforce how cloud resources are configured
- Manage regulatory compliance, cost control, security, or design consistency

## Use cases
- Specify the resource types that your organization can deploy.
- Specify a set of VM SKUs that your organization can deploy.
- Restrict the locations your organization can specify when deploying resources.
- Enforce a required tag and its value.
- Audit if Azure Backup service is enabled for all VMs.

<hr>

## Create Azure policies
To implement Azure Policies, you can follow these steps.

1. **Browse Policy Definitions**: A Policy Definition expresses what to evaluate and what actions to take. Every policy definition has conditions under which it is enforced. And, it has an accompanying effect that takes place if the conditions are met. For example, you could prevent VMs from being deployed if they are exposed to a public IP address.
2. **Create Initiative Definitions**: An initiative definition is a set of Policy Definitions to help track your compliance state for a larger goal. For example, ensuring a branch office is compliant.
3. **Scope the Initiative Definition**: You can limit the scope of the Initiative Definition to Management Groups, Subscriptions, or Resource Groups.
4. **View Policy Evaluation results**: Once an Initiative Definition is assigned, you can evaluate the state of compliance for all your resources. Individual resources, resource groups, and subscriptions within a scope can be exempted from having policy rules affect it. Exclusions are handled individually for each assignment.

<hr>

## Create policy definitions
There are many Built-in Policy Definitions for you to choose from. Sorting by Category will help you locate what you need. For example,

- The Allowed Virtual Machine SKUs enables you to specify a set of virtual machine SKUs that your organization can deploy.
- The Allowed Locations policy enables you to restrict the locations that your organization can specify when deploying resources. The Allowed Locations policy can be used to enforce your geo-compliance restrictions.

When there isn't an applicable policy you can add a new Policy Definition. You can import a policy definition from GitHub. 

Note: Policy Definitions have a specific JSON format.

<hr>

## Create initiative definitions
Once you have determined which Policy Definitions you need, you create an Initiative Definition. This definition will contain one or more policies. There is a pick list on the right side of the New Initiative definition page (not shown) to make your selection.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-policy/media/create-initiative-definition-e1198a51.png">

<hr>

## Scope the initiative definition
Once our Initiative Definition is created, you can assign the definition to establish its scope. A scope determines what resources or grouping of resources the policy assignment gets enforced on.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-policy/media/assign-definition-87bc203c.png">

You can select the Subscription, and then optionally a Resource Group.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-policy/media/scope-initiative-9fbf59d5.png">

<hr>

## Determine compliance
Once your policy is in place, you can use the Compliance blade to review non-compliant initiatives, non-compliant policies, and non-compliant resources.
<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-policy/media/determine-compliance-c198f4ba.png">

Policy conditions are evaluated against your existing resources. although the portal doesn't show the evaluation logic, the compliance state results are shown. The compliance state result is either compliant or non-compliant.

<hr>

## Knowledge check
1. There are several Azure policies that need to be applied to a new branch office. What is the best way to proceed? -> Create a policy initiative
2. The finance team wants to categorize resources and billing for different departments like Research and Human Resources. The billing needs to be consolidated across multiple resource groups to ensure everyone complies with the solution. Resource tags have been created for each department. What should be done next? -> Create an Azure policy
3. The finance team wants to ensure that only cost-effective VM SKU sizes are deployed. What is the best way to meet this requirement? -> Create a policy in Azure Policy that specifies the allowed SKU sizes.
4. Which of the following can be used to manage governance across multiple Azure subscriptions? -> Management groups

<hr>

## Summary
Azure Policy is a service in Azure that enables you to create, assign, and manage policies that control or audit your resources. Azure Policy helps you define and implement your governance strategy.

<hr>

## Configure role-based access control
You will learn how to use role-based access control to ensure resources are protected, but users can still access the resources they need.

## Objectives
- Identify the features are usage cases for role-based access control.
- List and create role definitions.
- Create role assignments.
- Identify the differences between Azure role-based access control and Azure Active Directory roles.
- Manage access to subscriptions using role-based access control.
- Review the built-in Azure role-based access control roles.

<hr>

## Implement role-based access control
Role-based access control (RBAC) helps you manage who has access to Azure resources, what they can do with those resources, and what areas they have access to. Azure RBAC is an authorization system built on Azure Resource Manager that provides fine-grained access management of resources in Azure.

## What can I do with Azure roles?
- Allow an application to access all resources in a resource group
- Allow one user to manage virtual machines in a subscription and another user to manage virtual networks
- Allow a DBA group to manage SQL databases in a subscription
- Allow a user to manage all resources in a resource group, such as VMs, websites, and subnets

## Concepts
- **Security principal**: Object that represents something that is requesting access to resources. Examples: user, group, service principal, managed identity.
- **Role definition**: Collection of permissions that lists the operations that can be performed. Examples: Reader, Contributor, Owner, User Access Administrator.
- **Scope**: Boundary for the level of access that is requested. Examples: management group, subscription, resource group, resource.
- **Assignment**: Attaching a role definition to a security principal at a particular scope. Users can grant access described in a role definition by creating an assignment. Deny assignments are currently read-only and can only be set by Azure.

## Considerations
Using Azure RBAC, you can segregate duties within your team and grant only the amount of access to users that they need to perform their jobs. Instead of giving everybody unrestricted permissions in your Azure subscription or resources, you can allow only certain actions at a particular scope.

When planning your access control strategy, it's a best practice to grant users the least privilege to get their work done.

<hr>

## Create a role definition
Each role is a set of properties defined in a JSON file. For example, Actions, NotActions, and DataActions. In the example below, the Owner role means all (asterisk) actions, no denied actions, and all (/) scopes.

        Name: Owner
        ID: 8e3af657-a8ff-443c-a75c-2fe8c4bcb65
        IsCustom: False
        Description: Manage everything, including access to resources
        Actions: {*}
        NotActions: {}
        AssignableScopes: {/}

## Actions and NotActions
The Actions and NotActions properties can be tailored to grant and deny the exact permissions you need.

## Scope your role
After defining the Actions and NotActions properties, you must scope the role. The AssignableScopes property of the role specifies the role scope. the scope can be subscriptions, resource groups, or resources.

        * /subscriptions/[subscription id]
        * /subscriptions/[subscription id]/resourceGroups/[resource group name]
        * /subscriptions/[subscription id]/resourceGroups/[resource group name]/[resource]

## Example 1
Make a role available for assignment in two subscriptions:

        “/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624”
## Example 2
Makes a role available for assignment only in the Network resource group.

        “/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network”

<hr>

## Create a role assignment
A role assignment is the process of scoping a role definition to a user, group, service principal, or managed identity. The purpose of the role assignment is to grant access. Access is revoked by removing a role assignment.

Note: A resource inherits role assignments from its parent resource.

<hr>

## Differences between Azure roles and Azure Active Directory roles
At a high level, Azure RBAC roles control permissions to manage Azure resources, while Azure AD administrator roles control permissions to manage Azure Active Directory resources. The following table compares some of the differences.

**Azure RBAC roles** | **Azure AD roles**
-------------------- | ------------------
Manage access to Azure resources | Manage access to Azure Active Directory resources
Scope can be specified at multiple levels (management group, subscription, resource group, resource) | Scope is at the tenant level
Role information can be accessed in Azure portal, Azure CLI, Azure PowerShell, Azure Resource Manager templates, REST API | Role information can be accessed in Azure admin portal, Microsoft 365 admin portal, Microsoft Graph AzureAD PowerShell

Note: Azure Resource Manager roles should be used instead of Classic administrator roles.

<hr>

## Determine role-based access control roles
Azure includes several built-in roles that you can use. There are four fundamental built-in roles. The first three apply to all resource types.

- **Owner**: Has full access to all resources including the right to delegate access to others. The Service Administrator and Co-Administrators are assigned the Owner role at the subscription scope.
- **Contributor**: Can create and manage all types of Azure resources but can't grant access to others.
- **Reader**: Can view existing Azure resources.
- **User Access Administrator**: Lets you manage user access to Azure resources.

## Other things to know
- There are other built-in roles. For example, the **Virtual Machine Contributor** role allows a user to create and manage virtual machines.
- When the built-in roles don't meet the specific needs of your organization, you can create your own custom roles.
- Roles can grant access to data within an object. For example, if a user has read data access to a storage account, then they can read the blobs or messages in the storage account.

## Knowledge
1. There are three virtual machines (VM1, VM2, and VM3) in a resource group. The Helpdesk hires a new employee. The new employee must be able to modify the settings on VM3. The employee must not be able to make changes on VM1 and VM2. Which of the following meets the requirements and minimizes administrative overhead? -> Assign the user to the Contributor role on VM3.
2. What's the main difference between Azure roles and Azure Active Directory roles? -> Azure roles apply to Azure resources. Azure AD roles apply to Azure AD resources such as users, groups, and domains.
3. What is included in a custom Azure role definition? -> Operations allowed for Azure resources and the scope of permissions.

<hr>

## Configure Azure Active Directory

## Objectives
- Identify the features and uses of Azure Active Directory.
- Define the main Azure Active Directory components such as identity, account and tenant.
- Compare Active Directory Domain Services to Azure Active Directory.
- Identify features of Azure Active Directory editions.
- Identify features and usage cases for Azure AD Join.
- Identify features and usage cases for Self-Service Password Reset.

<hr>

## Describe Azure Active Directory benefits and features
Azure Active Directory (Azure AD) is Microsoft's multi-tenant cloud-based directory and identity management service.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-active-directory/media/azure-active-directory-a3b1df09.png">

## Benefits and features
- **Single sign-on to any cloud or on-premises web app**: Azure AD provides secure single sign-on to cloud and on-premises applications. SSO includes Microsoft 365 and thousands of SaaS applications such as Salesforce, Workday, DocuSign, ServiceNow, and Box.
- **Works with iOS, macOS, Android, and Windows devices**: Users can launch applications from a personalized web-based access panel, mobile app, Microsoft 365, or custom company portals using their existing work credentials. The experience is the same on iOS, macOS, Android, and Windows devices.
- **Protect on-premises web applications with secure remote access**: Access our on-premises web applications from everywhere and protect with multifactor authentication, conditional access policies, and group-based access management. Users can access SaaS and on-premises web apps from the same portal.
- **Easily extend Active Directory to the cloud**: You can connect Active Directory and other on-premises directories to Azure Active Directory in just a few steps. This connection means a consistent set of users, groups, passwords, and devices across both environments.
- **Protect sensitive data and applications**: You can enhance application access security with unique identity protection capabilities. This includes a consolidated view into suspicious sign-in activities and potential vulnerabilities. you can also take advantage of advanced security reports, notifications, remediation recommendations, and risk-based policies.
- **Reduce costs and enhance security with self-service capabilities**: Delegate important tasks such as resetting passwords and the creation of management of groups to your employees. Providing self-service application access and password management through verification steps can help reduce helpdesk calls and enhance security.

<hr>

## Describe Azure Active Directory concepts
It is important to understand these Azure AD concepts.
- **Identity**: An object that can get authenticated. An identity can be a user with a username and password. Identities also include applications or other servers that might require authentication through secret keys or certificates.
- **Account**: An identity that has data associated with it. You can't have an account without an identity.
- **Azure AD Account**: An identity created through Azure AD or another Microsoft cloud service, such as Microsoft 365. Identities are stored in Azure AD and accessible to your organization's cloud service subscriptions. This account is also sometimes called a Work or school account.
- **Azure subscription**: Used to pay for Azure cloud services. You can have many subscriptions and they're linked to a credit card.
- **Azure tentant/directory**: A dedicated and trusted instance of Azure AD, a Tenant is automatically created when your organization signs up for a Microsoft cloud service subscription.

- More instances of Azure AD can be created
- Azure AD is the underlying product providing the identity service
- The term Tenant means a single instance of Azure AD representing a single organization
- The terms Tenant and Directory are often used interchangeably

<hr>

## Compare Active Directory Domain services to Azure Active Directory
AD DS is the traditional deployment of Windows Server-based Active Directory on a physical or virtual server. Although AD DS is commonly considered to be primarily a directory service, it is only one component of the Windows Active Directory suite of technologies, which also includes Active Directory Certificate Services (AD CS), Active Directory Lightweight Directory Services (AD LDS), Active Directory Federation Services (AD FS), and Active Directory Rights Management Services (AD RMS). Although you can deploy and manage AD DS in Azure virtual machines it's recommended you use Azure AD instead, unless you are targeting IaaS workloads that depend on AD DS specifically.

## Azure Active Directory is different
Although Azure AD has many similarities to AD DS, there are also many differences. It is important to realize that using Azure AD is different from deploying an Active Directory domain controller on an Azure virtual machine and adding it to your on-premises domain. Here are some characteristics of Azure AD that make it different.

- **Identity solution**: Azure AD is primarily an identity solution, and it is designed for Internet-based applications by  using HTTP and HTTPS communications.
- **REST API Querying**: Because Azure AD is HTTP/HTTPS based, it cannot be queried through LDAP. Instead, azure AD uses the REST API over HTTP and HTTPS.
- **Communication Protocols**: Because Azure AD is HTTP/HTTPS based, it does not use Kerberos authentication. Instead, it uses HTTP and HTTPS protocols such as SAML, WS-Federation, and OpenID Connect for authentication (and OAuth for authorization).
- **Federation Services**: Azure AD includes federation services, and many third-party services (such as Facebook).
- **Flat structure**: Azure AD users and groups are created in a flat structure, and there are no Organizational Units (OUs) or Group Policy Objects (GPOs).

Note: Azure AD is a managed service. You only manage the users, groups, and policies. Deploying AD DS with virtual machines using Azure means that you manage the deployment, configuration, virtual machines, patching, and other backend tasks.

<hr>

## Select Azure Active Directory editions
Azure Active Directory comes in four editions-**Free, Microsoft 365 Apps, Premium P1,** and **Premium P2**. The Free edition is included with an Azure subscription. The Premium editions are available through a Microsoft Enterprise Agreement, the Open Volume License Program, and the Cloud Solution Providers program. Azure and Microsoft 365 subscribers can also buy Azure Active Directory Premium P1 and P2 online.

**Azure Active Directory Free**: Provides user and group management, on-premises directory synchronization, basic reports, and single sign-on across Azure, Microsoft 365, and many popular SaaS apps.

**Azure Active Directory Microsoft 365 Apps**: This edition is included with O365. In addition to the Free features, this edition provides Identity & Access Management for Microsoft 365 apps including branding, MFA, group access management, and self-service password reset for cloud users.

**Azure Active Directory Premium P1**: In addition to the Free features, P1 also lets your hybrid users access both on-premises and cloud resources. It also supports advanced administration, such as dynamic groups, self-service group management, Microsoft Identity Manager (an on-premises identity and access management suite) and cloud write-back capabilities, which allow self-service password reset for your on-premises users.

**Azure Active Directory Premium P2**: In addition to the Free and P1 features, P2 also offers Azure Active Directory Identity Protection to help provide risk-based Conditional Access to your apps and critical company data. Privileged Identity Management is included to help discover, restrict, and monitor administrators and their access to resources and to provide just-in-time access when needed.

<hr>

## Implement Azure Active Directory join
Azure Active Directory (Azure AD) enables single sign-on to devices, apps, and services from anywhere. IT administrators must ensure corporate assets are protected and that devices meet standards for security and compliance.

Azure AD Join is designed to provide access to organizational apps and resources and to simplify Windows deployments of work-owned devices. AD Join has these benefits:

- **Single-Sign-On (SSO)** to your Azure-managed SaaS apps and services. Your users won't have additional authentication prompts when accessing work resources. The SSO functionality is available even when users are not connected to the domain network.

- **Enterprise state roaming** of user settings across joined devices. With Windows 10, users gain the ability to securely synchronize their user settings and application settings data to the cloud. This reduces the time to configure a new device.

- **Access to Microsoft Store for Business** using an Azure AD account. Your users can choose from an inventory of applications pre-selected by the organization.
- **Windows Hello** support for secure and convenient access to work resources.
- **Restriction of access** to apps from only devices that meet compliance policy.
- **Seamless access to on-premises resources** when the device has line of sight to the on-premises domain controller.

## Connection options
To get a device under the control of Azure AD, you have two options:

- **Registering** a device to Azure AD enables you to manage a device's identity. Azure AD device registration provides the device with an identity that is used to authenticate the device when a user signs-in to Azure AD. You can use the identity to enable or disable a device.
- **Joining** a device is an extension to registering a device. Joining provides the benefits of registering and changes the local state of a device. Changing the local state enables your users to sign-in to a device using an organizational work or school account instead of a personal account.

Note: Although AD Join is intended for organizations that do not have on-premises Windows Server Active Directory infrastructure it can be used for other scenarios like branch offices.

<hr>

## Implement self-service password reset
Enabling **Self-service Password Reset** (SSPR) gives the users the ability to bypass the helpdesk and reset their own passwords.

To configure Self-Service Password Reset, you first determine who will be enabled to use self-service password reset. From your existing Azure AD tenant, on the Azure portal under **Azure Active Directory (Users)** select **Password reset**.

In the Password reset properties there are three options: **None, Selected**, and **All**.

The **Selected** option is useful for creating specific groups who have self-service password reset enabled. You can create a group for testing or proof of concept before deploying to a larger group. Once you are ready to deploy this functionality to all users with accounts in your AD Tenant, you can change the setting.

## Authentication methods
After enabling password reset for user and groups, you pick the number of authentication methods required to reset a password and the number of authentication methods available to users. At least one authentication method is required to reset a password. It is a good idea to have other methods available. You can choose from email notification, a text, or code sent to user's mobile or office phone, or a set of security options.

You can require security questions to be registered for the users in your AD tenant. You can also configure how many correctly answered security questions are required for a successful password reset. Security questions can be less secure than other methods because some people might know the answers to another user's questions.

Note: Azure Administrator accounts can always reset their passwords no matter what options are configured.

<hr>

## Knowledge check
1. Which of the following correctly describes Azure Active Directory? -> Azure AD is primarily an identity solution.
2. A dedicated and trusted instance of Azure Active Directory is often referred to as? -> An Azure tenant.
3. Your users want to sign-in to devices, apps, and services from anywhere. Users want to sign-in using an organizational work or school account instead of a personal account. What should you do first? -> Join the device.

<hr>

## Create Azure users and groups in Azure Active Directory
Create users in Azure Active Directory. Understand different types of groups. Create a group and add members. Manage business-to-business guest accounts.

## Objectives
- Add users to Azure Active Directory.
- Manage app and resource access by using Azure Active Directory groups.
- Give guest users access in Azure Active Directory business to business (B2B).

## What are user accounts in Azure Active Directory?
In Azure Active Directory (Azure AD), all user accounts are granted a set of default permissions. A user's account access consists of the type of user, their role assignments, and their ownership of individual objects.

There are different types of user accounts in Azure AD. Each type has a level of access specific to the scope of work expected to be done under each type of user account. Administrators have the highest level of access, followed by the member user accounts in the Azure AD organization. Guest users have the most restricted level of access.

## Permission and roles
Permissions help you control the access rights a user or group, which are assigned by roles. When someone is assigned a role, they inherit permissions from that role.

## Administrator roles
Administrator roles allow users elevated access to control who is allowed to do what.

## Member users
A role with default permissions like being able to manage their profile information. New members are usually assigned this role. Anyone who isn't an administrator or a guest usually falls into this role.

## Guest users
Guest users have restricted Azure AD organization permissions. When you invite someone to collaborate with your organization, you add them to your Azure AD organization as a guest user. Then you can either send an invitation email that contains a redemption link or send a direct link to an app you want to share. Guest users sign in with their own work, school, or social identities. By default, Azure AD member users can invite guest users. This default can be disabled by someone who has the User Administrator role.

## Add user accounts
You can add individual user accounts through the Azure portal, Azure PowerShell, or the Azure CLI.

If you want to use the Azure CLI, run the following cmdlet:
        
        # Create a new user
        az ad user create

This command creates a new user by using the Azure CLI.

For Azure PowerShell, run the following cmdlet:

        # Create a new user
        New-AzureADUser

You can bulk create member users and guests accounts. The following example shows how to bulk invite guest users.

        $invitations = import-csv c:\bulkinvite\invitations.csv

        #messageInfo = New-Object Microsoft.Open.MSGraph.Model.InvitedUserMessageInfo

        $messageInfo.customizedMessageBody = "Hello. You are invited to the Contoso organization."

        foreach ($email in $invitations)
                {New-Azure-ADMSInvitation `
                        -InvitedUserEmailAddress $email.InvitedUserEmailAddress `
                        -InvitedUserDisplayName $email.Name `
                        -InvitedRedirectUrl https://myapps.microsoft.com `
                        -SendInvitationMessage $true
                }

You can create the comma-separated value (CSV) file with the list of all the users you want to add. An invitation is sent to each user in that CSV file.

## Delete user accounts
You can also delete user accounts through the Azure portal, Azure PowerShell, or the Azure CLI. In PowerShell, run the cmdlet `Remove-AzureADUser`. In the Azure CLI, run the cmdlet `az ad user delete`.

When you delete a user, the account remains in a suspended state for 30 days. During that 30-day window, the user account can be restored.

## Knowledge check
1. If you delete a user account by mistake, can it be restored? -> The user account can be restored, but only if it was deleted within the last 30 days.
2. What kind of account would you create to allow an external organization easy access? -> A guest user account for each member of the external team.

<hr>

## Manage app and resource access by using Azure Active Directory groups
Azure Active Directory (Azure AD) helps you to manage your cloud-based apps, on-premises apps, and resources by using your organization's groups. Your resources can be part of the Azure AD organization, like permissions to manage objects through roles. Or your resources can be external to the organization, like software as a service (SaaS) apps, Azure services, SharePoint sites, and on-premises resources.

## Access management in Azure AD
- **Azure AD roles**: Use Azure AD roles to manage Azure AD-related resources like users, groups, billing, licensing, application registration, and more.
- **Role-based access control (RBAC) for Azure resources**: User RBAC roles to manage access to Azure resources like virtual machines, SQL databases, or storage. For example, you could assign an RBAC role to a user to manage and delete SQL databases in a specific resource group or subscription.

## Access rights through single user or group assignment
- **Direct assignment**: Assign a user the required access rights by directly assigning a role that has those access rights.
- **Group assignment**: Assign a group the required access rights, and members of the group will inherit those rights.
- **Rule-based assignment**: Use rules to determine a group membership based on user or device properties. For a user account or device's group membership to be valid, the user or device must meet the rules. If the rules aren't met, the user account or device's group membership is no longer valid. The rules can be simple. You can select prewritten rules or write your own advanced rules.

<hr>

## Collaborate by using guest accounts and Azure Active Directory B2B
With Azure Active Directory business to business (B2B), you can add people form other companies to your Azure AD tenant as guest users.

## Guest user access in Azure AD B2B
In any scenario where external users need temporary or restricted access to your organization's resources, give them guest user access. You can grant guest user access with the appropriate restrictions in place. Then remove access when the work is done.

You can use the Azure portal to invite B2B collaboration users. Invite guest users to the Azure AD organization, group, or application. After you invite a user, their account is added to Azure AD as a guest account. 

The guest can get the invitation through email. Or you can share the invitation to an application by using a direct link. The guest then redeems their invitation to access the resources.

By default, users and administrators in Azure AD can invite guest users. But this ability can be limited or disabled by the Global Administrator.

## Collaborate with any partner by using their identities
If your organization has to manage the identities of each external guest user who belongs to a given partner organization, it faces increased responsibilities. With Azure Active Directory B2B, you don't have to manage your external users' identities. The partner has the responsibility to manage its own identities. External users continue to use their current identities to collaborate with your organization.

## Why use Azure AD B2B instead of federation?
With Azure AD B2B, you don't take on the responsibility of managing and authenticating the credentials and identities of partners. Your partners can collaborate with you even if they don't have an IT department.

Giving access to external users is much easier than in a federation. You don't need an AD administrator to create and manage external user accounts. Any authorized user can invite other users.

A federation is more complex. A federation is where you have a trust established with another organization, or a collection of domains, for shared access to a set of resources. You might be using an on-premises identity provider and authorization service like Active Directory Federation Services (AD FS) that has an established trust with Azure AD. To get access to resources, all users have to provide their credentials and successfully authenticate against the AD FS server. If you have someone trying to authenticate outside the internal network, you need to set up a web application proxy. The architecture might look something like the following diagram: 

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/create-users-and-groups-in-azure-active-directory/media/6-federation-example.png">

An on-premises federation with Azure AD might be good if your organization wants all authentication to Azure resources to happen in local environment. Administrators can implement more rigorous levels of access control. But this means that, if your local environment is down, users can't access the Azure resources and services they need.

With a B2B collaboration, external teams get the required access to Azure resources and services with the appropriate permissions. There's no need for a federation and trust to be established, and authentication doesn't depend on an on-premises server. Authentication is done directly through Azure. Collaboration becomes simplified, and you don't have to worry about situations where users can't sign in because an on-premises directory isn't available.

<hr>

## Secure your Azure resources with Azure role-based access control (Azure RBAC)
Learn how to use Azure RBAC to manage access to resources in Azure.

## Objectives
- Verify access to resources for yourself and others
- Grant access to resources
- View activity logs of Azure RBAC changes

<hr>

## Azure subscriptions
First, remembers that each Azure subscription is associated with a single Azure AD directory. Users, groups, and applications in that directory can manage resources in the Azure subscription. The subscriptions use Azure AD for single sign-on (SSO) and access management. You can extend your on-premises Active Directory to the cloud by using **Azure AD Connect**. This feature allows your employees to manage their Azure subscriptions by using their existing work identities. Why you disable an on-premises Active Directory account, it automatically loses access to all Azure subscriptions connected with Azure AD.

## What is Azure RBAC?
Azure role-based access control (Azure RBAC) is an authorization system built on Azure Resource Manager that provides fine-grained access management of resources in Azure. With Azure RBAC, you can grant the exact access that users need to do their jobs. For example, you can use Azure RBAC to let one employee manage virtual machines in a subscription while another manages SQL databases within the same subscription.

You grant access by assigning the appropriate Azure role to users, groups, and applications at a certain scope. The scope of a role assignment can be a management group, subscription, a resource group, or a single resource. A role assigned at a parent scope also grants access to the child scopes contained within it. For example, a user with access to a resource group can manage all the resources it contains, like websites, virtual machines, and subnets. The Azure role that you assign dictates what resources the user, group, or application can manage within that scope.

The following diagram depicts how the classic subscription administrator roles, Azure roles, and Azure AD roles are related at a high level. Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/secure-azure-resources-with-rbac/media/2-azuread-and-azure-roles.png">

In the preceding diagram, a subscription is associated with only one Azure AD tenant. Also note that a resource group can have multiple resources but is associated with only one subscription. Although it's not obvious from the diagram, a resource can be bound to only one resource group.

## What can I do with Azure RBAC?
Azure RBAC allows you to grant access to Azure resources that you control. 

Here are some scenarios you can implement with Azure RBAC:
- Allow one user to manage virtual machines in a subscription and another user to manage virtual networks
- Allow a database administrator group to manage SQL databases in a subscription
- Allow a user to manage all resources in a resource group, such as virtual machines, websites, and subnets
- Allow an application to access all resources in a resource group

## How does Azure RBAC work?
You control access to resources using Azure RBAC by creating role assignments, which control how permission are enforced. To create a role assignment, you need three elements: a security principal, a role definition, and a scope. YOu can think of these elements as "who", "what", and "where".

**1. Security principal (who)**
A *security principal* is just a fancy name for a user, group, or application that you want to grant access to.

**2. Role definition (what you can do)**
A *role definition* is a collection of permissions. It's sometimes just called a role. A role definition lists the permissions that can be performed, such as read, write, and delete. Roles can be high-level, like Owner, or specific, like Virtual Machine Contributor.

Azure includes several built-in roles that you can use. The following lists four fundamental built-in roles:

- Owner: Has full access to all resources, including the right to delegate access to others.
- Contributor: Can create and manage all types of Azure resources, but can't grant access to others.
- Reader: Can view existing Azure resources.
- User Access Administrator: Lets you manage user access to Azure resources.

If the built-in roles don't meet the specific needs of your organization, you can create your own custom roles.

**3. Scope (where)**
*Scope* is where the access applies to. This is helpful if you want to make someone a Website Contributor, but only for one resource group.

In Azure, you can specify a scope at multiple levels: management group, subscription, resource group, or resource. Scopes are structured in a parent-child relationship. When you grant access at a parent scope, those permissions are inherited by the child scopes. For example, if you assign the Contributor role to a group at the subscription scope, that role is inherited by all resource groups and resources in the subscription.

## Role assignment
Once you have determined the who, what, and where, you can combine those elements to grant access. A *role assignment* is the process of binding a role to a security principal at a particular scope, for the purpose of granting access. To grant access, you create a role assignment. To revoke access, you remove a role assignment.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/secure-azure-resources-with-rbac/media/2-rbac-overview.png">

## Azure RBAC is an allow model
Azure RBAC is an allow model. What this means is that when you are assigned a role, Azure RBAC allows you to perform certain actions, such as read, write, and delete. So, if one role assignment grants you read permissions to a resource group and a different role assignment grants you write permissions to the same resource group, you will have read and write permissions on that resource group.

Azure RBAC has something called `NotActions` permissions. Use `NotActions` to create a set of not allowed permissions. The access granted by a role, the effective permissions, is compted by subtracting the `NotActions` operations from the `Actions` operations. For example, the Contributor role has both `Actions` and `NotActions`. The wildcard (*) in `Actions` indicates that it can perform all operations on the control plane. The you subtract the following operations in `NotActions` to compute the effective permissions:
- Delete roles and role assignments
- Create roles and role assignments
- Grants the caller User Access Administrator access at the tenant scope
- Create or update any blueprint artifacts
- Delete any blueprint artifacts

<hr>

## Knowledge check
1. What is a role definition in Azure? -> A collection of permissions with a name that assignable to a user, group, or application.
2. Suppose an administrator wants to assign a role to allow a user to create and manage Azure resources but not be able to grant access to others. Which of the following built-in roles would support this? -> Contributor
3. What is the inheritance order for scope in Azure? -> Management group, Subscription, Resource group, Resource.
4. Suppose a team member can't view resources in a resource group. Where would the administrator go to check the team member's access? -> Go to the resource group and select **Access control (IAM) > Check Access.**
5. Suppose an administrator in another department needs access to a virtual machine managed by your department. What's the best way to grant them access to just that resource? -> At the resource scope, assign the role with the appropriate access.
6. Suppose a developer needs full access to a resource group. If you are following least-privilege best practices, what scope should you specify? -> Resource group
7. Suppose an administrator needs to generate a report of the role assignments for the last week. Where in the Azure portal would they generate that report? -> Search for **Activity log** and filter on the **Create role assignment (roleAssignments)** operation.

<hr>

## Summary
To grant access, you assign users a role at a particular scope. Using Azure RBAC, you can grant only the amount of access to users that they need to perform their jobs. Azure RBAC has over 70 built-in roles, but if your organization needs specific permissions, you can create your own custom roles. Azure keeps track of your Azure RBAC changes, in case you need to see what changes were made in the past.



