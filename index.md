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

<hr>

## Allow users to reset their password with Azure Active Directory self-service password reset
Evaluate self-service password reset to allow users in your organization to reset their passwords or unlock their accounts. Set up, configure, and test self-service password reset.

# Objectives
- Decide whether to implement self-service password reset
- Implement self-service password reset to meet your requirements
- Configure self-service password reset to customize the experience

<hr>

## Self-service password reset in Azure Active Directory
In Azure AD, any user can change their password if they're already signed in. But if they're not signed in and forgot their password or it's expired, they'll need to reset their password. With SSPR, users can reset their passwords in a web browser or from a Windows sign-in screen to regain access to Azure, Microsoft 365, and any other application that uses Azure AD for authentication.

## How SSPR works
The user initiates a password reset either by going directly to the password reset portal or by selecting the **Can't access your account** link on a sign-in page. The reset portal takes these steps:
1. **Localization**: The portal checks the browser's locale setting and renders the SSPR page in the appropriate language.
2. **Verification**: The user enter their username and passes a captcha to unsure it's a user and not a bot.
3. **Authentication**: The user enters the required data to authenticate their identity. They might, for example, enter a code or answer a security question.
4. **Password reset**: If the user passes the authentication tests, they can enter a new password and confirm it.
5. **Notification**: A message is sent to the user to confirm the reset.

## Authenticate a password reset
Azure supports six different ways to authenticate reset requests.
1. **Mobile app notification**: Azure sends a notification to the app
2. **Mobile app code**: Enter the code from the app
3. **Email**: Azure sends a code to the address
4. **Mobile phone**: Azure sends a text message
5. **Office phone**: You receive an automated phone call
6. **Security questions**: Answer the questions

In free and trial Azure AD organizations, phone call options aren't supported.


## Require the minimum number of authentication methods
You can specify the minimum number of methods that the user must set up: one or two.

For the security question method, you can specify a minimum number of questions that the user must set up to register for this method.

## Recommendations
- Enable two or more of the authentication reset request methods
- Use the mobile app notification or code as the primary method, but also enable the email or office phone methods to support users without mobile devices
- The mobile phone method isn't a recommended method because it's possible to send fraudulent texts
- The security question option is the least recommended method because the answers to the security questions might be known to other people.

## Account associated with administrator roles
- A strong, two-method authentication policy is always applied to accounts with an administrator role, regardless of your configuration for other users
- The security questions method isn't available to accounts that are associated with an administrator role

## Configure notifications
Administrators can choose how users are notified of password changes. There are two options that you can enable:
- Notify users on password resets
- Notify all admins when other admins reset their password

## License requirements
The password reset functionality you can use depends on your edition (free, P1, or P2). Any user who is signed in can change their password, regardless of the edition of Azure AD.

## SSPR deployment options
You can deploy SSPR with password writeback by using Azure AD Connect or cloud sync, depending on the needs of users. Each option can be deployed side-by-side in different domains to target different sets of users. This helps existing users on-premises to writeback password changes while adding an option for user in disconnected domains because of a company merger or split. Cloud sync can also provide higher availability because it doesn't rely on a single instance of Azure AD Connect.

<hr>

## Knowledge check
1. When is a user considered registered for SSPR? -> When they've registered at least the number of methods that you've required to reset a password.
2. When you enable SSPR for your Azure AD organization... -> Users can reset their passwords when they can't sign in.

<hr>

## Implement Azure AD self-service password reset
Before you start to configure SSPR, you need these things in place:
- An Azure AD organization. This organization must have at least a trial license enabled.
- An Azure AD account with Global Administrator privileges. You'll ust this account to set up SSPR.
- A non-administrative user account. You'll use this account to test SSPR (non-admin).
- A security group to test the configuration with. The non-admin account must be a member of this group. 

## Scope of SSPR rollout
There are three setting for the **Self-service password reset enabled** property:
- **Disabled**: No users in the Azure AD organization can use SSPR. This value is the default.
- **Enabled**: All users in the Azure AD organization can use SSPR.
- **Selected**: Only the members of the specified security group can use SSPR. You can use this option to enable SSPR for a targeted group of users, who can test it and verity that it works as expected. When you're ready to roll it out broadly, set the property to **Enabled** so that all users have access to SSPR.

## Configure SSPR
Here are the high-level steps to configure SSPR.
1. Go to the Azure portal, go to **Active Directory > Password reset**.
2. Properties:
- Enable SSPR.
- You can enable it for all users in the Azure AD organization or for selected users.
- To enable for selected users, you must specify the security group. Members of this group can use SSPR.
3. Authentication methods:
- Choose whether to require one or two authentication methods.
- Choose the authentication methods that the users can use.
4. Registration:
- Specify whether users are required to register for SSPR when they next sign in.
- Specify how often users are asked to reconfirm their authentication information.
5. Notifications: Choose whether to notify users and administrators of password resets.
6. Customization: Provide an email address or web address or web page URL where your users can get help.

<hr>
<hr>

## Implement and manage storage in Azure
You will learn how to configure blob storage including tiers and object replication.

## Objectives
- Identify features and usage cases for Azure Blob storage.
- Configure Blob storage and Blob access tiers.
- Configure Blob lifecycle management rules.
- Configure Blob object replication.
- Upload and price Blob storage.

<hr>

## Implement blob storage
Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs. Blob storage can store any type of text or binary data, such as a document, media file, or application installer. Blob storage is also referred to as object storage.

Common uses of Blob storage include:
- Serving images or documents directly to a browser.
- Storing files for distributed access, such as installation.
- Streaming video and audio.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

## Blob service resources
Blob storage offers three types of resources:
- The storage account
- Containers in the storage account
- Blobs in a container

The following diagram shows the relationship between these resources:

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-blob-storage/media/blob-storage-94fb52b8.png">

Note: Within the storage account, you can group as many blobs as needed in a container.

<hr>

## Create blob containers

A container provides a grouping of a set of blobs. All blobs must be in a container. An account can contain an unlimited number of containers. A container can store an unlimited number of blobs. You can create the container in the Azure portal.

**Name**: The name may only container lowercase letters, numbers, and hyphens, and must begin with a letter or a number. The name must also be between 3 and 63 characters long.

**Public access level**: Specifies whether data in the container may be accessed publicly. By default, container data is private to the account owner.

- Use **Private** to ensure there is no anonymous access to the container and blobs.
- Use **Blob** to allow anonymous public read access for blobs only.
- Use **Container** to allow anonymous public read and list access to the entire container, including the blobs.

Note: You can also create the Blob container with PowerShell using the **New-AzStorageContainer** command.

<hr>

## Create blob access tiers
Azure Storage provides different options for accessing block blob data based on usage patterns. By selecting the correct access tier for your needs, you can store your block blob data in the most cost-effective manner.

- **Hot**: Optimized for frequent access of objects in the storage account. New storage accounts are created in the Hot tier by default.
- **Cool**: Optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days. Storing data in the Cool tier is more cost-effective, but accessing that data may be more expensive than accessing the data in the Hot tier.
- **Archive**: Optimized for data that can tolerate several hours of retrieval latency and will remain in the Archive tier for at least 180 days. Most cost-effective option for storing data, but accessing that data is more expensive than accessing data in the Hot or Cool tiers.

Note: When data usage changes, you can switch access tiers at any time.

<hr>

## Add blob lifecycle management rules
Data sets have unique life cycles. Early in the lifecycle, people access some data often. But the need to access drops drastically as the data ages. Some data stays idle and is rarely access once stored. Some data expires days or months after creation, while other data sets are actively read and modified through their lifetimes. Azure Blob storage lifecycle management offers a rich, rule-based policy for GPv2 and Blob storage accounts. Use the policy to transition your data to the appropriate access tiers or expire at the end of the data's lifecycle.

The lifecycle management policy lets you:
- Transition blobs to a cooler storage tier (hot to cool, hot to archive, or cool to archive) to optimize for performance and cost.
- Delete blobs at the end of their lifecycle.
- Define rules to be run once per day at the storage account level.
- Apply rules to containers or a subset of blobs.

By adjusting storage tiers in respect to the age of data, you can design the least expensive storage options for your needs. Lifecycle management policy rules are available to move aging data to cooler tiers.

<hr>

## Determine blob object replication
Object replication asynchronously copies block blobs in a container according to rules that you configure. The contents of the blob, any versions associated with the blob, and the blob's metadata and properties are all copied from the source container to the destination container.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-blob-storage/media/blob-object-replication-21fd3c07.png">

## Scenarios
- **Minimizing latency**: Object replication can reduce latency for read requests by enabling clients to consume data from a region that is in closer physical proximity.
- **Increase efficiency for compute workloads**: Compute workloads can process the same sets of block blobs in different regions.
- **Optimizing data distribution**: You can process or analyze data in a single location and then replicate just the results to other regions.
- **Optimizing costs**: After your data has been replicated, you can reduce costs by moving it to the archive tier using life-cycle management policies.

## Considerations
Object replication:
- requires that blob versioning is enabled on both the source and destination accounts.
- doesn't support blob snapshots. Any snapshots on a blob in the source account are not replicated to the destination account.
- is supported when the source and destination accounts are in the hot or cool tier. The source and destination accounts may be in different tiers.
- when configured, you create a replication policy that specifies the source storage account and the destination account. A replication policy includes one or more rules that specify a source container and a destination container and indicate which block blobs in the source container will be replicated.

<hr>

## Upload blobs
A blob can be any type and size file. Azure Storage offers three types of blobs: *block* blobs, *page* blobs, and *append* blobs. You specify the blob type and access tier when you create the blob.

- **Block blobs (default)** consist of blocks of data assembled to make a blob. Most scenarios using Blob storage employ block blobs. Ideal for storing text and binary data in the cloud, like files, images, and videos.
- **Append blobs** are like block blobs in that they are made up of blocks, but they are optimized for append operations, so they are useful for logging scenarios.
-**Page blobs** can be up to 8 TB in size and are more efficient for frequent read/write operations. Azure virtual machines use page blobs as OS and data disks.

Note: Once the blob has been created, its type cannot be changed.

## Blob upload tools
There are multiple methods to upload data to blob storage, including the following methods:

- **AzCopy** is an command-line tool for Windows and Linux that copies data to and from Blob storage, across containers, or across storage accounts.
- **Azure Storage Data Movement Library** is a .NET library for moving data between Azure Storage services. The AzCopy utility is built with the Data Movement library.
- **Azure Data Factory** supports copying data to and from Blob storage by using the account key, shared access signature, service principal, or managed identities for Azure resources authentications
- **Blobfuse** is a virtual file system driver for Azure Blob storage. You can use blobfuse to access your existing block blob data in your Storage account through the Linux file system.
- **Azure Data Box Disk** is a service for transferring on-premises data to Blob storage when large datasets or network constraints make uploading the data over the wire unrealistic. You can use Azure Data Box Disk to request solid-state disks (SSDs) from Microsoft. You can then copy your data to those disks and ship them back to Microsoft to be uploaded into Blob storage.
- **Azure Import/Export** service provides a way to export large amounts of data from your storage account to hard drives that you provide and that Microsoft then ships back to you with your data.

<hr>

## Determine storage pricing
All storage accounts use a pricing model for blob storage based on the tier of each blob. When using a storage account, the following billing considerations apply:

- **Performance tiers**: As the performance tier gets cooler, the per-gigabyte cost decreases.
- **Data access costs**: Data access charges increase as the tier gets cooler. For data in the cool and archive storage tier, you are charged a per-gigabyte data access charge for reads.
- **Transaction costs**: There is a per-transaction charge for all tiers. The charge increases as the tier gets cooler.
- **Geo-Replication data transfer costs**: Applies only to accounts with geo-replication configured, including GRS and RA-GRS. Geo-replication data transfer incurs a per-gigabyte charge.
- **Outbound data transfer costs**: Data that is transferred out of an Azure region incur billing for bandwidth usage on a per-gigabyte basis. This billing is consistent with general-purpose storage accounts.
- **Changing the storage tier**: Changing from cool to hot incurs a charge equal to reading all the data existing in the storage account. However, changing the account storage tier from hot to cool incurs a charge equal to writing all the data into the cool tier (GPv2 accounts only).

<hr>

## Knowledge check
1. Which of the following best describes Azure blob storage access tiers? -> The administrator can switch between hot and cool performance tiers at any time.
2. Which of these changes between access tiers will happen immediately? -> Hot to Cool (decreasing is immediate)
3. Which of the following best describes blob object replication? -> Object replication doesn't support blob snapshots.

<hr>

## Configure storage security
You will learn how to configure common storage security features like storage access signatures.

## Objectives
- Configured shared access signatures including URI and SAS parameters
- Configure storage service encryption
- Implement customer-managed keys
- Recommend opportunities to improve storage security

## Review storage security strategies
Azure Storage provides a comprehensive set of security capabilities that together enable developers to build secure applications.

- **Encryption**: All data written to Azure Storage is automatically encrypted using Storage Service Encryption (SSE).
- **Authentication**: Azure AD and RBAC are supported for Azure Storage for both resource management operations and data operations as follows:
- You can assign RBAC roles scoped to the storage account to security principals and use Azure AD to authorize resource management operations such as key management.
- Azure AD integration is supported for data operations on the Blob and Queue services.
- **Data in transit**: Data can be secured in transit between an application and Azure by using Client-Side Encryption, HTTPS, or SMB 3.0.
- **Disk encryption**: OS and data disks by Azure VMs can be encrypted using Azure Disk Encryption.
- **Shared Access Signatures**: Delegated access to the data objects in Azure Storage can be granted using Shared Access Signatures.

## Authorization options
Every request made against a secured resource in the Blob, File, Queue, or Table service must be authorized. Authorization ensure that resources in your storage account are accessible only when you want them to be, and only to those users or applications th whom you grant access. Options for authorizing requests to Azure Storage include:

- **Azure Active Directory (Azure AD)**
- **Shared Key**: Shared Key authorization relies on your account access keys and other parameters to produce an encrypted signature string that is passed on the request in the Authorization header.
- **Shared access signatures**: Shared access signatures (SAS) delegate access to a particular resource in your account with specified permissions and over a specified time interval.
- **Anonymous access to containers and blobs**: You can optionally make blob resources public at the container or blob level. A public container or blob is accessible to any user for anonymous read access. Read requests to public containers and blobs do not require authorization.

<hr>

## Create shared access signatures
A shared access signature (SAS) is a URI that grants restricted access rights to Azure Storage resources. You can provide a SAS to clients who shouldn't have access to your storage account key. By distributing a SAS URI to these clients, you grant them access to a resource for a specified period of time. SAS is a secure way to share your storage resources without compromising your account keys.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-storage-security/media/configure-secure-signatures-be02fa89.png">

A SAS gives you granular control over the type of access you grant to clients who have the SAS including:

- An account-level SAS can delegate access to multiple storage services. For example, blob, file, queue, and table.
- An interval over which the SAS is valid, including the start time and the expiry time.
- The permissions granted by the SAS. For example, a SAS for a blob might grant read and write permissions to that blob, but not delete permissions.

Note: SAS provides both **account-level** and **service-level** control. The account-level SAS delegates access to resources in one or more of the storage services. The service-level SAS delegates access to a resource in just one of the storage services.

Optionally, you can also:
- Specify an IP address or range of IP addresses form which Azure Storage will accept the SAS. For example, you might specify a range of IP addresses belonging to your organization.
- Specify the protocol over which Azure Storage will accept the SAS. You can use this optional parameter to restrict access to clients using HTTPS.

Note: A stored access policy can provide another level of control over service-level SAS on the server side. You can group shared access signatures and provide other restrictions by using policy.

## Identify URI and SAS parameters
When you create your SAS, a URI is created using parameters and tokens. The URI consist of your Storage Resource URI and the SAS token.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-storage-security/media/secure-parameters-76db5bda.png">

Below is an example URI:

        https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https &sig=F%6GRVAZ5Cdj2Pw4txxxxx

Each parameter has a specific meaning.

**Name** | **SAS portion** | **Description**
-------- | --------------- | ---------------
Resource URI | https://myaccount.blob.core.windows.net/?restype=service&comp=properties | The Blob service endpoint, with params for getting service properties (when called with GET) or setting service properties (when called with SET)
Storage services version | sv=2015-04-05 | For storage services version 2012-02-12 and later, this param indicates the version to use
Services | ss=bf | The SAS applies to the Blob and File services
Resource types | srt=s | The SAS applies to service-level operations
Start time | st=2015-04-29T22%3A18%3A26Z | Specified in UTC time. If you want the SAS to be valid immediately, omit the start time
Expiry time | se=2015-04-30T02%3A23%3A26Z | Specified in UTC time
Resource | sr=b | The resource is a blob
IP Range sip=168.1.5.60-168.1.5.70 | The range of IP addresses from which a request will be accepted
Protocol | spr=https | Only requests using HTTPS are permitted
Signature | sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B | Used to authenticate access to the blob. 

<hr>

## Determine storage service encryption
Azure **Storage Service Encryption** (SSE) for data at rest protects your data by ensuring your organizational security and compliance commitments are met.

SSE automatically encrypts your data before persisting it to Azure-managed Disks, Azure Blob, Table storage, or Azure Files, and decrypts the data before retrieval.

SSE encryption, encryption at rest, decryption, and key management are transparent to users. All data written to the Azure storage platform is encrypted through 256-bit AES encryption, on the of the strongest block ciphers available.

Note: SSE is enabled for all new and existing storage accounts and cannot be disabled. Because your data is secured by default, you don't need to modify your code or applications.

## Create customer managed keys
The Azure Key Vault can manage your encryption keys. You can create your own an store them in the key vault, or use Key Vault's API to generate encryption keys.

Customer-managed keys give you more flexibility and control. You can create, disable, audit, rotate, and define access controls.

Note: Customer-managed keys can be used with SSE. You can use either a new or existing key vault and key. The storage account and the key vault must be in the same region, but they can be in different subscriptions.

<hr>

## Apply storage security best practices

## Risks
When you use shared access signatures in your apps, you should be aware of two potential risks.
- If a SAS is compromised, it can be used by anyone who obtains it.
- If a SAS provided to a client application expires and the application is unable to retrieve a new SAS from your service, then the application's functionality may be hindered.

## Recommendations
- Always use HTTPS to create or distribute a SAS
- Reference stored access policies where possible
- Use near-term expiration times on an unplanned SAS
- Have client automatically renew the SAS if necessary
- Be careful with SAS start time (if you need access now, start time about 15 minutes in the past)
- Be specific with the resource to be accessed
- Understand that your account will be billed for any usage, including that done with SAS
- Validate data written using a SAS
- Don't assume SAS is always the correct choice
- Use Storage Analytics to monitor your application

<hr>

## Knowledge check
1. A company uses an Azure storage account for storing large numbers of video and audio files. Containers store each type of file and access should be limited to those files. Additionally, the files can only be accessed through shared access signatures. The company wants to be able to revoke access to the files and to change the period for which users can access the files. Which of the following is the easiest way to meet the requirement? -> Implement stored access polices for each container to enable revocation of access or change of duration. 
2. When configuring network access to an Azure Storage Account, what is the default network rule? -> To allow all connections from all networks (dangerous!)
3. The company is planning on delegation model for the Azure storage. Apps in the production environment must have unrestricted access to storage resources. Which of the following is the best course of action? -> Use access keys for the production apps (access keys provide unrestricted access to the storage resources)

<hr>

## Configure Azure files and Azure File Sync
You configure Azure File shares to provide a central location for documents, and Azure File Sync to keep the information up to date across multiple offices.

## Objectives
- Identify when to use Azure files verses Azure Blobs
- Configure Azure file shares with file share snapshots
- Identify features and usage cases of Azure File Sync
- Identify File Sync components and configuration steps

<hr>

## Compare files to blobs
File storage offers shared storage for applications using the industry standard SMB protocol. Microsoft Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can also access file data in the share.

Apps running in Azure VMs or cloud services can mount a file storage share to access file data. This process is similar to how a desktop application would mount a typical SMB share. Any number of Azure VMs or roles can mount and access the File storage share simultaneously.

## Common uses of file storage
- **Replace and supplement**: Azure Files can be used to completely replace or supplement traditional on-premises file servers or NAS devices.
- **Access anywhere**: Popular operating systems such as Windows, macOS, and Linux can directly mount Azure File shares wherever they are in the world.
- **Lift and shift**: Azure Files make it easy to "lift and shift" applications to the cloud that expect a file share to store file applications or user data.
- **Azure File Sync**: Azure File shares can also be replicated with Azure File Sync to Windows Servers, either on-premises or in the cloud, for performance and distributed caching of the data where it's being used.
- **Shared applications**: Storing shared application settings, for example in configuration files.
- **Diagnostic data**: Storing diagnostic data such as logs, metrics, and crash dumps in a shared location.
- **Tools and utilities**: Storing tools and utilities needed for developing or administering Azure virtual machines or cloud services.

## Files and blobs comparison
Sometimes it is difficult to decide when to use file shares instead of blobs or disk shares. Take a minute to review this table that compares the different features.

## Azure Files
- Provides SMB, NFS, client libraries, and a REST interface.
- Use when you want to "lift and shift" an application to the cloud that already uses the native file system APIs to share data between it and other apps running in Azure.

## Azure Blobs
- Provides client libraries and a REST interface that allows unstructured data to be stored and accessed at a massive scale in block blobs.
- Use when you want your application to support streaming and random-access scenarios and you want to be able to access your application data form anywhere.

Other distinguishing features, when selecting Azure files:
- Azure files are true directory objects. Azure blobs are a flat namespace.
- Azure files are accessed through file shares. Azure blobs are accessed through a container.
- Azure files provide shared access across multiple VMs. Azure disks are exclusive to a single virtual machine.

<hr>

## Manage file shares
To access your files, you will need a storage account. After the storage account is created, you can create the file share.

## Mapping file shares (Windows)
You can connect to your Azure file share with Windows or Windows Server. Just select **Connect** from your virtual machine page.

Note: Ensure port 445 is open. Azure Files uses SMB protocol. SMB communicates over TCP port 445. Also, ensure you firewall is not blocking TCP ports 445 from the client machine.

## Mounting file shares (Linux)
You can also connect to your Azure file share with Linux machines. Just select **Connect** form the VM page. Azure file shares can be mounted in Linux distributions using the CIFS kernal client. File mounting cna be done on-demand with the mount command or on-boot (persistent) by creating an entry in /etc/fstab.

## Secure transfer required
The secure transfer option enhances the security of your storage account by only allowing requests to the storage account by secure connection. For example, when calling REST APIs to access your storage accounts, you must connect using HTTPS. Any requests using HTTP will be rejected when *Secure transfer required* is enabled.

<hr>

## Create file share snapshots
Azure Files provides the capability to take share snapshots of file shares. Share snapshots capture a point-in-time, read-only copy of your data.

Share snapshot capability is provided at the file share level. Retrieval is provided at the individual file level, to allow for restoring individual files. You cannot delete a share that has share snapshots unless you delete all the share snapshots first.

Share snapshots are incremental in natures. Only the data that has changed after your most recent share snapshot is saved. Incremental snapshots minimizes the time required to create the share snapshot and saves on storage costs. Even though share snapshots are saved incrementally, you need to retain only the most recent share snapshot in order to restore the share.

## When to use share snapshots
- Protection against application error and data corruption
- Protection against accidental deletions or unintended changes
- General backup purposes

<hr>

## Implement file sync
Use **Azure File Sync** to centralize your organization's file shares in Azure Files, while keeping the flexibility, performance, and compatibility of an on-premises file server. Azure File Sync transforms Windows Server into a quick cache of your Azure file share. You can use any protocol that's available on Windows Server to access your data locally, including SMB, NFS, and FTPS. You can have as many caches as you need across the world.

There are many uses and advantages to file sync:
- Lift and shift
- Branch offices
- Backup and disaster recovery
- File archiving

<hr>

## Identify file sync components

**Storage Sync Service**: Top-level Azure resource for Azure File Sync. A subscription can have multiple Storage Sync Service resources deployed.

**Sync group**: Defines the sync topology for a set of files. Endpoints within a sync group are kept in sync with each other. A Storage Sync Service can host as many sync groups as you need.

**Registered server**: Represents a trust relationship between your server (or cluster) and the Storage Sync Service. You can register as many servers to a Storage Sync Service instance as you want.

**Azure File Sync agent**: A downloadable packaged that enables Windows Server to be synced with an Azure file share. The agent has three main components:
1. FileSyncSvc.exe: Background server that monitors changes on server endpoints and for initiating sync sessions to Azure.
2. StorageSync.sys: File system filter, responsible for tiering files to Azure Files (when cloud tiering is enabled).
3. PowerShell management cmdlets: Interact with the Microsoft.StorageSync Azure resource provider.

**Server endpoint**: represents a specific location on a registered server, such as a folder on a server volume. Multiple server endpoints can exist on the same volume if their namespaces do not overlap.

**Cloud endpoint**: A file share that is part of the sync group. The entire Azure file share syncs, and an Azure file share can be a member of only one cloud endpoint. An Azure file share can be a member of only one sync group. If you add an Azure file share that has an existing set of files as a cloud endpoint to a sync group, the existing files are merged with any other files that are already on other endpoints in the sync group.

## Deploy Azure File Sync
There are several high-level steps for configuring File Sync.

<img width="517" alt="image" src="image.png">

1. **Deploy the Storage Sync Service**: Can be deployed from the Azure portal, you will need to provide Name, Subscription, Resource Group, and Location.
2. **Prepare Windows Server to use with Azure File Sync**
3. **Install the Azure File Sync**
4. **Register Windows Server with Storage Sync Service**

<hr>

## Cloud tiering overview
An optional feature of Azure File Sync, decreases the amount of local storage required while keeping the performance of an on-premises file server.

When enabled, this features stores only frequently accessed (hot) files on your local server. Infrequently accessed (cool) files are split into namespace (file and folder structure) and file content. The namespace is stored locally and the file content stored in an Azure file share in the cloud.

## How cloud tiering works
when you enable cloud tiering, there are two policies that you can set to inform Azure File Sync when to tier cool files, the **volume free space policy** and the **date policy**.

The **volume free space policy** tells Azure File Sync to tier cool files to the cloud when a certain amount of space is taken up on your local disk.

The **date policy** tiers cool files to the cloud if they haven't been accessed for x number of days.

<hr>

## Knowledge check
1. Which of the following is correct about cloud tiering? -> Archives infrequently accessed files to free up space on the local file share.
2. A local manufacturing company runs dedicated software in their warehouse to keep track of stock. The software needs to run on machines in the warehouse, but the management team want to access the output from the main office. The limited bandwidth available in the warehouse has caused problems in the past when they tried to use cloud-based solutions. What is the best way to sync these files with the cloud? -> Use a machine in the warehouse to host a file share, install Azure File Sync, and share a drive with the rest of the warehouse.
3. Which of the following best describes the Azure File Sync agent? -> It's installed on a server to enable Azure File Sync replication between the local file share and an Azure file share.

<hr>

## Summary
Azure Files offers fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol or Network File System (NFS) protocol. Azure File Sync is a service that allows you to cache several Azure file shares on an on-premises Windows Server or cloud VM.

<hr>

## Configure storage with tools
You will learn how to configure storage with tools like Storage Explorer and AZCopy.

## Objectives
- Configure and use Storage Explorer
- Configure the Import and Export service
- Configure and use AZCopy 

<hr>

## Use Azure Storage Explorer
Azure Storage Explorer is a standalone app that makes it easy to work with Azure Storage data on Windows, macOS, and Linux. With Storage Explorer, you can access multiple accounts and subscriptions and manage all your storage content.

Requires both management (Azure Resource Manager) and data layer permissions, which means you need Azure Active Directory permissions, which give you access to your storage account, the containers in the account, and the data in the containers.

## Connecting to storage
- Connect to storage accounts associated with your Azure subscriptions.
- Connect to storage accounts and services that are shared from other Azure subscriptions.
- Connect to and manage local storage by using the Azure Storage Emulator.

In addition you can work with storage accounts in global and national Azure:
- **Connect to an Azure subscription**: Manage storage resources that belong to you Azure subscription.
- **Work with local development storage**: Manage local storage by using the Azure Storage Emulator.
- **Attach to external storage**: Manage storage resources that belong to another Azure subscription or that are under national Azure clouds by using the storage account's name, key, and endpoints.
- **Attach a storage account by using a SAS**: Manage storage resources that belong to another Azure subscription by using a shared access signature (SAS).
- **Attach a service by using a SAS**: Manage a specific storage service (blob container, queue, or table) that belongs to another Azure subscription by using a SAS.

## Accessing external storage accounts
Storage Explorer lets you attach to external storage accounts so that storage accounts can be easily shared. To create the connection you will need the storage **Account name** and **Account key**. In the portal, the account key is called **key1**.

To use a name and key form a national cloud, use the **Storage endpoints domain** drop-down to select **Other** and then enter the custom storage endpoint domain.

Note: Access keys provide access to the entire storage account. Store your access keys securely. We recommend regenerating your access keys regularly. You are provided tow access keys so that you can maintain connections using one while regenerating the other.

When you regenerate your access keys, you must update any Azure resources and applications that access this storage account to use the new keys. This action will not interrupt access to disks from your virtual machines.

<hr>

## Use the import and export service
Used to securely import large amounts of data to Azure Blob storage and Azure Files by shipping disk drives to an Azure datacenter. This service can also be used to transfer data from Azure Blob storage to disk drives and ship to your on-premises sites. You must supply your own disk drives and transfer the data yourself.

## Usage cases
Consider using Azure Import/Export service when uploading or downloading data over the network is too slow or getting more network bandwidth is cost-prohibitive. Situations such as:

- Migrating data to the cloud
- Content distribution
- Backup
- Data recovery

## Import jobs
An Import job securely transfers large amounts of data to Azure Blob storage (block and page blobs) and Azure Files by shipping disk drives to an Azure datacenter. In this case, you will be shipping hard drives containing your data.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-storage-tools/media/import-jobs-3dd387ae.png">

In order to perform an import, follow these steps:
1. Create an Azure Storage account.
2. Identify the numbers of disks that you will need to accommodate all the data that you want to transfer.
3. Identify a computer that you will use to perform the data copy, attach physical disks that you will ship to the target Azure datacenter, and install the WAImportExport tool.
4. Run the WAImportExport tool to copy the data, encrypt the drive with BitLocker, and generate journal files.
5. Use the Azure portal to create an import job referencing the Azure Storage account. As part of the job definition, specify the destination address representing the Azure region where the Azure Storage account resides.
6. Ship the disks to the destination that you specified when creating the import job and update the job by providing the shipment tracking number.
7. Once the disks arrive at the destination, the Azure datacenter staff will carry out data copy to the target Azure Storage account and ship the disks back to you.

## Export jobs
Export jobs transfer data from Azure storage to hard disk drives and ship to your on-premises sites.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-storage-tools/media/export-jobs-850746e1.png">

In order to perform the export, follow these steps:
1. Identify the data in the Azure Storage blobs that you intent to export.
2. Identify the number of desks you need to transfer the data
3. Use the Azure Portal to create an export job referencing the Azure Storage account. Specify the blobs you want to export, the return address, and your carrier account number. Microsoft will ship your disks back to you after the export process is complete.
4. Ship the required number of disks to the Azure region hosting the storage account. Update the job by providing the shipment tracking number.
5. Once the disks arrive, staff will carry out data copy from the storage account to the disks and encrypt the volumes using BitLocker and ship them back to you. The BitLocker keys will be available in the Azure portal.

## Import/Export Tool (WAImportExport)
The **Azure Import/Export Tool** is the drive preparation and repair tool that you can use with the Microsoft Azure Import/Export service. You can use the tool for the following functions:
- Before creating an import job, you can use this tool to copy data to the hard drives you are going to ship.
- After an import job has completed, you can use this tool to repair any blobs that were corrupted, were missing, or conflicted with other blobs.
- After you receive the drives from a completed export job, you can use this tool to repair any files that were corrupted or missing on the drives.

Import/Export service requires the use of internal SATA II/III HDDs or SSDs. EAch dist contains a single NTFS volume that you encrypt with BitLocker when preparing a drive. To prepare a drive, you must connect it to a computer running a 64-bit version of the Windows client or server operating system to run the WAImportExport tool from that computer. The WAImportExport tool handles data copy, volume encryption, and creation of journal files. Journal files are necessary to create an import/export job and help ensure the integrity of the data transfer.

Note: You can create jobs directly from the Azure portal or you can accomplish this programmatically by using Azure Storage Import/Export REST API.

<hr>

## Use AzCopy
An alternative method for transferring data is **AzCopy**. AzCopy v10 is the next-generation command-line utility for copying data to/from Microsoft Azure Blob and File storage, which offers a redesigned command-line interface and new architecture for high-performance reliable data transfers. Using AzCopy, you can copy data between a file system and a storage account, or between storage accounts.

## New features
Synchronize a file system to Azure Blob or vice versa. Idea for incremental copy scenarios.

- Supports Azure Data Lake Storage Gen2 APIs.
- Supports copying an entire account (Blob service only) to another account.
- Account to account copy is now using the new Put from URL APIs. No data transfer to the client is needed which makes the transfer faster.
- List/Remove files and blobs in a given path.
- Supports wildcard patterns in a path, --include flags, and --exclude flags.
- Improved resiliency: every AzCopy instance will create a job order and a related log file. You can view and restart previous jobs and resume failed jobs. AzCopy will also automatically retry a transfer after a failure.
- General performance improvements.

## Authentication options
- **Azure AD**: (Supported for Blob and ADLS Gen2 services). Use .\azcopy login to sign in using Azure Active Directory. The user should have *Storage Blob Data Contributor* role assigned to write to Blob storage using Azure Active Directory authentication.
- **SAS tokens**: (Supported for Blob and File services). Append the SAS token to the blob path on the command line to use it.

## Getting started
AzCopy has a simple self-documented syntax. Here's how you can get a list of available commands:

        azcopy --help

The basic syntax for AzCopy commands is:

        azcopy copy [source] [destination] [flags]

Note: AzCopy is available on Windows, Linux, and macOS.

<hr>

## Knowledge check
1. A manufacturing company's finance department wants to control how the data is being transferred to Azure Files. They want a graphical tool to manage the process, but they don't want to use the Azure portal. What tool would be best in this situation? -> Azure Storage Explorer
2. There is an existing storage account in Azure that stores unstructured data. The organization creates a new storage account. What is the best way to move the existing data to the new storage account? -> Use the AzCopy command-line tool.
3. The finance team needs to transfer a series of large files to blob storage. It may take several hours to upload each file. Everyone is concerned if the transfer fails, it will have to be restarted. Which tool is the most appropriate for this task? -> AzCopy

<hr>

## Create an Azure storage account
Create an Azure Storage account with the correct options for your business needs.

## Objectives
- Decide how many storage accounts you need for your project
- Determine the appropriate settings for each storage account
- Create a storage account using the Azure portal

<hr>

## What is Azure Storage?
*Azure Storage* includes Azure Blobs, Azure Files, Azure Queues, and Azure Tables. These four data services are all primitive, cloud-based storage services, and are often used together in the same application.

## What is a storage account?
A *storage account* is a container that groups a set of Azure Storage services together. Only data services from Azure Storage can be included in a storage account (Azure Blobs, Azure Files, Azure Queues, and Azure Tables). 

Combining data services into a single storage account enables you to manage them as a group. The settings you specify when you create the account, or any changes that you make after creation, apply to all services in the storage account. Deleting a storage account deletes all the data stored inside.

A storage account is an Azure resource and is part of a resource group.

Other Azure data services, such as Azure SQL and Azure Cosmos DB, are managed as independent Azure resources and cannot be included in a storage account.

## Storage account settings
A storage account defines a policy that applies to all the storage services in the account. For example, you could specify that all the contained services will be stored in the West US datacenter, accessible only over HTTPS, and billed to the sales department subscription. 

The setting that are defined by a storage account are:

- **Subscription**: The Azure Subscription that will be billed for the services in the account.

- **Location**: The datacenter that will store the services in the account.

- **Performance**: Determines the data services you can have in your storage account and the type of hardware disks used to store the data

- Standard allows you to have any data service (Blob, File, Queue, Table) and uses magnetic disk drives.
- Premium provides mores services for storing data and uses sold0state drives (SSD) for storage.

- **Replication**: Determines the strategy used to make copies of your data to protect against hardware failure or natural disaster. At a minimum, Azure automatically maintains three copies of your data within the datacenter associated with the storage account. A minimum replication is called locally redundant storage (LRS), and guards against hardware failure but does not protect you from an event that incapacitates the entire datacenter. You can upgrade to one of the other options such as geo-redundant storage (GRS) to get replication at different datacenters across the world.

- **Access tier**: Controls how quickly you will be able to access the blobs in a storage account.

- **Secure transfer required**: A security feature that determines the supported protocols for access. Enabled requires HTTPS, while disables allows HTTP.

- **Virtual networks**: A security feature that allows inbound access requests only from the virtual network(s) you specify.

## How many storage accounts do you need?
A storage account represents a collection of settings like location, replication strategy, and subscription owner. You need one storage account for each group of settings that you want to apply to your data. The number of storage accounts you need is typically determined by your data diversity, cost sensitivity, and tolerance for management overhead.

## Data diversity
Organizations often generate data that differs in where it is consumed, how sensitive it is, which group pays the bills, etc. Diversity along any of these vectors can lead to multiple storage accounts. Let's consider two examples:

1. Do you have data that is specific to a country or region? If so, you might want to store the data in a datacenter in that region or country for performance and compliance reasons. You will need one storage account for each geographical region.

2. Do you have some data that is proprietary and some for public consumption? If so, you could enable virtual networks for the proprietary data and not for the public data. Separating proprietary data and public data will also require separate storage accounts.

In general, increased diversity means an increased number of storage accounts.

## Cost sensitivity
A storage account by itself has no financial cost; however, the settings you choose for the account do influence the cost of services in the account. Geo-redundant storage costs more than locally redundant storage. Premium performance and the Hot access tier increase the cost of blobs.

You can use multiple storage accounts to reduce costs. For example, you could partition you data into critical and non-critical categories. You could place your critical data into a storage account with geo-redundant storage and put your non-critical data in a different storage account with locally redundant storage.

A typical strategy is to start with an analysis of your data and create partitions that share characteristics like location, billing, and replication strategy, and then create one storage account for each partition.

## Choose your account settings
There are three settings that apply to the account itself, rather than to the data stored in the account:

- Name
- Deployment model
- Account kind

These settings impact how you manage your account and the cost of the services within it.

## Name
Each storage account has a name. The name must be globally unique within Azure, use only lowercase letters and digits and be between 3 and 24 characters.

## Deployment model
A *deployment model* is the system Azure uses to organize your resources. The model defines the API that you use to create, configure, and manage those resources. Azure provides two deployment models:

- **Resource Manager**: the current model that uses the Azure Resource Manager API
- **Classic**: a legacy offering that uses the Azure Service Management API

Most Azure resources only work with Resource Manager, and makes it easy to decide with model to choose. However, storage accounts, virtual machines, and virtual networks support both, so you must choose one or the other when you create your storage account.

The key feature difference between the two models is their support for grouping. The Resource Manager model adds the concept of a *resource group*, which is not available in the classic model. A resource group lets you deploy and manage a collection of resources as a single unit.

Microsoft recommends that you use *Resource Manager* for all new resources.

## Account kind
Storage account *kind* is a set of policies that determine which services you can include in the account and the pricing of those services. There are three kinds of storage accounts:

- **StorageV2 (general purpose v2)**: the current offering that supports all storage types and all of the latest features
- **Storage (general purpose v1)**: a legacy kind that supports all storage types but may not support all features
- **Blob storage**: a legacy kind that allows only block blobs and append blobs

Microsoft recommends that you use the **General-purpose v2** option for new storage accounts.

There are a few special cases that can be exceptions to this rule. For example, pricing for transactions is lower in general purpose v1, which would allow you to slightly reduce costs if that matches your typical workload.

The core advice here is to choose the **Resource Manager** deployment model and the **StorageV2(general purpose v2)** account kind for all your storage accounts.

## Choose an account creation tool
There are several tools that create a storage account. You choice is typically based on if you want a GUI and whether you need automation.

## Available tools
- Azure Portal
- Azure CLI
- Azure PowerShell
- Management client libraries

<hr>

## Knowledge check
1. Suppose you have two video files stored as blobs. One of the videos is business-critical and requires a replication policy that creates multiple copies across geographically diverse datacenters. The other video is non-critical, and local replication policy is sufficient. Which of the following options would satisfy both data diversity and cost sensitivity consideration? -> Create two storage accounts. The first account makes use of Geo-redundant storage (GRS) and hosts the business-critical video content. The second accounts makes use of Local-redundant storage (LRS) and hosts the non-critical video content.
2. The name of a storage account must be: -> Globally unique.
3. In a typical project, when would you create your storage account(s)? -> At the beginning, during project setup.

<hr>

## Control access to Azure Storage with shared access signatures
Grant access to data stored in your Azure Storage accounts securely through the use of shared access signatures.

## Objectives
- Identify the features of a shared access signature for Azure Storage.
- Identify the features of stored access policies.
- Programmatically generate and use a shared access signature to access storage.

<hr>

## Access Azure Storage
Files stored in Azure Storage are accessed by clients over HTTP/HTTPS. Azure checks each client request for authorization to access stored data. Four options are available for blob storage:
- Public access
- Azure AD
- Shared key
- Shared access signature (SAS)

## Public access
Public access is also known as anonymous public read access for containers and blobs.

There are two separate settings that affect public access:
- **The Storage Account**: Configure the storage account to allow public access by setting the *AllowBlobPublicAccess* property. When set to true, Blob data is available for public access only if the container's public access setting is also set.
- **The Container**: You can enable anonymous access only if anonymous access has been allowed for the storage account. A container has two possible settings for public access: *Public read access for blobs*, or *public read access for a container and its blobs*. Anonymous access is controlled at the container level, not for individual blobs. This means that if you want to secure some of the files, you need to put them in a separate container that doesn't permit public read access.

Both storage account and container settings are required to enable anonymous public access. The advantages of this approach are that you don't need to share keys with clients who need access to your files. You also don't need to manage a SAS.

## Azure AD
Use the Azure AD option to securely access Azure Storage without storing any credentials in your code. AD authorization takes a two-step approach. First, you authenticate a security principal that returns an OAuth 2.0 token if successful. This token is then passed to Azure Storage to enable authorization to the requested resource.

Use this form of authentication if you're running an app with managed identities or using security principals.

## Shared key
Azure Storage creates two 512-bit access keys for every storage account that's created. You share these keys to grant clients access to the storage account. These keys grant anyone with access to the equivalent of root access to your storage.

We recommend that you manage storage keys with Azure Key Vault because it's easy to rotate keys on a regular schedule to keep your storage account secure.

## Shared access signature
A SAS lets you grant granular access to files in Azure Storage, such as read-only or read-write access, expiration time, after which the SAS no longer enables the client to access the chosen resources. A shared access signature is a key that grants permission to a storage resource, and should be protected in the same manner as an account key.

Azure Storage supports three types of shared access signatures:
- **User delegation SAS**: Can only be used for Blob storage and is secured with Azure AD credentials.
- **Service SAS**: A service SAS is secured using a storage account key. A service SAS delegates access to a resource in any one of four Azure Storage services: Blob, Queue, Table, or File.
- **Account SAS**: An account SAS is secured with a storage account key. An account SAS has the same controls as a service SAS, but can also control access to service-level operations, such as Get Service Stats.

You can create a SAS ad-hoc by specifying all the options you need to control, including start time, expiration time, and permissions.

If you plan to create a service SAS, there's also an option to associate it with a stored access policy. A stored access policy can be associated with up to five active SASs. You can control access and expiration at the stored access policy level. This is a good approach if you need to have granular control to change the expiration, or revoke a SAS. The only way to revoke or change an ad-hoc SAS is to change the storage account keys.

## Check your knowledge
1. Your organization has an internal system to share patient appointment information and notes. You can secure the access based on a user's membership in an Azure Active Directory (Azure AD) group. Which kind of authorization support this scenario best, and why? -> Use Azure Active Directory. By using Azure AD, you can create a service principal to authenticate your app.
2. Your public-facing static websites stores all its public UI images in blob storage. The website needs to display the graphics without any kind of authorization. Which is the best option? -> Public access.

<hr>

## Use shared access signatures to delegate access to Azure Storage
By using a shared access signature (SAS) you can delegate access to your resources. Clients don't have direct access to your storage account credentials and, at a granular level, you control what they access.

## How shared access signatures work
A SAS has two components, a URI that points to one or more storage resources, and a token that indicates how the resources may be accessed by the client.

In a single URI, such as `https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D`, you can separate the UIR from the SAS token as follows:

URI | SAS token
--- | ---
`https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?` | `sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D`

## Create a SAS in .NET
The following steps show how to create a SAS in .NET that doesn't require Azure AD.

## Create a blob container to connect to the storage account on Azure
        BlobContainerClient container = new BlobContainerClient("ConnectionString", "Container");

## Retrieve the blob you want to create a SAS token for and create a BlobClient
        foreach (BlobItem blobItem in container.GetBlobs()) 
        {
                BlobClient bob = container.GetBlobClient(blobItem.Name);        
        }

## Create a BlobSasBuilder object for the blob you use to generate the SAS token
        BlobSasBuilder sas = new BlobSasBuilder
        {
                BlobContainerName = blob.BlobContainerName,
                BlobName = blob.Name,
                Resource = "b",
                ExpiresOn = DateTimeOffset.UtcNow.AddMinutes(1)
        };

        // Allow read access
        sas.SetPermissions(BlobSasPermissions.Read);

## Authenticate a call to the ToSasQueryParameters method of the BlobSasBuilder object
        StorageSharedKeyCredential storageSharedKeyCredential = new StorageSharedKeyCredential( "AccountName", "AccountKey");

        sasToken = sas.ToSasQueryParameters(storageSharedKeyCredential).ToString();

## Best Practices
- To securely distribute a SAS and prevent man-in-the-middle attacks, always use HTTPS.
- The most secure SAS is user delegation. Use it wherever possible because it removes the need to store your storage account key in code. Azure AD must be used to manage credentials; this option might not be possible for your solution.
- Try to set your expiration time to the smallest useful value. If a SAS key becomes compromised, it can be exploited for only a short time.
- Apply the rule of minimum-required privileges. Only grant the access that's required. For example, in your app, read-only access is sufficient.
- There are some situations where a SAS isn't the correct solution. When there's an unacceptable risk of using a SAS, create a middle-tier service to manage users and their access to storage.

The most flexible and secure way to use a service or account SAS is to associate the sAS tokens with a stored access policy.

<hr>

## What are stored access policies?
You can create a stored access policy on four kinds of storage resources:
- Blob containers
- File shares
- Queues
- Tables

Teh stored access policy you create a blob container can be used for all the blobs in the container and or the container itself. A stored access policy is created with the following properties:
- **Identifier**: The name you use to reference the stored access policy.
- **Start time**: A DateTimeOffset value for the date and time when the policy might start to be used. The value can be null.
- **Expiry time**: A DateTimeOffset value for the date and time when the policy expires. After this time, requests to the storage will fail with a 403 error-code message.
- **Permissions**: the list of permissions as a string that can be one or all of **acdlrw**.

## Create stored access policies
You can create a stored access policy with C# code by using the Azure portal or Azure CLI commands.

## With C# .NET code
        BlobSignedIdentifier identifier = new BlobSignedIdentifier
        {
                Id = "stored access policy identifier",
                AccessPolicy = new BlobAccessPolicy
                {
                        ExpiresOn = DateTimeOffset.UtcNow.AddHours(1),
                        Permissions = "rw
                }
        };

        blobContainer.SetAccessPolicy(permissions: new BlobSignedIdentifier[] { identifier });

## With the portal
In the portal, go to the storage account and then go to the blob storage container. On the left, select **Access policy**. To add a new stored access policy, select **+ Add policy**.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/control-access-to-azure-storage-with-sas/media/5-add-a-policy.png">

## With Azure CLI commands
        az storage container policy create \
                --name <stored access policy identifier> \
                --container-name <container name> \
                --start <start time UTC datetime> \
                --expiry <expiry time UTC datetime> \
                --permissions <(a)dd, (c)reate, (d)elete, (l)ist, (r)ead, or (w)rite> \
                --account-key <storage account key> \
                --account-name <storage account name> \

<hr>

## Upload, download, and manage data in Azure Storage Explorer
Azure Storage Explorer allows you to quickly view all the storage services under your account. You can browse through, read, and edit data stored in those services through a user-friendly graphical interface.

## Objectives
- Describe the features on Azure Storage Explorer
- Install Storage Explorer
- Use Storage Explorer to connect to Azure Storage services and manipulate stored data

## Connect Azure Storage Explorer to a storage account
Storage accounts provide a flexible solution that keeps data as files, tables, and messages. With Azure Storage Explorer, it's easy to read and manipulate this data.

## What is Storage Explorer?
Storage Explorer is a GUI application developed by Microsoft to simplify access to, and the management of, data stored in azure storage accounts. Storage Explorer is available on Windows, macOS, and Linux.

Some of the benefits of using Storage Explorer are:
- It's easy to connect to and manage multiple storage accounts.
- The interface lets you connect to Data Lake Storage.
- You can also use the interface to update and view entities in your storage accounts.
- Storage Explorer is free to download and use.

With Storage Explorer, you can use a range of storage and data operation tasks on any of your Azure storage accounts. These tasks include edit, download, copy, and delete.

## Azure Storage types
- **Azure Blob Storage**
- **Azure Table Storage**
- **Azure Queue Storage**
- **Azure Files**
- **Azure Data Lake Storage**: Azure Data Lake, based on Apache Hadoop, is designed for large data volumes and can store unstructured and structured data. Azure Data Lake Storage Gen1 is a dedicated service. Azure Data Lake Storage Gen2 is Azure Blob Storage with the hierarchical namespace feature enabled on the account.

## Manage multiple storage accounts in multiple subscriptions
Storage Explorer gives you the ability to manage the data stored in multiple Azure storage accounts and across Azure subscriptions.

## Use local emulators
Storage Explorer supports two emulators: Azure Storage Emulator and Azurite.
- Azure Storage Emulator uses a local instance of Microsoft SQL Server 2012 Express LocalDB. It emulates Azure Table, Queue, and Blob storage.
- Azurite, which is based on Node.js, is an open-source emulator that supports most Azure Storage commands through an API.

## Connecting Storage Explorer to Azure
There are several ways to connect your Storage Explorer application to your Azure storage accounts.

You need two permissions to access your Azure storage account: management and data. However, you can use Storage Explorer with the data-layer permission. The data layer requires the user to be granted, at a minimum, a read data role. The natures of the read/write role should be specific to the type of data stored in the storage account. The data layer is used to access blobs, containers, and other data resources.

## Connection types
There are many ways to connect an Azure Storage Explorer instance to your Azure resources. For example:
- Add resources by using Azure AD
- Use a connection string
- Use a shared access signature URI
- Use a name and key
- Attach to a local emulator
- Attach to Azure Data Lake Storage by using a URI

## Add an Azure account by using Azure AD
Use this connection type when the user can access the data layer. You can use it only to create an Azure Data Lake blob container or a standard blob container. Connecting to Azure Storage through Azure AD requires more configuration than the other methods. The account that you use to connect to Azure must have the correct permissions and authorization to access the target resources.

## Connect by using a shared access signature URI
You'll need a SAS URI whether you want to use a file share, table, queue, or blob container. You can get a SAS URI either form the Azure portal or from Storage Explorer.

## Connect by using a storage account name and key
To find the storage access keys from the Azure portal, go the correct storage account page and select **access keys**.

## Manage Data Lake Storage Gen1
You can use Storage Explorer to access and manage data stored in Data Lake Storage Gen1.

<hr>

## Configure storage accounts
You will learn to configure storage accounts including replication and endpoints.

## Configure a custom domain
There are two ways to configure this service: Direct CNAME mapping and an intermediary domain.

Note: Azure Storage does not yet natively support HTTPS with custom domains. You can currently use Azure CDN to access blobs by using custom domains over HTTPS.

## Secure storage endpoints
For accessing a storage account, you would use the **Firewalls and virtual networks** blade to add the virtual networks tha will have access. 

- Firewalls and Virtual Networks restricts access to the Storage Account from specific Subnets on Virtual Networks or public IPs.
- Subnets and Virtual Networks must exist in the same Azure Region or Region Pair as the Storage Account.

<hr>

## Knowledge check
1. Which of the following replicates data to a secondary region, maintains six copies of the data, and is the default replication option? -> Read-access geo-redundant storage.
2. The name of a storage account must be which of the following? -> Globally unique.
3. A manufacturing company has sensors that record time-relative data. Only the most recent data is useful. The company wants the lowest cost storage for this data. What is the best storage account for the company? -> Locally redundant storage.

<hr>

## Configure virtual machine availability
You will learn how to configure virtual machine availability including vertical and horizontal scaling.

## Objectives
- Implement availability sets and availability zones.
- Implement update and fault domains.
- Implement virtual machine scale sets.
- Autoscale virtual machines.

<hr>

## Plan for maintenance and downtime
Three scenarios can lead to your VM being impacted:
1. Unplanned hardware maintenance
2. Unexpected downtime
3. Planned maintenance

<hr>

## Setup availability sets
An **Availability Set** is a logical feature used to ensure that a group of related VMs are deployed so that they aren't all subject to a single point of failure and not all upgraded at the same time during a host operating system upgrade in the datacenter. VMs place in an availability set should perform an identical set of functionalities and have the same software installed.

Keep these general principals in mind.

- For redundancy, configure multiple VMs in an Availability Set.
- Configure each application tier into separate Availability Sets.
- Combine a Load Balancer with Availability Sets.
- Use managed disks with the virtual machines.

You create Availability Sets through the Azure portal in the disaster recovery section. Also, you can build Availability Sets using Resource Manager templates, scripting, or API tools.

## Service Level Agreements (SLAs)
- For all VMs that have two or more instances deployed across two or more Availability Zones in the same Azure region, Microsoft guarantees you will have VM connectivity to at least one instance at least 99.99% of the time.
- For all VMs that have two or more instances deployed in the same Availability SEt, you will have connectivity to at least one instance at least 99.95% of the time.
- For any Single Instance Virtual Machine using premium storage for all Operating System Disks and Data Disks, Microsoft guarantees VM connectivity at least 99.9%.

<hr>

## Review update and fault domains
Update Domains and Fault Domains help Azure maintain high availability and fault tolerance when deploying and upgrading applications. Each virtual machine in an availability set is placed in one update domain and one fault domain.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-machine-availability/media/update-fault-domains-c1ceee00.png">

## Update domains
An **update domain (UD)** is a group of nodes that are upgraded together during the process of a service upgrade (rollout). An update domain allows Azure to perform incremental or rolling upgrades across a deployment. Each update domain contains a set of VMs and associated physical hardware that can be updated and rebooted at the same time. During planned maintenance, only one update domain is rebooted at a time. By default, there are five (non-user-configurable) update domains, but you configure up to 20 update domains.

## Fault domains
A **fault domain (FD)** is a group of nodes that represent a physical unit of failure. A fault domain defines a group of virtual machines that share a common set of hardware, switches, that share a single point of failure. Two fault domains mitigate against hardware failures, network outages, power interruptions, or software updates. Think of a fault domain as nodes belonging to the same physical rack.

<hr>

## Review availability zones
Availability Zones is a high-availability offering that protects your applications and ata from datacenter failures.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-machine-availability/media/availability-zones-26abc45c.png">

## Considerations
- Availability Zones are unique physical locations within an Azure region.
- Each zone is made up of one or more datacenters equipped with independent power, cooling, and networking.
- To ensure resiliency, there's a minimum of three separate zones in all enabled regions.
- The physical separation of Availability Zones within a region protects applications and data from datacenter failures.
- Zone-redundant services replicate your applications and data across Availability Zones to protect from single-points-of-failure.
- With Availability Zones, Azure offers industry best 99.99% VM update SLA.

## Implementation
An Availability Zone in an Azure region is a combination of fault domain and an update domain. For example, if you create three or more VMs across three zones in an Azure region, your VMs are effectively distributed across three fault domains and three update domains. The Azure platform recognizes this distribution across update domains to make sure that VMs in different zones are not updated at the same time. Build high-availability into your application architecture by colocating your compute, storage, networking, and data resources within a zone and replicating in other zones.

Azure services that support Availability Zones fall into two categories:
- **Zonal services**: Pins the resource to a specific zone (for example, virtual machines, managed disks, Standard IP addresses).
- **Zone-redundant services**: Platform replicates automatically across zones (for example, zone-redundant storage, SQL Database).

Note: To achieve comprehensive business continuity on Azure, build your application architecture using the combination of Availability Zones with Azure region pairs.

## Implement scale sets
VM scale sets are an Azure Compute resource you can use to deploy and manage a set of **identical** VMs. With all VMs configured the same, VM scale sets are designed to support true autoscale. No pre-provisioning of VMs is required. It is easier to build large-scale services targeting big computer, big data, and containerized workloads. As demand goes up more VM instances can be added. As demand goes down VM instances can be removed. The process can be manual or automated or a combination of both.

## Scale set benefits
- All VM instances are created from the same base OS image and config. This approach lets you easily manage hundreds of VMs without additional configuration tasks or network management.
- Scale sets support the use of the Azure load balancer for basic layer-4 traffic distribution, and Azure Application Gateway for more advanced layer-7 traffic distribution and SSL termination.
- Used to run multiple instances of your application. If one of these VM instances has a problem, customers continue to access your application through one of the other instances with minimal interruption.
- Supports up to 1,000 VM instances. If you create and upload your own custom VM images, the limit is 600 VM instances.

## Create scale sets
When you create a scale set, consider these parameters.
- **Initial instance count**
- **Instance size**
- **Azure spot instance**: Low-priority VMs are allocated from Azure's excess compute capacity. Spot instances enable sereral types of workloads to run at a reduced cost.
- **Enable scaling beyond 100 instances**: If set to No, the scale set will be limited to one placement group with a max capacity of 100. If yest, the scale set can span multiple placement groups. This allows for capacity to be up to 1,000 but changes the availability characteristics of the scale set.
- **Spreading algorithm**: We recommend deploying with max spreading for most workloads. This approach provides the best spreading.

<hr>

## Implement autoscale
An Azure VM scale set can automatically increase or decrease the number of VM instances that run your application. This means you can dynamically scale to meet changing demand.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-machine-availability/media/autoscale-45b054e0.png">

## Autoscale benefits
- **Automatically adjust capacity**: Lets your create rules for when to scale and by how much.
- **Scale out**
- **Scale in**
- **Schedule events**
- **Less overhead**

## Configure autoscale
When you create a scale set you can enable Autoscale. You should also define a minimum, maximum, and default number of VM instances.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-machine-availability/media/implement-autoscale-74d25345.png">

<hr>

## Knowledge check
1. Another admin creates an Azure VM scale set with 5 VMs. Later, alerts who the VMs are all running at max capacity with the CPU being fully consumed. However, more VMs are not deploying in the scale set. What does be done to ensure additional VMs are deployed with the CPU is 75% consumed? -> Enable auto scale option.
2. The DevOps team for a large food delivery company is configuring a VM scale set. Friday night is typically the busiest time. Conversely, 7 AM on Wednesday is generally the quietest time. Which of the following VM scale set features should be configured to add more machines during that time? -> Schedule-based rules.
3. VM scale sets deploy and manage what? -> A set of identical VM instances created from the same base OS image and configuration.

<hr>

## Configure VM extensions
You will learn how to use VM extensions to automate virtual machine deployments.

## Objectives
- Identify features and usage cases for machine extensions
- Identify features and usage cases for custom script extensions
- Identify features and usage cases for desired state configuration

<hr>

## Implement VM extensions
A VM **extension** is used to automate tasks like creating, maintaining, and removing VMs. They are small applications that provide post-deployment configuration and automation tasks on Azure VMs. For example, if a VM requires software installation, anti-virus protection, or a configuration script inside, a VM extension can be used. Extensions are all about managing your VMs.

Azure VM extensions can be:
- Managed with Azure CLI, PowerShell, ARM templates, and the Azure portal.
- Bundled with a new VM deployment or run against any existing system. For example, they can be part of a larger deployment, configuring applications on VM provision, or run against any supported extension OS post deployment.

<hr>

## Implement custom script extensions
A Customer Script Extension (CSE) can be used to automatically launch and execute VM customization tasks post configuration. Your script extension may perform simple tasks such as stopping the VM or installing a software component.

You can install the CSE form the Azure portal by accessing the VM's **Extensions** blade. Once the CSE resource is created, you will provide a PowerShell script file. The file will include PowerShell commands you want to execute on the VM. You can also pass arguments. After the file is uploaded it executes immediately. Scripts can be downloaded from Azure storage on GitHub, or provided to the Azure portal at extension run time.

You could also use the PowerShell **Set-AzVmCustomScriptExtension** command. This command requires the URI for the script in the blob container:

        Set-AzVmCustomScriptExtension -FileUri https://scriptstore.blob.core.windows.net/scripts/Install_IIS.ps1 -Run "PowerShell.exe" -VmName vmName -ResourceGroupName resourceGroup -Location "location"

## Considerations
- **Timeout**: CSEs have 90 minutes to run. If that is exceeded, it is marked as a timeout.
- **Dependencies**: Make sure all required content is available (network or storage access).
- **Failure events**: Be sure to account for any errors that might occur when running your script (disk space, security, permissions).
- **Sensitive data**: Protect any sensitive data.

<hr>

## Implement desired state configuration
Desired State Configuration (DSC) is a management platform in Windows PowerShell. DSC enables deploying and managing configuration data for software services and managing the environment in which these services run. DSC provides a set of Windows PowerShell language extensions, cmdlets, and resources that you can use to declaratively specify how you want your software environment to be configured. DSC also provides a means to maintain and manage existing configurations.

DSC centers around creating *configurations*. A configuration is an easy-to-read script that describes an environment made up of computers (nodes) with specific characteristics. These characteristics can be as simple as ensuring a specific Windows feature is enabled or as complex as deploying SharePoint. Use DSC when the CSE will not work for your application.

In this example, we are installing IIS on the localhost. The configuration is saved as a PS1 file.

        configuration IISInstall
        {
                Node “localhost”
                {
                        WindowsFeature IIS
                        {
                                Ensure = “Present”
                                Name = “Web-Server”
                }       } 
        }

The DSC script consists of a Configuration block, Node block, and one or more resource blocks.

- The **Configuration** block is the outermost script block. You define it by using the **Configuration** keyword and providing a name. 
- **Node** blocks define the computers or VMs that you are configuring. In the example, there is one Node block that targets a computer named 'localhost'.
- One ore more resource blocks. **Resource** blocks configure the resource properties. In this example, there is one resource block that uses **WindowsFeature**. WindowsFeature indicates the name (Web-Server) of the role or feature that you want to ensure is added or removed. Ensure indicates if the role or features is added. Your choices are Present and Absent.

<hr>

## Knowledge check
1. What is a small application that provides post-deployment configuration and automation tasks on Azure VMs? -> VM extensions.
2. Custom script extensions time out after how long? -> 90 minutes.
3. The infrastructure team needs to install IIS on the localhost. They do not want to use a Custom Script Extension. Which of the following could be used instead? -> Desired State Configuration (DSC).

<hr>

## Configure app service plans
You will learn how to configure the App Service Plan including pricing and scaling.

## Objectives
- Identify features and usage cases of the Azure App Service.
- Select an appropriate Azure App Service plan pricing tier.
- Scale the App Service Plan.
- Scale out the App Service Plan.

<hr>

## Implement Azure app service plans
In App Service, an app runs in an App Service plan. An App Service plan defines a set of compute resources for a web app to run. These compute resources are analogous to the server farm in conventional web hosting. One or more apps can be configured to run on the same computing resources (or in the same App Service plan).

When you create an App Service plan in a certain region (for example, West Europe), a set of compute resources is created for that plan in that region. Whatever apps you put into this App Service plan run on these compute resources as defined by our App Service plan. Each App Service plan defines:

- **Region** (West US, East US, etc.)
- **Number of VM instances**
- **Size of VM instances** (Small, Medium, Large)

## How the app runs and scales
In the Free and Shared tiers, an app receives CPU minutes on a shared VM instance and cannot scale out. In other tiers, an app runs and scales as follows.

When you create an app in App Service, it is put into an App Service plan. When the app runs, it runs on all the VM instances configured in the App Service plan. If multiple apps are in the same App Service plan, they all share the same VM instances. If you have multiple deployment slots for an app, all deployment slots also run on the same VM instances. If you enable diagnostic logs, perform backups, or run WebJobs, they also use CPU cycles and memory on these VM instances.

The App Service plan is the scale unit of the App Service apps. If the plan is configured to run five VM instances, then all apps in the plan run on all five instances. If the plan is configured for autoscaling, then all apps in the plan are scaled out together based on the autoscale settings.

## Considerations
You can potentially save money by putting multiple apps into one App Service plan. You can continue to add apps to an existing plan as long as the plan has enough resources to handle the load. However, keep in mind that apps in the same App Service plan all share the same compute resources. To determine whether the new app has the necessary resources, you need to understand the capacity of the existing App Service plan, and the expected load for the new app. Overloading an App Service plan can potentially cause downtime for your new and existing apps. Isolate your app into a new App Service plan when:
- The app is resource-intensive
- You want to scale the app independently from the other apps in the existing plan
- The app needs resources in a different geographical region.

<hr>

## Determine app service plan pricing
The pricing tier of an App Service plan determines what App Service features you get and how much you pay for the plan.

- **Free and Shared**: Basic tiers that run on the same VMs as other apps. Some apps may belong to other customers. These tiers are intended to be used only for development and testing purposes. There is no SLA provided for these service plans, and are metered on a per App basis.
- **Basic**: Designed for apps that have lower traffic requirements, and don't need advanced auto scale and traffic management features. Pricing is based on the size and number of instances you run. Built-in network load-balancing support automatically distributes traffic across instances. This service plan with Linux runtime environments supports Web App for Containers.
- **Standard**: Designed for running production workloads. Has built-in network load-balancing support and auto scaling. If on Linux then also supports Web App for Containers.
- **Premium**: Designed to provide enhanced performance for production apps. The upgraded Premium plan, Premium v2, features Dv2-series VMs with faster processors, SSD storage, and double memory-to-core ratio compared to standard. The first generation of premium is still available to existing customers' scaling needs.
- **Isolated**: Designed to run mission critical workloads that are required to run in a virtual network. The private environment used with an Isolated plan is called the App Service Environment. The plan can scale to 100 instances with more available upon request.

<hr>

## Changing your App Service plan (scale up)
Your App Service plan can be scaled up or down at any time by changing the pricing tier.

## Other considerations
- The scale settings take only seconds to apply and affect all apps in your App Service plan. They don't require you to change your code or redeploy your application.
- If your app depends on other services, such as Azure SQL Database or Azure Storage, you can scale up these resources separately. These resources aren't managed by the App Service plan.

<hr>

## Configure app service plan scaling

## Autoscale settings
An autoscale setting is read by the autoscale engine to determine whether to scale out or in. Autoscale settings are grouped into profiles.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-app-service-plans/media/web-app-autoscale-94c4da54.png">

Rules include a trigger and a scale action (in or out). The trigger can be metric-based or time-based.
- **Metric-based**: Measure application load and add or remove. For example, do this action when CPU usage is above 50%. Examples of metrics are CPU time, Average response time, and Requests.
- **Time-based**: (schedule based) rules allow you to scale when you see time patterns in your load and want to scale before a possible load increase or decrease occurs. For example, trigger a webhook every 8am on Saturday in a given time zone.

## Considerations
- Having a minimum instance count makes sure your application is always running even under no load.
- Having a maximum instance count limits your total possible hourly cost.
- You can automatically scale between the minimum and maximum using rules you create.
- Ensure the max and min values are different and have an adequate margin between them.
- Always use a scale-out and scale-in rule combination that performs and increase and decrease.
- Choose the appropriate statistic for your diagnostics metric (Average, Minimum, Maximum, Total).
- Always select a safe default instance count. The default instance count is important because autoscale scales your service to that count when metrics are not available.
- Always configure autoscale notifications

## Notification settings
A notification setting defines what notifications should occur when an autoscale event occurs based on satisfying the criteria of one of the autoscale setting's profiles. Autoscale can notify one or more email addresses or make calls to one or more webhooks.

<hr>

## Knowledge check
1. The infrastructure team is responsible for managing a production web app. The app requires scaling to 5 instances and 100 GB of disk storage. The most cost efficient solution is desired. Which App Service Plan would meet the requirements. -> Premium.
2. What provides more CPU, memory, or disk space without adding more virtual machines? -> Scale up.
3. Triggering a webhook at 8am on Saturday is an example of which of the following? -> A time-based rule.

<hr>

## Configure Azure App Services
You'll learn how to configure and monitor the Azure App Service including deployment slots.

## Objectives
- Identify features and usage cases for the Azure App Service.
- Create an App Service.
- Configure deployment settings, specifically deployment slots.
- Secure the App Service.
- Configure custom domain names.
- Backup the App Service.
- Configure Application Insights.

<hr>

## Create an app service
When creating an App Service, you will need to specify a resource group and service plan. Then there are few other configuration choices.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-app-services/media/web-instances-feb0bc48.png">

- **Name**: Must be unique and will be used to locate your app. For example, webappces1.azurewebsites.net. You can map a custom domain name, if you prefer to use that instead.
- **Publish**: The app service can host either Code or a Docker Container.
- **Runtime stack**: The software stack to run the app, including the language and SDK versions. For Linux apps and custom container apps, you can also set an optional start-up command or file. Choices include: .NET Core, .NET Framework, Node.js, PHP, Python, and Ruby. Various versions of each are available.
- **Operating system**: Choices are Linux and Windows.
- **Region**: Your choice will affect app service plan availability.

## Application settings
Once your app service is created, additional configuration information is available.

Certain configuration settings can be included in the developer's code or configured in the app service. Here are a few interesting settings.
- **Always On**: Keep the app loaded even when there's no traffic. It's required for continuous WebJobs or for WebJobs that are triggered using a CRON expression.
- **ARR affinity**: In a multi-instance deployment, ensure that the client is routed to the same instance for the life of the session.
- **Connection strings**: Encrypted at rest and transmitted over an encrypted channel.

<hr>

## Create deployment slots
When deploy your web app, web app on Linux, mobile back end, or API app to Azure App Service, you can use a separate deployment slot instead of default production slot when you're running in the **Standard**, **Premium**, or **Isolated** App Service plan tier. Deployment slots are live apps with their own hostnames. App content and configurations element can be swapped between two deployment slots, including the production slot.

## Deployment slot advantages
- You can validate app changes in a staging deployment slot before swapping it with the production slot.
- Deploying an app to a slot first and swapping it into production ensures that all instances of the slot are warmed up before being swapped into production. This eliminates downtime when you deploy your app. The traffic redirection is seamless, and no requests are dropped because of swap operations. This entire workflow can be automated by configuring Auto Swap when pre-swap validation is needed.
- After a swap, the slot with previously staged app now has the previous production app. If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last know good site" back.

Auto swap streamlines Azure DevOps scenarios where you want to deploy your app continuously with zero cold starts and zero downtime for customers of the app. When auto swap is enabled from a slot into production, every time you push your code changes to that slot, App Service automatically swaps the app into production after it's warmed up into the source slot. Auto swap isn't current supported in we apps on Linux.

Note: Each App Service plan mode supports a different number of deployment slots.

<hr>

## Add deployment slots
New deployment slots can be empty or cloned. When you clone a configuration from another deployment slot, the cloned configuration is editable. Some configuration elements follow the content across a swap (not slot specific), whereas other configuration elements stay in the same slot after a swap (slot specific). Deployment slot settings fall into three categories.

- Slot-specific app settings and connection strings, if applicable.
- Continuous deployment settings, if enabled.
- App Service authentication settings, if enabled.

**Settings that are swapped:**
- General settings, such as framework version, 32/64-bit, web sockets
- App settings (can be configured to stick to a slot)
- Connection strings (can be configured to stick to a slot)
- Handler mappings
- Public certificates
- WebJobs content
- Hybrid connections *
- Service endpoints *
- Azure Content Delivery Network *

Features marked with an asterisk (*) are planned to be unswapped.

**Settings that aren't swapped:**
- Publishing endpoints
- Custom domain names
- Non-public certificates and TLS/SSL setting
- Scale settings
- WebJobs schedulers
- IP restrictions
- Always On
- Diagnostic settings
- Cross-origin resource sharing (CORS)
- Virtual network integration

<hr>

## Secure an app service
Azure App Service provides built-in authentication and authorization support, so you can sign in users and access data by writing minimal or no code in your web app, API, or mobile back end, and also Azure Functions.

Secure authentication and authorization requires deep understanding of security, including federation, encryption, JSON web tokens (JWT) management, grant types, and so on, App Service provides these utilities so that you can spend more time and energy on providing business value to you customer.

Note: You're not required to use App Service for authentication and authorization. many web frameworks are bundled with security features, and you can use them if you like.

## How it works
The authentication and authorization module runs in the same sandbox as your application code. When it's enabled, every incoming HTTP request passes through it before being handled by your application code. This module handles several things for your app:

- Authenticates users with the specified provider.
- Validates, stores, and refreshes tokens.
- Manages the authenticated session. 
- Injects identity information into request headers.

The module runs separately from your application code and is configured using app settings. No SDKs, specific languages, or changes to your application code are required.

## Configuration settings
In the Azure portal, you can configure App Service with a number of behaviors:

1. **Allow Anonymous requests (no action)**: This option defers authorization of unauthenticated traffic to your application code.  For authenticated requests, App Service also passes along authentication information in the HTTP headers. The option provides more flexibility in handling anonymous requests. It lets you present multiple sign-in providers to your users.
2. **Allow only authenticated requests**: The option is **Log in with <provider>**. App Service redirects all anonymous requests to `/.auth/login/<provider>` for the provider you choose. If the anonymous request comes from a native mobile app, the returned response is an `HTTP 401 Unauthorized`. With this option, you don't need to write any authentication code in your app.

Note: Restricting access in this way applies to all calls in your app, which may not be desirable for apps wanting a publicly available home page, as in many single-page applications.

## Logging and tracing
If you enable application logging, you will see authentication and authorization traces directly in your log files. If you see an authentication error that you didn't expect, you can conveniently find all the details by looking in your existing application logs. If you enabled failed request tracing, you can see exactly what role the authentication and authorization module may have played in a failed request. in the trace logs, look for references to a module named `EastAuthModule_32/64`.

<hr>

## Create custom domain names
When you create a web app, Azure assigns it to a subdomain of azurewebsites.net. Azure also assigns a virtual IP address.

## Configuration steps
1. **Reserve your domain name**: If you haven't already registered for an external domain name already, the easiest way to set up a custom domain is to buy  one directly in the Azure portal. If you do not use the portal, you can sue any domain registrar.
2. **Create DNS records that map the domain to your Azure web app**: The Domain Name System (DNS) uses data records to map domain names into IP addresses. There are several types of DNS records. For web apps, you'll create either an A record or a CNAME record. If the IP address changes, a CNAME entry is still valid, whereas an A record must be updated. However, some domain registrars do not allow CNAME records for the root domain or for wildcard domains. In that case, you must use an A record.

- An A (Address) record maps a domain name to an IP address.
- A CNAME (Canonical Name) record maps a domain name to another domain name. DNS uses the second name to look up the address. Users still see the first domain name in their browser. For example, you could map contoso.com to your webapp.azurewebsites.net.

3. **Enable the custom domain**: After obtaining your domain and creating your DNS record, you can use the portal to validate the custom domain and add it to your web app.

Note: To map a custom DNS name to a web app, the web app's App Service plan must be a paid tier.

<hr>

## Back up an app service
The Backup and Restore feature in Azure App Service lets you easily create app backups manually or on a schedule. You can configure the backups to be retained for up to an indefinite amount of time. You can restore the app to a snapshot of a previous state by overwriting the existing app or restoring to another app.

## What gets backed up
App Service can backup the following information to an Azure storage account and container that you have configured your app to use.
- App configuration.
- File content.
- Database connected to your app (SQL Database, Azure Database for MYSQL, Azure Database for PostgreSQL, MYSQL in-app).

## Considerations
- The Backup and Restore feature requires the App Service plan to be in the Standard or Premium tier.
- You can configure backups manually or on a schedule.
- You need an Azure storage account and container in the same subscription as the app that you want to back up. After you have made one or more backups for your app, the backups are visible on the Containers page of your storage account, and your app. In the storage account, each backup consists of a .zip file that contains the backup data and an .xml file that contains a manifest of your .zip file contents. You can unzip and browse these files if you want to access your backups without actually performing an app restore.
- Full backups are the default. When a full backup is restored, all content on the site is replaced with whatever is in the backup. If a file is on the site, but not in the backup it gets deleted.
- Partial backups are supported. Partial backups allow you to choose exactly which files you want to back up. When a partial backup is restored, any content that is located in one of the excluded directories, or any excluded file, is left as is. You restore partial backups of your site the same way you would restore a regular backup.
- You can exclude files and folders you do not want in the backup.
- Backups can be up to 10 GB of app and database content.
- Using a firewall enabled storage account as the destination for your backups is not supported.

<hr>

## Use Application Insights
Application Insights, a feature of Azure Monitor, monitors your live applications. It will automatically detect performance anomalies, and includes powerful analytics tools to help you diagnose issues and to understand what users actually do with your app. It's designed to help you continuously improve performance and usability. Insights works on various platforms including .NET, Node.js, and Java EE, hosted on-premises, hybrid, or any public cloud. It integrates with your DevOps process, and has connection points to a variety of development tools. It can monitor and analyze data from mobile apps by integrating with Visual Studio App Center.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-app-services/media/app-insights-16629887.png">

## Application Insights features
Application Insights is aimed at the development team, to help you understand how your app is performing and how it's being used. It monitors:
- **Request rates, response times, and failure rates**
- **Dependency rates, response times, and failure rates**: Find out whether external services are slowing you down.
- **Exceptions**: Both server and browser exceptions are reported.
- **Page views and load performance**: Reported by your users' browsers.
- **User and session counts**
- **Performance counters**: From your Windows and Linux server machines, such as CPU, memory, and network usage.
- **Host diagnostics**: From Docker or Azure.
- **Diagnostic trace logs**: From your app - so you can correlate trace events with requests.
- **Custom events and metrics**: That your write yourself in the client or server code.

<hr>

## Knowledge check
1. When cloning a configuration from another deployment slot, which configuration setting follows the content across the swap? -> Connection strings
2. The marketing team wants to know which web pages are most popular, at what times of day, and where the users are located. Which of the following should be recommended? -> Application Insights
3. Which of the following is a valid automated deployment source? GitHub

<hr>

## Configure Azure Container Instances
You will learn how to configure Azure Container Instances including container groups.

## Objectives
- Identify when to use containers versus virtual machines.
- Identify the features and usage cases of Azure Container Instances.
- Implement Azure Container Groups.

<hr>

## Compare containers to virtual machines
Containers represent the next stage in the virtualization of computing resources. Container-based virtualization allows you to virtualize the operating system. This way, you can run multiple applications within the same instance of an operating system, while maintaining isolation between the applications. This means that containers within a VM provide functionality similar to that of VMs within a physical server.

**Feature** | **Containers** | **Virtual Machines**
----------- | -------------- | --------------------
Isolation | Typically provides lightweight isolation from the host and other containers but doesn't provide as strong a security boundary as a virtual machine. | Provides complete isolation from the host operating system and other VMs. This is useful when a strong security boundary is critical, such as hosting apps from competing companies on the same server or cluster.
Operating system | Runs the user mode portion of an operating system and can be tailored to contain just the needed services for your app, using fewer system resources. | Runs a complete operating system including the kernel, thus requiring more system resources (CPU, memory, and storage).
Deployment | Deploy individual containers by using Docker via command line; deploy multiple containers using an orchestrator such as Azure Kubernetes Service. | Deploy individual VMs by using Windows Admin Center or Hyper-V Manager; deploy multiple VMs by using PowerShell or System Center Virtual Machine Manager.
Persistent storage | Use Azure Disks for local storage for a single node, or Azure Files (SMB shares) for storage shared by multiple nodes or servers. | Use a virtual hard disk (VHD) for local storage for a single VM, or an SMB file share for storage shared by multiple servers.
Fault tolerance | If a cluster node fails, any containers running on it are rapidly recreated by the orchestrator on another cluster node. | VMs can fail over to another server in a cluster, with the VM's operating system restarting on the new server.

## Container advantages
Containers offer several advantages over physical and virtual machines, including:
- Increased flexibility and speed when developing and sharing the application code.
- Simplified application testing.
- Streamlined and accelerated application development.
- Higher workload density, resulting in improved resource utilization.

<hr>

## Review Azure container instances
Containers are becoming the preferred way to package, deploy, and manage cloud applications. Azure Container Instances offers the fastest and simplest way to run a container in Azure, without having to manage any virtual machines and without having to adopt a higher-level service. Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-container-instances/media/container-overview-0e72c2ba.png">

**Feature** | **Description**
----------- | ---------------
Fast Startup Times | Containers can start in seconds without the need to provision and manage virtual machines.
Public IP Connectivity and DNS Names | Containers can be directly exposed to the internet with an IP address and FQDN
Hypervisor-level Security | Container applications are as isolated in a container as they could be in a virtual machine
Custom Sizes | Container nodes can be scaled dynamically to match actual resource demands for an application.
Persistent Storage | Containers support direct mounting of Azure File Shares.
Linux and Windows Containers | Container instances can schedule both Windows and Linux containers. Simply specify the OS type when you create your container groups.
Coscheduled Groups | Container instances supports scheduling of multi-container groups that share host machine resources.
Virtual Network Deployment | Container instances can be deployed into an Azure virtual network.

<hr>

## Implement container groups
The top-level resource in Azure Container Instances is the container group. A container group is a collection of containers that get scheduled on the same host machine. The containers in a container group share a lifecycle, resources, local network, and storage volumes. It's similar in concept to a pod in Kubernetes.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-container-instances/media/container-groups-ea19ee6b.png">

An example container group:
- Is scheduled on a single host machine.
- Is assigned a DNS name label.
- Exposes a single public IP address, with one exposed port.
- Consists of two containers. One container listens on port 80, while the other listens on port 1433.
- Includes two Azure file shares as volume mounts, and each container mounts one of the shares locally.

## Deployment options
Here are two common ways to deploy a multi-container group: use a Resource Manager template or a YAML file. A Resource Manager template is recommended when you need to deploy additional Azure service resources (for example, an Azure Files share) when you deploy the container instances. Due to the YAML format's more concise nature, a YAML file is recommended when your deployment includes only container instances.

## Resource allocation
Container groups can share an external-facing IP address, one or more ports on that IP address, and a DNS label with a fully qualified domain name (FQDN). To enable external clients to reach a container within your group, you must expose the port on the IP address and from the container. Because containers within the group share a port namespace, port mapping isn't supported. A container group's IP address and FQDN will be released when the container group is deleted.

## Common scenarios
Multi-container groups are useful in cases where you want to divide a single function task into a small number of container images. These images can then be delivered by different teams and have separate resource requirements. Example usage could include:
- A container serving a web application and a container pulling the latest content from source control.
- An application container and a logging container. The logging container collects the logs and metrics output by the main application and writes them to a long-term storage.
- An application container and a monitoring container. The monitoring container periodically makes a request to the application to ensure that it's running and responding correctly, and raises an alert if it's not.
- A front-end container and a back-end container. The front end might serve a web application, with the back end running a service to retrieve data.

<hr>

## Review the docker platform
Docker is a platform that enables developers to host applications within a container.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-container-instances/media/docker-c787c4b8.png">

A container is essentially a standalone package that contains everything that is needed to execute a piece of software. The package includes:

- The application executable code.
- The runtime environment (such as .NET Core).
- System tools.
- Settings.

The Docker platform is available on both Linux and Windows and can be hosted on Azure. The key thing that Docker provides is the guarantee that the containerized software will always run the same. It doesn't matter if the code is run locally on Windows, Linux or in the cloud on Azure. The software can be developed locally within a Docker container, shared with QA resources for testing, and then deployed to production in the Azure Cloud. Once deployed, the application can easily be scaled using the Azure Container Instances (ACI).

## Docker terminology
You should be familiar with the following key terms before using Docker and Container Instances to create, build, and test containers:

- **Container**: An instance of a Docker image. It represents the execution of a single application, process, or service. It consists of the contents of a Docker image, an execution environment, and a standard set of instructions. When scaling a service, you create multiple containers from the same image, passing different parameters to each instance.
- **Container image**: Refers to a package with all the dependencies and information required to create a container. The dependencies include frameworks and the deployment and execution configuration that a container runtime uses. Usually, an image derives from multiple base images that are layers stacked on top of each other to form the container's file system. An image is immutable once it has been created.
- **Build**: Refers to the action of building a container image based on the information and context provided by the Docker file. The build also includes any other files that are needed. You build images by using the `docker build` command.
- **Pull**: Refers to the process of downloading a container image from a container registry.
- **Push**: Refers to the process of uploading a container image to a container registry.
- **Dockerfile**: Refers to a text file that contains instructions on how to build a Docker image. The Dockerfile is like a batch script. The first line identifies the base image. The rest of the file includes the build actions.

<hr>

## Knowledge check
1. What is a reason to select VMs or containers? -> VMs provide complete isolation from the host operating system and other VMs.
2. What is a feature of Azure Container Instances? -> Billing occurs when the container is in use.

<hr>

## Configure Azure Kubernetes Service
You will learn how to configure Azure Kubernetes including networking, storage, and scaling.

## Objectives
- Identify AKS components including pods, clusters, and nodes.
- Configure network connections for AKS.
- Configure storage options for AKS.
- Implement security options for AKS.
- Scale AKS including adding Azure Container Instances.

<hr>

## Explore the AKS terminology

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-kubernetes-service/media/kubernetes-terms-ee59aa6e.png">

- **Pools** are groups of nodes with identical configurations.
- **Nodes** are individual virtual machines running containerized applications.
- **Pods** are a single instance of an application. A pod can contain multiple containers.
- **Container** is a lightweight and portable executable image that contains software and all of its dependencies.
- **Deployment** has one or more identical pods managed by Kubernetes.
- **Manifest** is the YAML file describing a deployment.

<hr>

## Explore the AKS cluster and node architecture
A Kubernetes cluster is divided into two components:
- **Azure-managed nodes**, which provide the core Kubernetes services and orchestration of application workloads.
- **Customer-managed nodes** that run your application workloads.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-kubernetes-service/media/kubernetes-clusters-4ae0bac1.png">

## Azure-managed node
When you create an AKS cluster, a cluster node is automatically created and configured. This node is provided as a managed Azure resource abstracted from the user. You pay only for the running agent nodes.

## Nodes and node pools
To run your applications and supporting services, you need a Kubernetes node. An *AKS cluster* contains one ore more nodes (Azure VMs) that run the Kubernetes node components and the container runtime.

- The *kubelet* is the Kubernetes agent that processes the orchestration requests from the Azure-managed node, and scheduling of running the requested containers.
- Virtual networking is handled by the *kube-proxy* on each node. The proxy routes network traffic and manages IP addressing for services and pods.
- The *container runtime* is the component that allows containerized applications to run and interact with additional resources such as the virtual network and storage. AKS clusters using Kubernetes version 1.19 node pools and greater used containerd as its container runtime. AKS clusters using Kubernetes prior to v1.19 to node pools use Moby (upstream docker) as its container runtime.

Nodes of the same configuration are grouped together into *node pools*. A Kubernetes cluster contains one or more node pools. The initial number of nodes and size are defined when you create an AKS cluster, which creates a default node pool. This default node pool in AKS contains the underlying VMs that run your agent nodes.

<hr>

## Configuring AKS networking
To allow access to your applications, or for application components to communicate with each other, Kubernetes provides an abstraction layer to virtual networking. Kubernetes nodes are connected to a virtual network, and can provide inbound and outbound connectivity for pods. The *kube-proxy* component runs on each node to provide these network features.

In Kubernetes, Serices logically group pods to allow for direct access via an IP address or DNS name and on a specific port. You can also distribute traffic using a load balancer. More complex routing of application traffic can also be achieved with Ingress Controllers. Security and filtering of the network traffic for pods is possible with Kubernetes network policies.

The Azure platform also helps to simplify virtual networking for AKS clusters. When you create a Kubernetes load balancer, the underlying Azure load balancer resource is created and configured. As you open network ports to pods, the corresponding Azure network security group rules are configured. For HTTP application routing, Azure can also configure external DSN as new ingress routes are configured.

## Services
To simplify the network configuration for application workloads, Kubernetes uses Services to logically group a set of pods together and provide network connectivity. The following Service types are available:

- **Cluster IP**: Creates an internal IP address for use within the AKS cluster. Good for internal-only applications that support other workloads within the cluster.
- **NodePort**: Creates a port mapping on the underlying node that allows the application to be access directly with the node IP address and port.
- **LoadBalancer**: Creates an Azure load balancer resource, configures an external IP address, and connects the requested pods to the load balancer backend pool. To allow customers traffic to reach the application, load-balancing rules are created on the desired ports.
- **ExternalName**: Creates a specific DNS entry to easier application access.

The IP address for load balancers and services can be dynamically assigned, or you can specify an existing static IP address to use. Both internal and external static IP addresses can be assigned. The existing static IP address is often tied to a DNS entry.

Both *internal* and *external* load balancers can be created. Internal load balancers are only assigned a private IP address, so can't be accessed from the Internet.

## Pods
Kubernetes uses pods to run an instance of your application. A pod represents a single instance of your application. Pods typically have a 1:1 mapping with a container, although there are advanced scenarios where a pod might contain multiple containers. These multi-container pods are scheduled together on the same node, and allow containers to share related resources.

When you create a pod, you can define resource limits to request a certain amount of CPU or memory resources. The Kubernetes Scheduler attempts to schedule the pods to run on a node with available resources to meet the request. You can also specify maximum resource limits that prevent a given pod from consuming too much compute resource from the underlying node.

Note: A best practice is to include resource limits for all pods to help the Kubernetes Scheduler understand what resources are needed and permitted.

A pod is a logical resource, but the container (or containers) is where the application workloads run. Pods are typically ephemeral, disposable resources. Therefore, individually scheduled pods miss some of the high availability and redundancy features Kubernetes provides. Instead, pods are usually deployed and managed by Kubernetes controllers, such as the Deployment controller.

<hr>

## Configure AKS storage
This section introduces the core concepts that provide storage to your applications in AKS:
- Volumes
- Persistent volumes
- Storage classes
- Persistent volume claims

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-kubernetes-service/media/kubernetes-storage-b14b0f2d.png">

## Volumes
A *volume* represents a way to store, retrieve, and persist data across pods and through the application lifecycle.

Traditional volumes to store and retrieve data are created as Kubernetes resources backed by Azure Storage. You can manually create these data volumes to be assigned to pods directly, or have Kubernetes automatically create them. These data volumes can use Azure Disks or Azure Files:

- *Azure Disks* can be used to create a Kubernetes *DataDisk* resource. Disks can use Azure Premium storage, backed by high-performance SSDs, or Azure Standard storage, backed by regular HDDs. For most production and development workloads, use Premium storage. Azure Disks are mounted as *ReadWriteOnce* so are only available to a single node. For storage volumes that can be accessed by multiple nodes simultaneously, use Azure Files.

- *Azure Files* can be used to mount an SMB 3.0 share backed by an Azure Storage account to pods. Files let you share data across multiple nodes and pods. Files can use Azure Standard storage backend by regular HDDs, or Azure Premium storage, backed by high-performance SSDs.

## Persistent volumes
Volumes are defined and created as part of the pod lifecycle only exist until the pod is deleted. Pods often expect their storage to remain if a pod is rescheduled on a different host during a maintenance event, especially in StatefulSets. A *persistent volume* (PV) is a storage resource created and managed by the Kubernetes API that can exist beyond the lifetime of an individual pod.

Azure Disks or Files are used to provide the PersistentVolume. As noted in the previous section on Volumes, the choice of Disks or Files is often determined by the need for concurrent access to the data or the performance tier.

A PersistentVolume can be *statically* created by a cluster administrator, or dynamically created by the Kubernetes API server. If a pod is scheduled and requests storage that is not currently available, Kubenetes can create the underlying Azure Disk or Files storage and attach it to the pod. Dynamic provisioning  uses a *StorageClass* to identify what type of Azure storage needs to be created.

## Storage classes
To define different tiers of storage, such as Premium and Standard, you can create a *StorageClass*. The StorageClass also defines the *reclaimPolicy*. This reclaimPolicy controls the behavior of the underlying Azure storage resource when the pod is deleted and the persistent volume may no longer be required. The underlying storage resource can be deleted, or retained for use with a future pod.

In AKS, four initial StorageClasses are created for cluster using the in-ree storage plugins:

- default: uses Azure StandardSSD storage to create a Managed Disk. The reclaim policy ensure that the underlying Azure Disk is deleted when the persistent volume that used it is deleted.
- managed-premium: Uses Azure Premium storage to create a Managed Disk. The reclaim policy again ensures that the underlying Azure Disk is deleted when the persistent volume that used it is deleted.
- azurefile: Uses Azure Standard storage to create an Azure File Share. The reclaim poicy ensures that the underlying Azure File Share is deleted when the persistent volume that used it is deleted.

If no StorageClass is specified for a persistent volume, the default StorageClass is used. Take care when requesting persistent volumes so that they use the appropriate storage you need. You can create a StorageClass for additional needs using `kubectl`.

## Persistent volume claims
A PersistentVolumeClaim requests either Disk or File storage of a particular StorageClass, access mode, and size. The Kubernetes API server can dynamically provision the underlying storage resource in Azure if there is no existing resource to fulfill the claim based on the defined StorageClass. The pod definition includes the volume mount once the volume has been connected to the pod.

A PersistentVolume is *bound* to a PersistentVolumeClaim once an availble storage resource has been assigned to the pod requesting it. There is 1:1 mapping of persistent volumes to claims.

<hr>

## Configure AKS scaling
<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-kubernetes-service/media/kubernetes-scale-a7fff281.png">

## Manually scale pods or nodes
You can manually scale replicas (pods) and nodes to test how your application responds to a change in available resources and state. Manually scaling resources also lets you define a set amount of resources to use to maintain a fixed cost, such as the number of nodes. To manually scale, you define the replica or node count, and the Kubernetes API schedules creating new pods or draining nodes.

## Horizontal pod autoscaler
Kubernetes uses the horizontal pod autoscaler (HPA) to monitor the resource demand and automatically scale the number of replicas. By default, the HPA checks the Metrics API every 30 seconds for any required chanes in replica count. When change are required, the number of replicas is increased or decreased accordingly. HPA works with AKS clusters that have deployed the Metrics Server for Kubernetes 1.8+.

When you configure the HPA for a given deployment, you define the minimum and maximum number of replicas that can run. You also define the metric to monitor and base any scaling decisions to, such as CPU usage.

## Cooldown of scaling events
As the horizontal pod autoscaler checks the Metrics API every 30 seconds, previous scale events may not have successfully completed before another check is made. This behavior could cause the horizontal pod autoscaler to change the number of replicas before the previous scale event has been able to receive application workload and the resource demands to adjust accordingly.

To minimize these race events, cooldown or delay values can be set. These values define how long the HPA must wait after a scale event before another scale event can be triggered. By default, the delay on scale up events is 3 minutes, and the deplay on scale down events is 5 minutes.

You may need to tune these cooldown values. The default cooldown values may give the impression that the HPA isn't scaling the replica count quickly enough. For example, to more quickly increase the number of replicas in use, reduce the `--horizontal-pod-autoscaler-upscale-delay` when you create your horizontal pod autoscaler definitions using `kubectl`.

## Cluster autoscaler
To respond to changing pod demands, Kubernetes has a cluster autoscaler that adjusts the number of nodes based on the requested compute resources in the node pool. By default, the cluster autoscaler checks the API server every 10 seconds for any required chanes in node count. If the cluster autoscale determines that a change is required, the number of nodes in your AKS cluster is increased or decreased accordingly. The cluster autoscaler works with RBAC-enabled AKS cluster that run Kubernetes 1.10x or higher.

Cluster autoscaler is typically used alongside the HPA. When combined, the HPA increases or decreases the number of pods based on application demand, and the cluster autoscaler adjusts the number of nodes as needed to run those additional pods accordingly.

## Scale out events
If a node does not have sufficient compute resources to run a requested pod, that pod cannot progress through the scheduling process. The pod cannot start unless other compute resources are available within the node pool.

When the cluser autoscaler notices pods that cannot be scheduled due to node pool resource contraints, the number of nodes within the node pool is increased to provide the extra compute resources. When those addditional nodes are successfully deployed and available for use within the node pool, the pods are then scheduled to run on them.

If your application needs to scale rapidly, some pods may remain in a state waiting to be scheduled until the new nodes deployed by the cluster autoscaler can accept the scheduled pods. For applications that have high burst demands, you can scale with virtual nodes and Azure Container Instances.

## Scale in events
The cluster autoscaler also monitors the pod scheduling status for nodes that have not recently received new scheduling requests. This scenario indicates that the node pool has more compute resources than are required, and that the number of nodes can be decreased.

A node that passes a threshold for no longer being needed for 10 minutes by default is scheduled for deletion. When this situation occurs, pods are scheduled to run on other nodes within the node pool, and the cluster autoscaler decreases the number of nodes.

You applications may experience some disruption as pods are scheduled on different nodes when the cluster autoscaler decreases the number of nodes. To minimize disruption, avoid applications that use a single pod instance.

<hr>

## Configure AKS scaling to Azure Container Instances
<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-kubernetes-service/media/kubernetes-burst-686ee747.png">

To rapidly scale your AKS cluster, you can integrate with Azure Container Instances (ACI). Kubernetes had built-in components to scale the replica and node count. However, if your application needs to rapidly scale, the HPA may schedule more pods than can be provided by the existing compute resources in the node pool. If configured, this scenario would then trigger the cluster autoscaler to deploy additional nodes in the node pool. It may take a few minutes for those nodes to successfully provision.

ACI lets you quickly deploy container instances without more infrastructure overhead. When you connect with AKS, ACI becomes a secured, logical extension of your AKS cluster. The Virtual Kubelet component is installed in your AKS cluster that presents ACI as a virtual Kubernetes node. Kubernetes can then schedule pods that run as ACI instances through virtual nodes, not as pods on VM nodes directly in your AKS cluster.

Your application requires no modification to use virtual nodes. Deployments can scale across AKS and ACI. There is no delay when the cluster autoscaler deploys new nodes in your AKS cluster.

Virtual nodes are deployed to another subnet in the same virtual network as your AKS cluster. This virtual network configuration allows the traffic between ACI and AKS to be secured. Like an AKS cluster, an ACI instance is a secure, logical compute resource that is isolated from other users.

<hr>

## Knowledge check
1. What is the Kubernetes agent that processes the orchestration requests and schedules running the requested containers? -> kublet
2. Which service would be best for internal-only applications that support other workloads within the cluster? -> ClusterIP
3. The leadership team has decided to move all servicesto Azure Kubernetes service. What contributes to the monthly Azure charge? -> Per node VM.

<hr>

## Summary
Azure Kubernetes Services (AKS) is recommended when you need full container organization. AKS includes discovery across multiple containers, automatic scaling, and coordinated application upgrades.

<hr>

## Manage virtual machines with the Azure CLI
Learn how to use the cross-platform Azure CLI to create, start, stop, and perform other management tasks related to VMs in Azure.

## Objectives
- Create a VM with the Azure CLI
- Resize VMs with the Azure CLI
- Perform basic management tasks using the Azure CLI
- Connect to a running VM with SSH and the Azure CLI

<hr>

## Knowledge check
1. Can you access Azure CLI on a tablet? -> Yes
2. What argument can you use to not block the CLI while a script is running? -> '--no-wait'
3. What can you use with Azure CLI to filter the results to get only the data you need? -> '--query'

<hr>

## Create a Windows VM in Azure
Azure virtual machines (VMs) enable you to create dedicated compute resources in minutes that can be used just like a physical desktop or server machine.

## Objectives
- Create a Windows virtual machine using the Azure portal
- Connect to a running Windows virtual machine using Remote Desktop
- Install software and change the network configuration on a VM using the Azure portal.

<hr>

## Introduction to Windows VMs in Azure
Azure VMs are an on-demand, scalable cloud computing resource. They're similar to virtual machines that are hosted in Winkdows Hyper-V. They include processor, memory, storage, and networking resources. You can start and stop VMs at will, just like with Hyper-V, and manage them from the Azure portal or with the Azure CLI. You can also use a Remote Desktop Protocol (RDP) client to connect directly to the Windows desktop user interface and use the VM as if you were signed in to a local Windows computer.

## Creating an Azure VM
You can define and deploy VMs on Azure in several ways: the Azure portal, a script (using the Azure CLI or Azure PowerShell), or through an ARM template. In all cases, you'll need to supply several pieces of information, which we'll cover shortly.

The Azure Marketplace also provides pre-configured images that include both an OS and popular software tools installed for specific scenarios.

## Resources used in a Windows VM
When creating a Windows VM in Azure, you also create resources to host the VM. These resources work together to virtualize a computer and run the Windows operating system. These must either exist (and be selected during VM creation), or they will be created with the VM.

- A VM that provides CPU and memory resources
- An Azure Storage account to hold the virtual hard disks
- Virtual disks to hold the OS, applications, and data
- Virtual network (VNet) to connect the VM to other Azure services on your own on-premises hardware
- A network interface to communicate with the VNet
- A public IP address so you can access the VM (this is optional)

Like other Azure services, you'll need a **Resource Group** to contain the VM (and optionally group these resources together for administration). When you create a new VM, you can either use an existing resource group or create a new one.

## Choose the VM image
Selecting an image is one of the first and most important decisions you'll make when creating a VM. An image is a template that's used to create a VM. These templates include an OS and often other software, such as development tools or web-hosting environments.

Any application that the computer can support can be included in the VM image. You can create a VM fom an image that's pre-configured to exactly match your requirements, such as hosting an ASP.NET Core app.

## Sizing your VM
Just as a physical machines has a certain amount of memory and CPU power, so does a VM. Azure offers a range of VMs of differening size at different price points. The size that you choose will determine the VMs processing power, memory, and max storage capacity.

Warning: There are quota limits on each subscription that can impact VM creation. In the classic deployment model, you cannot have more than 20 virtual *cores* across all VMs within a region. You can either split up VMs across regions or file an online request to increase your limits.

VM sizes are grouped into categories, starting with the B-series for basic testing, and running up to the H-series for massive computing tasks. You should select the size of the VM based on the workload you want to perform. It is possible to change the size of a VM after it's been created, but the VM must be stopped first, so it's best to size it appropriately from the start if possible.

## Scenario based guidelines

- **General use computer/web**: Use B, or D*** VM sizes
- **Heavy computational tasks**: Use F*** VM sizes
- **Large memory usage**: Use E**, M, G*, or D*v2 sizes
- **Data storage and processing**: Use Ls
- **Heavy graphics rendering**: Use N** sizes
- **High-performance computing (HPC)**: Use H

## Choosing storage options
Disk technology options include a traditional platter-based hard dist drive (HDD) or a more modern solid-state drive (SSD).

## Mapping storage to disks
Azure uses virtual hard disks (VHDs) to represent physical disks for the VM. VHDs replicate the logical format and data of a disk drive, but are stored as page blobs in an Azure Storage account. You can choose on a per-disk basis what type of storage it should use (SSD or HDD). This allows you to control the performance of each disk, likely based on the I/O you plan to perform on it.

By default, two VHDs will be created for your Windows VM:
1. The **Operating System disk**: This is your primary or C: drive and has a maximum capacity of 2048 GB.
2. A **Temporary disk**: This provides temporary storage for the OS or any apps. It is configured as the D: drive by default and is sized based on the VM size, making it an idea location for the Windows paging file.

Warning: A temporary disk is not persistent. You should only write data to this disk that you are willing to lose at any time.

## What about data?
You can store data on the C: drive along with the OS, but a better approach is to create dedicated *data disks*. You can create and attach additional disks to the VM. Each data disk can hold up to 32,767 GiB (gibibytes) of data, with the maximum amount of storage determed by the VM size you select.

Note: An interesting capability is to create a VHD image from a real disk. This allows you to easily migrate *existing* information from an on-premises computer to the cloud.

## Unmanaged vs. Managed disks
The final storage choice you'll make is whether to use **unmanaged** or **managed** disks.

With unmanaged disks, you are responsible for the storage accounts that are used to hold the VHDs corresponding to your VM disks. You pay the storage account rates for the amount of space you use. A single storage account has a fixed rate limit of 20,000 I/O operations/sec. This means that a single storage account is capable of supporting 40 standard virtual hard disks at full throttle. If you need to scale out, then you need more than one storage account, which can get complicated.

Managed disks are the newer (and recommended) disk-storage model. They elegantly solve the complexity of unmanaged disks by putting the burden of managing the storage accounts onto Azure. You specify the disk type (Premium or Standard) and the size of the disk, and Azure creates and manages both the disk and the storage it uses. You don't have to worry about storage account limits, which makes them easier to scale out. They also offer several other benefits:

- **Increased reliability**: Azure ensures that VHDs associated with high-reliability VMs will be placed in different parts of Azure storage to provide similar levels of resilience.
- **Better security**: Managed disks are truly managed resources in the resource group. This means they can use role-based access control to restrict who can work with the VHD data.
- **Snapshot support**: You can use snapshots to create a readonly copy of a VHD. You have to shut down the owning VM, but creating the snapshot only takes a few seconds. Once it's done, you can power on the VM and use the snapshot to create a duplicate VM to troubleshoot a production issue or roll back the VM to the point in time that the snapshot was taken.
- **Backup support**: You can automatically back up managed disks to different regions for disaster recovery with Azure Backup, all without affecting the service of the VM.

## Network communication
VMs communicate with external resources using a virtual network (VNet). The VNet represents a private network in a single region on which your resources communicate. A virtual network is just like the networks you manage on-premises. You can divide them up with subnets to isolate resources, connect them to other networks (including your on-premises networks), and apply traffic rules to govern inbound and outbound connections.

## Planning your network
When you create a new VM, you'll have the option of creating a new virtual network, or using an existing VNet in your region.

Having Azure create the network together with the VM is simple, but it's likely not ideal for most scenarios. It's better to plan your network requirements *up front* for all the components in your architecture and create the VNet structure you'll need separately, and then create the VMs and place them into the already-created VNets.

<hr>

## What is the Remote Desktop Protocol?
Remote Desktop (RDP) provides remote connectivity to the UI of Windows-based computers. RDP enables you to sign in to a remote physical or virtual Windows computer and control that computer as if you were seated at the console. An RDP connection enables you to carry out the vast majority of operations that you can do from the console of a physical computer, with the exception of some power and hardware-related functions.

An RDP connection requires an RDP client. Microsoft provides RDP clients for the following operating systems:
- Windows (build-int)
- macOS
- iOS
- Android

There are also open-source Linux clients, such as Remmina, that allow you to connect to a Windows PC from an Ubuntu distribution.

## Connecting to an Azure VM
With a public IP, we can communicate with the VM over the Internet. Alternatively, we can set up a VPN to connects our on-premises network to Azure, letting us securely connect to the VM without exposing a public IP.

One thing to be aware of with public IP addresses in Azure is they're often dynamically allocated. That means the IP address can change over time; for VMs, this happens when the VM is restarted. You can pay more to assign static addresses if you want to connect directly to an IP address instead of a name and need to ensure that the IP address won't change.

## How to you connect to a VM in Azure using RDP?
In the Azure portal, go to the properties of your VM, and at the top click, **Connect**. This will show you the IP addresses assigned to the VM and give you the option to download a **preconfigured.rdp** file that Windows then opens in the RDP client. You can choose to connect over the public IP address of the VM in the RDP file. Instead, if you're connecting over VPN or ExpressRoute, you can select the internal IP address. You can also select the port number for the connection.

If you're using a static public IP address for the VM, you can save the **.rdp** file to your desktop. If you're using dynamic IP addressing, the **.rdp** file only remains valid while the VM is running. If you stop and restart the VM, you must download another **.rdp** file.

When you connect, you'll typically receive two warnings. These are:
- **Publisher warning**: caused by the **.rdp** file not being publicly signed
- **Certificate warning**: caused by the machine certificate not being trusted

In test environments, you can ignore these warnings. In production environments, the **.rdp** file can be signed using **RDPSIGN.EXE** and the machine certificate placed in the client's **Trusted Root Certification Authorities** store. 


<hr>

## Open ports in Azure VMs
By default, new VMs are locked down.

Apps can make outgoing requests, but the only inbound traffic allowed is from the virtual network (for example, other resources on the same network), and from Azure's Load Balancer (probe checks).

There are two steps to adjusting the configuration to support FTP. When you create a new VM, you have an opportunity to open a few common ports (RDP, HTTP, HTTPS, and SSH). However, if you require other changes to the firewall, you will need to do them yourself.

The process for this involves two steps:
1. Create a Network Security Group.
2. Create an inbound rule allowing traffic on port 20 and 21 for active FTP support.

## What is a Network Security Group
Virtual networks (VNets) are the foundation of the Azure networking model, and provide isolation and protection. Network Security Groups (NSGs) are the main tool you use to enforce and control network traffic rules at the networking level. NSGs are an optional security layer that provides a software firewall by filtering inbound and outbound traffic on VNet.

Security groups can be associated to a network interface (for per-host rules), a subnet in the virtual network (to apply to multiple resources), or both levels.

## Security group rules
NSGs use *rules* to allow or deny traffic moving through the network. Each rule identifies the source and destination address (or range), protocol, port (or range), direction (inbound or outbound), a numeric priority, and whether to allow or deny the traffic that matches the rule. The following illustration shows NSG rules applied at the subnet and network interface levels.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/create-windows-virtual-machine-in-azure/media/7-nsg-rules.png">

Each security group has a set of default security rules to apply the default network rules described above. These default rules cannot be modified, but *can* be overriden.

# How Azure uses network rules
For inbound traffic, Azure processes the security group associated to the subnet, then the security group applied to the network interface. Outbound traffic is processed in the opposite order (the network interface first, followed by the subnet).

**Warning**: Security groups are optional at both levels. If no security group is applied, then **all traffic is allowed** by Azure. If the VM has a public IP, this could be a serious risk, particularly if the OS doesn't provide some sort of firewall.

The rules are evaluated in *priority order*, starting with the **lowest priority** rule. Deny roles always *stop* the evaluation. For example, if an outbound request is blocked by a network interface rule, any rules applied to the subnet will not be checked. In order for traffic to be allowed through the security group, it must pass through all applied groups.

The last rule is always a **Deny All** rule. This is a default rule added to every security group for both inbound and outbound traffic with a priority of 
65500. That means to have traffic pass through the security group, *you must have an allow rule* or it will be blocked by the default final rule.

Note: SMTP (port 25) is a special case. Depending on your subscription level and when your account was created, outbound SMTP traffic may be blocked. You can make a request to remove this restriction with business justification.

<hr>

## Knowledge check
1. When creating a Windows virtual machine in Azure, which port would you open using the INBOUND PORT RULES in order to allow remote-desktop access? -> RDP (3389).
2. Suppose you have an application running on Windows VM in Azure. What is the best-practice guidance on where the app should store data files? -> Attached data disk.
3. What is the final rule that is applied in every Network Security Group? -> Deny All.

<hr>

## Host a web application with Azure App Service
Azure App Service enables you to build adn host web applications in the programming language of your choice without managing infrastructure.

## Objectives
- Use the Azure portal to create an Azure App Service web app.
- Use developer tools to create the code for a starter web application.
- Deploy your code to Azure App Service

<hr>

## What is Azure App Service?
Azure App Service is a fully managed web application hosting platform. This platform as a service (PaaS) offered by Azure allows you to focus on designing and building your app while Azure takes care of the infrastructure to run and scale your applications.

## Deployment slots
Add **deployment slots** to an Azure Service web app to easily swap the staging deployment slot with the production slot when you are ready.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/host-a-web-app-with-azure-app-service/media/2-deployment-slots.png">

## Integrated Visual Studio publishing and FTP pushing
You can publish your app to Azure with Visual Studio via Web Deploy technology. App Service also supports FTP-based publishing for more traditional workflows.

## Built-in auto scale support (automatic scale-out based on real-world load)
The ability to scale up/down or scale out is baked into the web app. Depending on the web app's usage, you can scale your app up/down by increasing/decreasing the resources of the underlying machine that is hosting your web app. Resoures can be number of cores or the amount of RAM available.

Scaling out, on the other hand, is the ability to increase the number of machine instances that are running your web app.

## App Service Plans
An App Service plan is a set of virtual server resources that run in App Service apps. A plan's **size** (sometimes referred to as its **sku** or **pricing tier**) determines the performance characteristics of the virtual servers that run the apps assigned to the plan and the App Service features that those apps have access to. Every App Service web app you create must be assigned to a single App Service plan that runs it.

A single App Service plan can host multiple App Service web apps. In most cases, the number of apps you can run on a single plan will be limited by the performance characteristics of the apps and the resource limitations of the plan. 

App Service plans are the unit of billing for App Service. The size of each App Service plan is your subscription, in addition to the bandwidth resources used by the apps deployed to those plans, determines the price that you pay. The number of web apps deployed on your App Service plan has no effect on your bill.

You can use any of the available Azure management tools to create an App Service plan. When you create a web app via the Azure portal, the wizard will help you create a new plan at the same time if you don't already have one.

<hr>

## Manual deployment
There are a few options that you can use to manually push your code to Azure:
- **Git**: App Service web apps feature a Git URL that you can add as a remote repository. Pushing to the remote repo will deploy your app.
- **az webapp up**: `webapp up` is a feature of the `az` command-line interface that packages your app and deploys it. Unlike other deployment methods, `az webapp up` can create a new App Service web app for you if you haven't already created one.
- **ZIP deploy**: Use `az webapp deployment source config-zip` to send a ZIP of your local files to App Service. You can also access ZIP deploy via basic HTTP utilities such as `curl`.
- **WAR deploy**: WAR deploy is an App Service deployment mechanism designed for deploying Java web applications using WAR packages. You can access WAR deploy using the Kudu HTTP API located at `http://<your-app-name>.scm.azurewebsites.net/api/wardeploy`. If this fails, try `https://<your-app-name>.scm.azurewebsites.net/api/wardeploy`.
- **Visual Studio**: Visual Studio features an App Service deployment wizard that can walk you through the deployment process.
- **FTP/S**: FTP or FTPS is a traditional way of pushing your code to many hosting environments, including App Service.

<hr>

## Knowledge check
1. True or false: Azure App service can automatically scale your web application to meet traffic demand? -> True.
2. What isn't a valid automated deployment source? -> SharePoint

<hr>

## Protect your virtual machine settings with Azure Automation State Configuration
Create a desired state configuration script that checks that IIS is installed. Onboard VMs for management by Azure Automation. Automatically install IIS on the VMs where that feature is missing.

## Objectives
- Identify the capabilities of Azure Automation State Configuration
- Learn how to onboard VMs for management by Azure Automation
- Automatically update VMs to maintain a desired state configuration (DSC).

<hr>

## What is Azure Automation State Configuration?
You use Azure Automation State Configuration to make sure that the virtual machines (VMs) in a cluster are in a consistent state, with the same software installed and the same configurations.

Azure Automation State Configuration is an Azure service built on PowerShell. It enables you to consistently deploy, reliably monitor, and automatically update the desired state of all your resources. Azure Automation provides tools to define configurations and apply them to real and virtual machines.

## Why use Azure Automation State Configuration?
Manually maintaining a correct and consistent configuration for the servers that run your services can be difficult and error prone. Azure Automation State Configuration uses PowerShell DSC to help address these challenges. It centrally manages your DSC artifacts and the DSC process.

Azure Automation State configuration has a built-in pull server. You can target nodes to automatically receive configurations from this pull server, conform to the desired state, and report back on their compliance. You can target virtual or physical Windows or Linux machines, in the cloud or on-premises.

You can use Azure Monitor logs to review the compliance of your nodes by configuring Azure Automation State Configuration to send this data.

## What is PowerShell DSC?
PowerShell DSC is a declarative management platform that Azure Automation State Configuration uses to configure, deploy, and control systems. A declarative programming language separates intent from execution. You specify the desired state and let DSC do the work to get there. You don't have to know how to implement or deploy a feature when a DSC resource is available. Instead, you focus on the structure of your deployment.

If you're already using PowerShell, you might wonder why you need DSC. Consider the following example. When you want to create a share on a Windows server, you might use this PowerShell command:

        # Create a file share
        New-SmbShare -Name MyFileShare -Path C:\Shared -FullAccess User1 -ReadAccess User2

This script is straightforward and easy to understand. However, if you use this script in production, you'll encounter several problems. Consider what might happen if the script runs multiple times or if `User2` already has full access rather than read only access.

This approach isn't *idempotent*. To achieve idempotence in PowerShell, you need to add logic and error handling. If the file share doesn't exist, you create it. If the share does exist, there's no need to create it. If `User2` exists but doesn't have read access, you add read access.

You PowerShell script would look something like:

        $shareExists = $false
        $smbShare = Get-SmbShare -Name $Name -ErrorAction SilentlyContinue
        if($smbShare -ne $null)
        {
                Write-Verbose -Message "Share with name $Name exists"
                $shareExists = $true
        }

        if ($shareExists -eq $false)
        {
                Write-Verbose "Creating share $Name to ensure it is Present"
                New-SmbShare @psboundparameters
        }
        else
        {
                # Need to call either Set-SmbShare or *ShareAccess cmdlets
                if ($psboundparameters.ContainsKey("ChangeAccess"))
                {
                        #...etc., etc., etc
                }
        }

Other special cases you haven't considered might come to light only when problems arise. DSC handles unexpected cases automatically. With DSC, you describe the result rather than the process to achieve the result.

The following DSC code snippet shows an example:
        Configuration Create_Share
        {
                Import-DscResource -Module xSmbShare
                # A node describes the VM to be configured

                Node $NodeName
                {
                        # A node definition contains one or more resource blocks
                        # A resource block describes the resource to be configured on the node
                        xSmbShare MySMBShare
                        {
                                Ensure      = "Present"
                                Name        = "MyFileShare"
                                Path        = "C:\Shared"
                                ReadAccess  = "User1"
                                FullAccess  = "User2"
                                Description = "This is an updated description for this share"
                        }
                }
        }

The example above uses the `xSmbShare` module, which tells DSC how to check the state of the file share. The DSC Resource Kit has more than 80 resource modules, including one for installing an IIS site. You'll find a link to the DSC Resource Kit at the end of the module.

You'll learn more about the structure of the PowerShell DSC code in the next unit.

## What is the LCM?
The local configuration manager (LCM) is a component of the Windows Management Framework (WMF) on a Windows operating system. The LCM is responsible for updating the state of a node, like a VM, to match the desired state. Every time the LCM runs, it completes the following steps:

1. **Get**: Get the current state of the node.
2. **Test**: Compare the current state of a node against the desired state by using a compiled DSC script (.mof file).
3. **Set**: Update the node to match the desired state described in teh .mof file.

You configure the LCM when you register a VM with Azure Automation.

## Push and pull architectures in DSC
The LCM on each node can operate in two modes.

- **Push mode**: An administrator manually sends, or pushes, the configuration to one or more nodes. The LCM makes sure that the state on each node matches what the configuration specifies.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/protect-vm-settings-with-dsc/media/2-push.png">

- **Pull mode**: A pull server holds the configuration information. The LCM on each node polls the pull server at regular intervals, by default every 15 minutes, to get the latest configuration details. These requests are denoted as step 1 in the following diagram. In step 2, the pull server sends the details about any configuration changes back to each node.

In pull mode, each node has to be registered with the pull service.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/protect-vm-settings-with-dsc/media/2-pull.png">

Both modes have advantages:
- Push mode is easy to set up. It doesn't need its own dedicated infrastructure, and it can run on a laptop. Push mode is helpful to test the functionality of DSC. You could also use push mode to get a newly imaged machien to the baseline desired state.
- Pull mode is useful in an enterprise deployment that spans a large number of machines. The LCM regularly polls the pull server and makes sure the nodes are in the desired state. If an external tool or team applies hotfixes that result in configuration drift on individual machines, those machiens are quickly brought back in line with the configuration you've set. This process can help you achieve a state of continuous compliance for your security and regulatory obligations.

## Knowledge check
1. What is Azure State Configuration? -> A service used to write, manage, and compile PowerShell Desired State Configurations (DSC), import DSC resources, and assign configurations to target nodes.
2. A PowerShell DSC script -> Describes the desired state
3. Why should you use pull mode instead of push mode for DSC? -> Pull mode is easy to set up and doesn't need its own dedicated infrastructure.

<hr>

## Use PowerShell DSC to achieve a desired state
Windows Server has a set of built-in PowerShell DSC resources. You can view these resources by running the `Get-DSCResource` PowerShell cmdlet.

        Get-DscResource | select Name,Module,Properties

For more complex resources, like Active Directory integration, use the DSC Resource Kit, which is updated monthly. The resource you want to configure must already be a part of the VM or part of the VM image. Otherwise, the job will fail to compile and run.

## Anatomy of a DSC code block
A DSC code block contains four sections. 

        Configuration MyDscConfiguration {              ##1
                Node "localhost" {                          ##2
                        WindowsFeature MyFeatureInstance {      ##3
                                Ensure = 'Present'
                                Name = 'Web-Server'
                        }
                }
        }
        MyDscConfiguration -OutputPath C:\temp\         ##4

1. **Configuration**: The configuration block is the outermost script block. It starts with the `Configuration` keyword, and you provide a name. Here, the name of the configuration is `MyDscConfiguration`.

The configuration block describes the desired configuration. Think of a configuration block like a function, except it contains a description of the resources to install rather than the code to install them.

Like a PowerShell function, a configuration block can take parameters. For example, you could parameterize the node name.

        Configuration MyDscConfiguration {
        param
        (
                [string] $ComputerName='localhost'
        )

        Node $ComputerName {
        ...
        }

2. **Node**: You can have one or more node blocks. The node block determines the names of .mof files that are generated when you compile the configuration. For example, the node named `localhost` generates only one *localhost.mof* file. But you can send that .mof file to any server. You generate multiple .mof files when you use multiple node names.

Use the array notation in the node block to target multiple hosts:

        Node @('WEBSERVER1', 'WEBSERVER2', 'WEBSERVER3')

3. **Resource**: One ore more resource blocks can specify the resources to configure. In this case, a single resource block references the `WindowsFeature` resource. The `WindowsFeature` resource here ensure that the Windows feature `Web-Server` is installed.

4. **MyDscConfiguration**: This call invokes the `MyDscConfiguration` block. It's like running a function. When you run a configuration block, it's compiled into a Manged Object Format (MOF) document. MOF is a compiled language created by Desktop Management Task Force, and it's based on interface definition language.

For every node listed in the DSC script, a .mof file is created in the folder you specified with the `-OutputPath` parameter.

## Configuration data in a DSC script
In a configuration data block, you can provide data that the configuration process might need. You apply this data to named nodes, or you apply it globally across all nodes.

A configuration data block is a named block that contains an array of nodes. The array must be named `AllNodes`. Inside the `AllNodes` array, you specify the data for a node by using the `NodeName` variable.

Using the previous scenario, let's say that on the web server that's installed on each node, you want to set the `SiteName` property to different values. You could define a configuration data block like this:

        $datablock =
        @{
                AllNodes =
                @(
                        @{
                        NodeName = "WEBSERVER1"
                        SiteName = "WEBSERVER1-Site"
                        },
                        @{
                        NodeName = "WEBSERVER2"
                        SiteName = "WEBSERVER2-Site"
                        },
                        @{
                        NodeName = "WEBSERVER3"
                        SiteName = "WEBSERVER3-Site"
                        }
                );
        }

If you want to set a property to the same value in each node, in the `AllNodes` array, specify `NodeName ="*"`.

## Secure credentials in DSC script
A DSC script might require credential information for the configuration process. Avoid putting a credential in plaintext in your source code management tool. Instead, DSC configurations in Azure Automation can reference credentials stored in a `PSCredential` type. Before running the script, get the credentials for the user, use the credentials to create a new `PSCredential` object, and pass this object into the script as a parameter.

Credentials aren't encrypted in .mof files by default. They're exposed as plaintext. To encrypt credentials, use a certificate in your configuration data. The certificate's private key need to be on the node on which you want to apply the configuration. Certificates are configured through the node's LCM.

Starting in PowerShell 5.1, .mof files on the node are encrypted at rest. In transit, all credentials are encrypted through WinRM.

## Push the configuration to a node
After you create a compiled .mof file for a configuration, you can push it to a node by running the `Start-DscConfiguration` cmdlet. If you add the path to the directory, it applies any .mof file it finds in that directory to the node:

        Start-DscConfiguration -path D:\

This step corresponds to *push mode*, which you learned about the previous unit.

## Pull the configuration for nodes
If you have hundreds of VMs on Azure, pull mode is more appropriate than push mode.

You can configure an Azure Automation account to act as a pull service. Just upload the configuration to the Automation account. Then register your VMs with this account.

Before you compile your configuration, import into your Automation account any PowerShell modules the DSC process needs. These modules define how to complete the task to achieve the desired state.

<hr>

## Configure virtual machines
You will learn to configure virtual machines including sizing, storage, and connections.

## Objectives
- Create a virtual machine planning checklist.
- Determine virtual machine locations and pricing models.
- Determine the correct virtual machine size.
- Configure virtual machine storage.

<hr>

## Review cloud services responsibilities
VMs are part of the Infrastructure as a Service (IaaS) offering. IaaS is an instant computing infrastructure, provisioned and managed over the Internet. Quickly scale up or down with demand and pay only for what you use.

<hr>

## Plan virtual machines
Provisioning VMs to Azure requires planning.
- Start with the network
- Name the VM
- Decide the location for the VM
- Determine the size of the VM
- Understanding the pricing model
- Storage for the VM
- Select and operating system

## Start with the network
Virtual networks (VNets) are used in Azure to provide private connectivity between Azure Virtual Machines and other Azure services. VMs and services that are part of the same virtual network can access one another. By default, services outside the virtual network cannot connect to services within the virtual network. You can, however, configure the network to allow access to the external service, including your on-premises servers.

This latter point is why you should spend some time thinking about your network configuration. Network addresses and subnets are not trivial to change once you have them set up, and if you plan to connect your private company network to the Azure services, you will want to make sure you consider the topology before putting any VMs into place.

## Name the VM
The VM name is used as the computer name, which is configured as part of the operating system. You can specify a name of up to 15 characters on Windows VM and 64 characters on a Linux VM.

This name also defines a manageable Azure resource, and it's not trivial to change later. A good convention is to include the following information in the name:
- Environment: Environment of the resource (dev, prod, QA)
- Location: Identifies the region into which the resource is deployed (uw - US West, ue (US East))
- Instance: For resources that have more than one named instance (01, 02)
- Product or Service: Identifies the product, application, or service that the resource supports
- Role: The role of the associated resource (sql, web, messaging)

For example, `devusc-webvm01` might represent the first development web server hosted in the US South Central location.

## Bastion connections
The Azure Bastion service is a new fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly in the Azure portal over SSL. When you connect via Azure Bastion, your VMs do not need a public IP address.

<hr>

## Knowledge check
1. Which workload option should be selected to run a network appliance on a virtual machine? -> Compute optimized
2. An organization has a security policy that prohibits exposing SSH ports to the outside world. What is the best way to connect to the Azure Linux VMs and install software? -> Configure the Bastion service.
3. What is the effect of the default network security settings for a new VM? -> Outbound request is allowed. Inbound traffic is only allowed from within the virtual network.

<hr>

## Configure network security groups
You will learn how to implement network security groups and ensure network security group rules are correctly applied.

## Objectives
- Determine when to use network security groups.
- Implement network security group rules.
- Evaluate network security group effective rules.

<hr>

## Implement network security groups
You can limit network traffic to resources in a virtual network using a network security group (NSG). A network security group contains a list of security rules that allow or deny inbound or outbound network traffic. An NSG can be associated to a subnet or a network interface. A NSG can be associated multiple times.

## Subnets
You can assign NSGs to subnets and create protected screened subnets (also called a DMZ). These NSGs can restrict traffic flow to all the machines that reside within that subnet. Each subnet can have zero, or one, associated NSGs.

## Network interfaces
You can assign NSGs to a NIC so that all the traffic that flows through that NIC is controlled by NSG rules. Each network interface that exists in a subnet can have zero, or one, associated network security groups.

## Associations
When you create an NSG the Overview blade provides information about the NSG such as, associated subnets, associated network interfaces, and security rules.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-network-security-groups/media/network-security-groups-1ebf7bed.png">

<hr>

## Application security group constraints
- There are limits to the number of ASGs you can have in a subscription, in addition to other limits related to ASGs.
- You can specify one ASG as the source and destination in a security role. You cannot specify multiple ASGs in the source or destination.
- All network interfaces assigned to an ASG must exist in the same virtual network that the first nework interface assigned to the ASG is in. For example, if the first network interface assigned to an ASG named AsgWeb is in the virtual network named VNet1, then all subsequent network interfaces assigned to ASGWeb must exist in VNet1. You cannot add network interfaces from different virtual networks to the same ASG.
- If you specify an ASG as the source and destination in a security rule, the network interfaces in both ASGs must exist in the same virtual network. For example, if AsgLogic contained network interfaces from VNet1, and AsgDb contained network interfaces from VNet2, you could not assign AsgLogic as the source and AsgDb as the destination in a rule. All network interfaces for both the source and destination ASGs need to exist in the same virtual network.

<hr>

## Knowledge check
1. The infrastructure team has two NSG security rules for inbound traffic to the backend web servers. There is an allow rule with a priority rule of 200. And, there is a deny rule with a priority of 150. Which rule will take precedence? -> The deny rule takes precedence (lower the number the higher the priority)
2. What is one of the default inbound security rules? -> Allow inbound coming from any VM to any other VM within the virtual network.
3. Which of the following is a valid service tag for network security group rules? -> VirtualNetwork. (Service tags represent a group of IP addresses. Other services tags are Internet, SQL, Storage, AzureLoadBalancer, and AzureTrafficManager).

<hr>

## Configure Azure Firewall
You will learn how to configure the Azure Firewall including firewall rules.

## Objectives
- Determine when to use Azure Firewall.
- Implement Azure Firewall including firewall rules.

<hr>

## Determine Azure Firewall uses
Azure Firewall is a managed, cloud-based network security service that protects your Azure Virtual Network resources. It's a fully stateful firewall as a service with built-in high availability and unrestricted cloud scalability. You can centrally create, enforce, and log application and network connectivity policies across subscriptions and virtual networks.

Azure Firewall uses a static public IP address for your network resources allowing outsite firewalls to identify traffic originating from your virtual network. The service is fully integarated with Azure Monitor for logging and analytics.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-firewall/media/firewall-threat-8a2c65f9.png">

## Azure Firewall feaures
- **Built-in high availability**: Additional load balancers aren't required. There's nothing you need to configure.
- **Availability Zones**: Azure Firewall can be configured during deployment to span multiple Availability Zones for increased availability.
- **Unrestricted cloud scalability**: Azure Firewall can scale up as much as you need to accommodate changing network traffic flows, so you don't need to budget for your peak traffic.
- **Application FQND filtering rules**: You can limit outbound HTTP/S traffic or Azure SQL traffic to a specified list of fully qualified domain names (FQDN) including wildcards.
- **Network traffic filtering rules**: You can centrally create allow or deny network filtering rules by source and destination IP address, port, and protocol. Azure Firewall is fully stateful, so it can distinguish legitimate packets for different types of connections. Rules are enforced and logged across multiple subscriptions and virtual networks.
- **Threat intelligence**: Threat intelligence-based filtering can be enabled for your firewall to alert and deny traffic from/to known malicious IP addresses and domains. The IP addresses and domains are sourced from the Microsoft Threat Intelligence feed.
- **Multiple public IP addresses**: You can assocaite multiple public IP addresses with your firewall.

<hr>

## Create Azure Firewalls
It's recommended to use a hub-spoke network topology when deploying a firewall.

- The *hub* is a virtual network in Azure that acts as a central point of connectivity to your on-premises network.
- The *spokes* are virtual networks that peer with the hub, and can be used to isolate workloads.
- Traffic flows between the on-premises datacenter and the hub through an ExpressRoute or VPN gateway connection.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-firewall/media/firewall-tasks-7b6dbe0f.png">

The benefits of this topology include:
- Cost savings by centralizing services that can be shared by multiple workloads, such as network virtual appliances (NVAs) and DNS servers, in a single location.
- Overcome subscriptions limits by peering virtual networks from different subscriptions to the central hub.
- Separation of concerns between central IT (SecOps, InfraOps) and workloads (DevOps).

Typical uses for a hub-spoke network architecture include:
- Workloads in different environments that require shared services. For example, development and testing environments that require DNS. Shared services are placed in a hub virtual network. Each environment is deployed to a spoke to maintain isolation.
- Workloads that don't require connectivity to each other, but require access to shared services.
- Enterprises that require central control over security aspects. For example, a firewall in the hub and workloads in each spoke.

<hr>

## Create firewall rules
There are three kinds of rules that you can configure in the Azure Firewall. Remember, by default, Azure Firewall blocks all traffic, unless you enable it.

## NAT rules
You can configure Azure Firewall Destination Network Address Translation (DNAT) to translate and filter inbound traffic to your subnets. Each rule in the NAT rule collection is used to translate your firewall public IP and port to a private IP and port. Scenarios where NAT rules might be helpful are publishing SSH, RDP, or non-HTTP/S applications to the internet. A NAT rule that routes traffic must be accompanied by a matching network rule to allow traffic. Configuration settings include:

- **Name**: A label for the rule.
- **Protocol**: TCP or UDP.
- **Source Address**: (Internet), a specific Internet address, or a CIDR block.
- **Destination Address**: The external address of the firewall that the rule will inspect.
- **Destination Ports**: The TCP or UDP ports that the rule will listen to on the external IP address of the firewall.
- **Translated Address**: The IP address of the service (virtual machine, internal load balancer, and so on) that privately hosts or presents the service.
- **Translated Port**: The port that the inbound traffic will be routed to by the Azure Firewall.

## Network rules
Any non-HTTP/S traffic that will be allowed to flow through the firwall must have a network rule. For example, if resources in one subnet must communicate with resources in another subnet, then you would configure a network rule from the source to the destination. Configuration settings include:

- **Name**: A friendly label for the rule.
- **Protocol**: TCP, UDP, ICMP (ping and traceroute) or Any.
- **Source Address**: The address or CIDR block of the source.
- **Destination Addresses**: The addresses or CIDR of the destination(s).
- **Destination Ports**: The destination port of the traffic.

## Application rules
Application rules define fully qualified domain names (FQDNs) that can be accessed from a subnet. For example, specify the Windows Update network traffic through the firewall. Configuration settings include:

- **Name**: A friendly label for the rule.
- **Source Addresses**: The IP address of the source.
- **Protocol: Port**: HTTP/HTTPS and the port that the web server is listening on.
- **Target FQDNs**: The domain name of the service, such as www.consoso.com. Wildcards can be used. An FQDN tag represents a group of FQDNs associated with well known Microsoft services. Example FQDNs tags include Windows Update, App Service Environment, and Azure Backup.

## Rule processing
When a packet is being inspected to determine if it is allowed or not, the rules are processed in this order:
1. Network Rules
2. Application Rules (network and application)

<hr>

## Knowledge check
1. Suppose a company wants to allow access to an Azure SQL Database instance using a FQDN address. What network rule type should they use to configure Azure firwall? -> Application (defines FQDN)
2. Suppose a company wants to allow external users to access an Azure virtual server with a remote desktop connection. What item would be implemented on the Azure firewall to allow these connections? ->  Destination network address translation
3. What describes the Azure firewall IP address? -> The firewall has a statically assigned public IP address.

<hr>

## Configure Azure DNS
You will learn how to configure Azure DNS including custom domain names and record sets.

## Objectives
- Identify features and usage cases for domains, custom domains, and private zones.
- Verify custom domain names using DNS records.
- Implement DNS zones, DNS delegation, and DNS record sets.

Azure DNS enables you to host your DNS records for your domains on Azure infrastructure. With Azure DNS, you can use the same credentials, APIs, tools, and billing as your other Azure services.

You company obtains a custom domain name for a new website. You need to use Azure DNS to manage this domain.

<hr>

## Custom domain name
The initial domain name can't be changed or deleted. You can however add a routable custom domain name you control. A custom domain name simplifies the user sign-on experience. Users can use credentials they are familiar with. For example, contosogold.onmicrosoft.com, could be assigned to contosogold.com

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-dns/media/custom-domain-names-8dae9b45.png">

## Practical information about domain names
- You must be a global administrator to perform domain management tasks. The global administrator is the user who created the subscription.
- Domain names in Azure AD are globally unique. When on Azure AD directory has verified a domain name, other directories can't use that name.
- Before a custom domain name can be used by Azure AD, the custom domain name must be added to your directory and verified.

<hr>

## Verify custom domain names
When an administrator adds a custom domain name to an Azure AD, it is initially in an unverified state. Azure AD won't allow any directory resources to use an unverified domain name. Only one directory can use a domain name, the organization that owns the domain name.

After adding the custom domain name, you must verify ownership of the domain name. Verification is performed by adding a DNS record. The DNS record could be MX or TXT. Once the DNS record is added, Azure will query the DNS domain for the presence of the record. This could take several minutes or several hours. When Azure verifies the presence of the DNS record, it will then add the domain name to the subscription.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-dns/media/verify-custom-domain-2a02fdb3.png">

<hr>

## Create Azure DNS zones
Azure DNS provides a reliable, secure DNS service to manage and resolve domain names in a virtual network without needing to add a custom DSN solution.

A DNS zone hosts the DNS records for the domain. So, to start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name. Each DNS record for your domain is then created inside this DNS zone.

From the Azure portal, you can easily add a DNS zone. Information for the DNS zone includes name, number of records, resource group, location, subscription, and name servers.

## Considerations
- The name of the zone must be unique within the resource group, and the zone must not exist already.
- The same zone name can be reused in a different resource group or a different Azure subscription.
- Where multiple zones share the same name, each instance is assigned different name server addresses.
- Root/Parent domain is registered at the registrar and pointed to Azure NS.
- Child domains are registered in Azure DNS directly.

Note: You do not have to own a domain name to create a DNS zone with that domain name in Azure DNS. However, you do need to own the domain to configure the domain.

<hr>

## Delegate DNS domains
To delegate your domain to Azure DNS, you first need to know the server names for your zone. Each time a DNS zone is created Azure DNS allocates name servers from a pool. Once the Name Servers are assigned, Azure DNS automatically creates authoritative NS record in your zone.

Note: When you copy each naem server address, make sure you copy the trailing period at the end of the address. The trailing period indicates the end of a fully qualified domain name.

Once the DNS zone is created, and you have the name servers, you need to update the parent domain. Each registrar has their own DNS management tools to change the name server records for a domain. In the registrar's DNS management page, edit the NS records and replace the NS record with the ones Azure DNS created.

Note: When delegating a domain to Azure DNS, you must use the name server names provided by Azure DNS. You should alwasy use all four name server names, regardless of the name of your domain.

## Child domains
If you want to set up a separate child zone, you can delegate a subdomain in Azure DNS. For example, after configuring contoso.com in Azure DNS, you could configure a separate child zone for partners.contoso.com.

Setting up a subdomain follows the same process as typical delegation. The only difference is that NS records must be created in the parent zone contoso.com in Azure DNS, rather than in the domain registrar.

Note: The parent and child zones can be in the same ore different resource group. Notice that the record set name in the parent zone matches the child zone name, in this case *partners**.

<hr>

## Add DNS record sets
It's important to understand the difference between DNS record sets and individual DNS records. A record set is a collection of records in a zone that have the same name and are the same type.

A record set cannot contain two identifical records. Empty record sets (with zero records) can be created, but do not appear on the Azure DNS name servers. Record sets of type CNAME can contain one record at most.

The **Add record set** page will change depending on the type of record you select. For an A record, you will need the TTL (Time to Live) and IP address. The time to live, or TTL, specifies how long each record is cached by clients before being requiried.

<hr>

## Plan for private DNS zones
When using private DNS zones, you can use your own custom domain names rather than the Azure-provided names. Using custom domain names helps you to tailor your virtual network architecture to best suite your organization's needs. It provides name resolution for virtual machines (VMs) within a virtual network and between virtual networks. Additionally, you can configure zones names with a split-horizon view, which allows a private and a public DNS zone to share the name.

The DNS records for the private zone are not viewable or retrievable. But, the DNS records are registered and will resolve successfully.

## Azure private DNS benefits
- **Removes the need for custom DNS solutions**: You can perform DNS management by using the native Azure infrastructure. This removes the burden of creating an managing custom DNS solutions.
- **Use all common DNS records types**: Azure DNS supports A, AAAA, CNAME, MX, PTR, SOA, SRV, and TXT records.
- **Automatic hostname record management**: Along with hosting your custom DNS records, Azure automatically maintains hostname records for the VMs in the specified virtual networks. In this scenario, you can optimize the domain names you use without needing to create custom DNS solutions or modify applications.
- **Hostname resolution between virtual networks**: Unlike Azure-provided host names, private DNS zones can be shared between virtual networks. This capability simplifies cross-network and service-discovery scenarios, such as virtual network peering.
- **Familiar tools and user experience**: To reduce the learning curve, this new offering uses well-established Azure DNS tools (PowerShell, Azure Resource Manager templates, and the REST API).
- **Split-horizon DNS support**: You can create zones with the same name that resolve to different answers from within a virtual network and from the public internet.
- **Available in all Azure regions**: The Azure DNS priate zones feature is available in all Azure regions in the Azure public cloud.

<hr>

## Knowledge check
1. Which of the following best summarizes the purpose of Azure DNS? -> Manages and hosts the registered domain and associated records.
2. What type of DNS record should be created to map one or more IP addresses agains a single domain? -> A or AAAA
3. Azure Private DNS allows which of the following? -> Lets organizations manage and resolve domain names in a virtual network without adding a custom DNS solution.

<hr>

## Configure virtual network peering
You will learn to configure a VNet peering connection and address transit and connectivity concerns.

## Objectives
- Identify usage cases and product features of virtual network peering.
- Configure gateway transit, connectivity, and service chaining.

<hr>

## Determine virtual network peering uses
Perhaps the simplest and quickest way to connect your VNets is to use VNet peering. Virtual network peering enables you to seamlessly connect two Azure virtual networks. Once peered, the virtual networks appear as one, for connectivity purposes. There are two types of VNet peering.

- **Regional VNet peering** connects Azure virtual networks in the same region.
- **Global VNet peering** connects Azure virtual networks in different regions. When creating a global peering, the peered virtual networks can exist in any Azure public cloud region or China cloud regions, but not in Government cloud regions. You can only peer virtual networks in the same region in Azure Government cloud regions.

## Benefits of virtual network peering
The benefits of using local or global virtual network peering, include:

- **Private**: Network traffic between peered virtual networks is private. Traffic between the virtual networks is kept on the Microsoft backbone network. No public internet, gateways, or encryption is required in the communication between the virtual networks.
- **Performance**: A low-latency, high-bandwidth connection between resources in different virutal networks.
- **Communication**: The ability for resources in one virtual network to communicate with resources in a different virtual network, once the virtual networks are peered.
- **Seamless**: The ability to transfer data across Azure subscriptions, deployment models, and across Azure regions.
- **No disruption**: No downtime to resources in either virtual network when creating the peering, or after the peering is created.

<hr>

## Determine gateway transit and connectivity
When virtual networks are peered, you configure a VPN gateway in the peered virtual network as a transit point. In this case, a peered virtual network uses the remote gateway to gain access to other resources. A virtual network can have only one gateway. Gateway transit is supported for both VNet Peering and Global VNet Peering.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-vnet-peering/media/gateway-transit-173a51a0.png">

When you Allow Gateway Transit the virtual network can communicate to resources outside the peering. For example, the subnet gateway could:
- Use a site-to-site VPN to connect to an on-premises network.
- Use a VNet-to-VNet connection to another virtual network.
- Use a point-to-site VPN to connect to a client.

In these scenarios, gateway transit allows peered virtual networks to share the gateway and get access to resources. This means you do not need to deploy a VPN gateway in the peer virtual network.

Note: Network security groups can be applied in either virtual network to block access to other virtual networks or subnets. When configuring virtual network peering, you can either open or close the network security group rules between the virtual networks.

<hr>

## Create virtual network peering
Here are the steps to configure VNet peering. Notice you will need two virtual networks. To test the peering, you will need a VM in each network. Initially, the VMs will not be able to communicate, but after configuration the communication will work. The step that is new is configuring the peering of the virtual networks.

1. Create two virtual networks.
2. **Peer the virtual networks**.
3. Create virtual machines in each virtual network.
4. Test the communciation between the virtual machines.

To configure the peering use the **Add peering** page. There are only a few optional configuration parameters to consider.

Note: When you add a peering on one virtual network, the second virtual network configuration is automatically added.

<hr>

## Determine service chaining uses
VNet Peering is nontransitive. When you establish VNet peering between VNet1 and VNet2 and between VNet2 and VNet3, VNet peering capabilities do not apply between VNet1 and VNet3. However, you can configure user-defined routes and service chaining to provide the transitivity. This allows you to:
- Implement a multi-level hub and spoke architecture.
- Overcome the limit on the number of VNet peerings per virtual network.

## Hub and spoke architecture
When you deploy hub-and-spoke networks, the hub virtual network can host infrastructure components like the network virtual appliance or VPN gateway. All the spoke virtual networks can then peer with the hub virtual network. Traffic can flow through network virtual appliances or VPN gateways in the hub virtual network.

## User-defined routes and service chaining
Virtual network peering enables the next hop in a user-defined route to be the IP address of a VM in the peered virtual network, or a VPN gateway.

Service chaining lets you define user routes. These routes direct traffic from one virtual network to a virtual appliance, or virtual network gateway.

## Checking connectivity
You can check the status of VNet peering.

- **Initiated**: When you create the peering to the second virtual network form the first virtual network, the peering status is Initiated.
- **Connected**: When you create the peering from the second virtual network to the first virtual network, its peering status is Connected. When you view the peering status for the first virtual network, you see its status changed from Initiated to Connected. The peering is not successfully established until the peering status for both virtual network peerings is Connected.

<hr>

## Knowledge check
1. Virtual network peering is successfully established when the peering status of both virtual network peerings shows which status? -> Connected
2. Which of the following allows peered virtual networks to share the gateway and get access to resources? -> Gateway transit
3. Which of the following best describes virtual network peering? -> Traffic between the virtual networks is kept on the Microsoft backbone network.

<hr>

## Summary
Virtual network peering connects virtual networks in a hub and spoke topology. Virtual network peering is cost-effective and easy to configure.

<hr>

## Configure VPN Gateway
You will learn how to create VPN gateways and securely connect your company sites to Azure.

## Objectives
- Identify features and usage cases for VPN gateways.
- Implement high availability scenarios.
- Configure site-to-site VPN connections using a VPN gateway.

<hr>

## Determine VPN gateway uses
A VPN gateway is a specific type of virtual network gateway that is used to send encrypted traffic between an Azure virtual network and an on-premises location over the public internet. You can also use a VPN gateway to send encrypted traffic between Azure virtual networks over the Microsoft network.

Each virtual network can have only one VPN gateway. However, you can create multiple connections to the same VPN gateway. When you create multiple connections to the same VPN gateway, all VPN tunnels share the available gateway bandwidth.

- **Site-to-site** connections connect on-premises datacenters to Azure virtual networks
- **VNet-to-VNet** connections connect Azure virtual networks (custom)
- **Point-to-site (User VPN)** cnnections connect individual devices to Azure virtual networks

A virtual network gateway is composed of two or more VMs are that are deployed to a specific subnet you create called the gateway subnet. Virtual network gateway VMs contain routing tables and run specific gateway services. These VMs are created when you create the virtual network gateway. You can't configure the VMs that are part of the virtual network gateway.

VPN gateways can be deployed in Azure Availability Zones. Availability zones bring resiliency, scalability, and higher availability to virtual network gateways. Availability Zones physically and logically separates gateways within a region while protecting your on-premises network connectivity to Azure form zone-level failures.

Note: Creating a virtual network gateway can take up to 45 minutes to complete.

<hr>

## Create site-to-site connections
Here are the high-level steps to create a VNet-to-VNet connection. The on-premises part is only needed when you are configuring Site-to-Site.

**Create VNets and subnets**: Contact your on-premises network administrator to reserve an IP address range for this virtual network.

**Specify the DNS server (optional)**: DNS is not required to create a Site-to-Site connection. However, if you need name resolution for resources that are deployed to your virtual network, you should specify a DNS server in the virtual network configuration.

<hr>

## Create the gateway subnet
Before creating a virtual network gateway for your virtual network, you first need to create the gateway subnet. The gateway subnet contains the IP addresses that are used by the virtual network gateway. If possible, it's best to create a gateway subnet by using a CIDR block of /28 or /27 to provide enough IP addressses to accommodate future configuration requirements.

When you create your gateway subnet, gateway VMs are deployed to the gateway subnet and configured with the required VPN gateway settings. Never deploy the other resources (for example, additional VMs) to the gateway subnet. The gateway subnet must be named *GatewaySubnet*.

Deploy a gateway in your virtual network by adding a gateway subnet.

<hr>

## Create the VPN gateway
The VPN gateway settings that you chose are critical to creating a successful connection.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-vpn-gateway/media/create-virtual-gateways-fc2fd022.png">

<hr>

## Determine the VPN gateway type
When you create the virtual network gateway, you must specify a VPN type. The VPN type that you choose depends on the connection topology that you want to create. For example, a Point-to-Site (P2S) connection requires a Route-based VPN type.

A VPN type can also depend on the hardware that you are using. Site-to-Site (S2S) configurations require a VPN device. Some VPN devices only support a certain VPN type.

- **Route-based VPNs**: Route-based VPNs use *routes* in the IP forwarding or routing table to direct packets into their corresponding tunnel interfaces. The tunnel interfaces then encrypt or decrypt the packets in and out of the tunnels. The policy (or traffic selector) for Route-based VPNs are configured as any-to-any (or wild cards).

- **Policy-based VPNs**: Policy-based VPNs encrypt and direct packets through IPsec tunnels based on the IPsec polices configured with the combinations of address prefixes between your on-premises network and the Azure VNet. The policy (or traffic selector) is defined as an access list in the VPN device configuration. When using a Policy-based VPN keep in mind the following limitations:
- Policy-Based VPNs can only be used on the Basic gateway SKU and is not compatible with other gateway SKUs.
- You can have only one tunnel when using a Policy-based VPN.
- You can only use Policy-based VPNs for S2S connections, and only for certain configurations. Most VPN Gateway configurations require a Route-based VPN.

Note: Once a virtual network gateway has been created, you can't change the VPN type.

<hr>

## Create the local network gateway
The local network gateway typically refers to the on-premises location. You give the site a name by which Azure can refer to it, then specify the IP address to FQDN of the on-premises VPN device for the connection. You also specify the IP address prefixes that will be routed through the VPN gateway to the VPN device. The address prefixes you specify are the prefixes located in the on-premises network.

<hr>

## Set up the on-premises VPN gateway
There is a validated list of standard VPN devices that work well with the VPN gateway. This list was created in partnership with device manufacturers like Cisco, Juniper, Ubiquiti, and Barracuda Networks.

When you device is not listed in the validated VPN devices table, the device may still work. Contact your device manufacturer for support and configuration instructions.

To configure your VPN device, you will need:
- **A shared key**: The same shared key that you specify when creating the VPN connection.
- **The public IP address of your VPN gateway**: The IP address can be new or existing.

Note: Depending on the VPN device that you have, you may be able to download a VPN device configuration script.

<hr>

## Create the VPN connection
Once your VPN gateways are created, you can create the connection between them. If your VNets are in the same subscription, you can use the portal.

## Verify the VPN connection
After you have configured all the Site-to-Site components, it is time to verify that everything is working. You can verify the connections either in the portal or by using PowerShell.

<hr>

## Determine high availability scenarios

## Active/standby
Every Azure VPN gateway consists of two instances in an active-standby configuration. For any planned maintenance or unplanned disruption that happens to the active instance, the standby instance would take over (failover) automatically, and resume the S2S VPN or VNet-to-VNet connections. The switch over will cause a brief interruption. For planned maintenance, the connectivity should be restored within 10 to 15 seconds. For unplanned issues, the connection recovery will be longer, about 1 minute to 1 and a half minutes in the worst case. For P2S VPN client connections to the gateway, the P2S connections will be disconnected and the users will need to reconnect from the client machines.

## Active/active
You can now create an Azure VPN gateway in an active-active configuration, where both instances of the gateway VMs will establish S2S VPN tunnels to your on-premises VPN device.

In this configuration, each Azure gateway instance will have a unique public IP address, and each will establish an IPsec/IKE S2S VPN tunnel to your on-premises VPN device specified in your local network gateway and connection. Both VPN tunnels are actually part of the same connection. You will still need to configure your on-premises VPN device to accept or establish two S2S VPN tunnels to those two Azure VPN gateway public IP addresses.

When in active-active configuration, the traffic from your Azure virtual network to your on-premises network will be routed through both tunnels simultaneously. The same TCP or UDP flow will always traverse the same tunnel or path, unless a maintenance event happens on one of the instances.

When a planned maintenance or unplanned event happens to one gateway instance, the IPsec tunnel from that instance to your on-premises VPN device will be disconnected. The corresponding routes on your VPN devices should be removed or withdrawn automatically so that the traffic will be switched over to the other active IPsec tunnel. On the Azure side, the switch over will happen automatically from the affected instance to the active instance.

<hr>

## Knowledge check
1. The company's VPN gateway must work with ExpressRoute. Which VPN type should be used? -> Route-based (Express**Route**)
2. The infrastructure team is connecting two virtual networks. Performance is the key concern. What will most influence performance? -> Selecting a route-based VPN. -> Selecting an appropriate Gateway SKU
3. The infrastructure team needs to configure a site-to-site VPN connection between the on-premises network and the Azure network. The on-premises network uses a Cisco ASA VPN device. What key piece of information will you need to set up the on-premises VPN gateway? -> The shared key provided when the site-to-site VPN connection was created.

<hr>

## Summary
A VPN gateway is a specific type of virtual network gateway that is used to send encrypted traffic between an Azure virtual network and an on-premises location over the public internet. You can also use a VPN gateway to send encrypted traffic between Azure virtual networks over the Microsoft network.

<hr>

## Azure Virtual Network (VNet)
An Azure Virtual Network (VNet) is a representation of your own network in the cloud. It is a logical isolation of the Azure cloud dedicated to your subscription. You can use VNets to provision and manage virtual private networks (VPNs) in Azure and, optionally, link the VNets with other VNets in Azure, or with your on-premises IT infrastructure to create hybrid or cross-premises solutions. Each VNet you create has its own CIDR block and can be linked to other VNets and on-premises networks if the CIDR blocks do not overlap. You also have control of DNS server settings for VNets, and segmentation of the VNet into subnets.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-networks/media/virtual-networks-c016972b.png">

Virtual networks can be used in many ways.

- **Create a dedicated private cloud-only VNet**: Sometimes you don't require a cross-premises configuration for your solution. When you create a VNet, your services and VMs within your VNet can communicate directly and securely with each other in the cloud. You can still configure endpoint connections for the VMs and services that require internet communication, as part of your solution.

- **Securely extend your data center with VNets**: You can build traditional site-to-site (S2S) VPNs to securely scale your datacenter capacity. S2S VPNs use IPSEC to provide a secure connection between your corporate VPN gateway and Azure.

- **Enable hybrid cloud scenarios**: VNets give you the flexibility to support a range of hybrid cloud scenarios. You can securely connect cloud-based applications to any type of on-premises system such as mainframes and Unix systems.

<hr>

## Create subnets
A virtual network can be segmented into one or more subnets. Subnets provide logical divisions within your network. Subnets can help improve security, increase performance, and make it easier to manage the network.

Each subnet contains a range of IP addresses that fall within the virtual network address space. The range must be unique within the address space for the virtual network. The range can't overlap with other subnet address ranges with the virtual network. The address space must be specified by using Classless Inter-Domain Routing (CIDR) notation.

## Considerations
- **Service requirements**: Each service directly deployed into virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of subnets. A service may require, or create, their own subnet, so there must be enough unallocated space for them to do so. For example, if you connect a virtual network to an on-premises network using an Azure VPN Gateway, the virtual network must have a dedicated subnet for the gateway.

- **Virtual appliances**: Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets, or to route traffic between subnets through a network virtual appliance. So, if you require that traffic between resoures in the same virtual network flow through a network virtual appliance (NVA), deploy the resources to different subnets.

- **Service endpoints**: You can limit access to Azure resources such as an Azure storage account or Azure SQL database, to specific subnets with a virtual network service endpoint. Further, you can deny access to the resources from the internet. You may create multiple subnets, and enable a service endpoint for some subnets, but not others.

- **Network security groups**: You can associate zero or one network security group to each subnet in a virtual network. You can associate the same, or a different, network security group to each subnet. Each network security group contains rules, which allow or deny traffic to and from sources and destinations.

- **Private Links**: Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. It simplifies the network architecture and secures the connection between endpoints in Azure by eliminating data exposure to the public internet.

**Note**: There are restrictions on using IP addresses. Azure reserves five IP addresses with each subnet.

<hr>

## Create virtual networks
You can create new virtual networks at any time. You can also add virtual networks when you create a virtual machine. Either way you will need to define the address space, and at least one subnet.

<hr>

## Plan IP addressing
You can assign IP addresses to Azure resources to communicate with other Azure resources, your on-premises network, and the internet. There are two types of Azure IP addresses: public and private IP addresses.

- **Private IP addresses**: Used for communication with an Azure virtual network (VNet), and your on-premises network, when you use a VPN gateway or ExpressRoute circuit to extend your network to Azure.
- **Public IP addresses**: Used for communication with the internet, including Azure public-facing services.

Note: IP Addresses are never managed form within a virtual machine.

## Static vs dynamic addressing
IP addresses can also be statically assigned or dynamically assigned. Static IP addresses do not change and are best for certain situations such as:
- DNS name resolution, where a change in the IP address would require updating host records.
- IP address-based security models that require apps or services to have a static IP address.
- TLS/SSL certificates linked to an IP address.
- Firewall rules that allow or deny traffic using IP address ranges.
- Role-based VMs such as Domain Controllers and DNS servers.

Note: You may decide to separate dynamically and statically assigned IP resources into different subnets.

<hr>

## Create public IP addressing
<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-virtual-networks/media/create-public-ip-address-f07bd67c.png">

**IP Version**: Select IPv4 or IPv6 or Both. Selecting Both will result in two Public IP addresses being created- one IPv4 address and one IPv6 address.

**SKU**: A Public IP's SKU must match the SKU of the Load Balancer with which it is used.

**Name**: The name must be unique within the resource group you select.

**IP address assignment**: There are two types of IP address assignments.

- **Dynamic**: Dynamic addresses are assigned only after a public IP address is associated to an Azure resource, and the resource is started for the first time. Dynamic addresses can change if they're assigned to a resource, such as a virtual machine, and the virtual machine is stopped (deallocated), and then restarted. The address remains the same if a virtual machine is rebooted or stopped (but not deallocated). Dynamic addresses are released when a public IP address resource is dissociated from a resource.

- **Static**: Static addresses are assigned when a public IP address is created. Static addresses aren't released until a public IP address resource is deleted. If the address isn't associated to a resource, you can change the assignment method after the address is created. If the address is associated to a resource, you may not be able to change the assignment method. If you select IPv6 for the IP version, the assignment method must be Dynamic for Basic SKU. Standard SKU addresses are Static for both IPv4 and IPv6.

<hr>

## Associate public IP addresses
A public IP address resource can be associated with virtual machine network interfaces, internet-facing load balancers, VPN gateways, and application gateways.

## Address SKUs
When you create a public IP address, you are given a SKU choice of either **Basic** or **Standard**. Your SKU choice affects the IP assignment method, security, available resources, and redundancy.

<hr>

## Associate private IP addresses
A private IP address resource can be associated with virtual machine network interfaces, internal load balancers, and application gateways. Azure can provide an IP address (dynamic assignment) or you can assign the IP address (static assignment).

A private IP address is allocated from the address range of the virtual network subnet a resource is deployed in.
- **Dynamic**: Azure assigns the next available unassigned or unreserved IP address in the subnet's address range. For example, Azure assigns 10.0.0.10 to a new resource, if addresses 10.0.0.4-10.0.0.9 are already assigned to other resources. Dynamic is the default allocation method.
-- **Static**: You select and assign any unassigned or unreserved IP address in the subnet's address range. For example, if a subnet's address range is 10.0.0.0/16 and addresses 10.0.0.0.4-10.0.0.0.9 are already assigned to other resources, you can assign any address between 10.0.0.10 - 10.0.255.254.

<hr>

## Knowledge check
1. When assigning private IPv4 addresses in a subnet with the address range 10.3.0.0/16, which of the following addresses are available for assignment dynamically? -> 10.3.255.254
2. The infrastructure team has implemented firewall rules to deny traffic based on IP address ranges. Which of the following should be used to meet the requirement? ->  Statically assigned IP addresses.
3. Which of the following resources could have a public IP address? -> Virtual Machine.

<hr>

## Summary and resources
Azure Virtual Network is the fundamental building block for your private network in Azure. VNet enables many types of Azure resources, such as Azure Virtual Machines, to securely communicate with each other, the internet, and on-premises networks. VNet is similar to a traditional network that you'd operate in your own data center, but brings with it additional benefits of Azure's infrastructure such as scale, availability, and isolation.

Azure IP addressing is critical to ensuring resources are accessible. Private IP addresses to communicate between resources in Azure. Public IP addresses enable Azure resources to be accessible directly from the internet.

<hr>

## Determine ExpressRoute uses
Azure ExpressRoute lets you extend your on-premises networks into the Microsoft cloud. The connection is facilitated by a connectivity provider. With ExpressRoute, you can establish connections to Microsoft cloud services, such as Microsoft Azure, Microsoft 365, and CRM Online.

## Make your connections fast, reliable, and private
Use Azure ExpressRoute to create private connections between Azure datacenters and infrastructure on your premises or in a colocation environment. ExpressRoute connections don't go over the public internet, and they offer more reliability, faster speeds, and lower latencies than typical internet connections. In some cases, using ExpressRoute connections to transfer data between on-premises systems and Azure can give you significant cost benefits.

With ExpressRoute, establish connections to Azure at an ExpressRoute location, such as an Exchange provider facility, or directly connect to Azure from your existing WAN network, such as a multiprotocol label switching (MPLS) VPN, provided by a network service provider.

## Use a virtual private cloud for storage, backup, and recovery
ExpressRoute gives you a fast and reliable connection to Azure with bandwidths up to 100 Gbps. The high connection speeds make it excellent for scenarios like periodic data migration, replication for business continuity, and disaster recovery. ExpressRoute is a cost-effective option for transferring large amounts of data, such as datasets for high-performance computing applications, or moving large virtual machines between your dev-test environments.

## Extend and connect your datacenters
Use ExpressRoute to connect and add compute and storage capacity to your existing datacenters. With high throughput and low latencies, Azure will feel like a natural extension to or between your datacenters, so you enjoy the scale and economics of the public cloud without having to compromise on network performance.

## Build hybrid applications
Build applications that span on-premises infrastructure and Azure without compromising privacy or performance. For example, run a corporate intranet application in Azure that authenticates your customers with an on-premises Active Directory service. You serve all of your corporate customers without traffic ever routing through the public internet.

<hr>

## Determine ExpressRoute capabilities
ExpressRoute is supported across all Azure regions and locations. ExpressRoute locations are where Microsoft peers with several service providers. When you are connected to at least one ExpressRoute location within the geopolitical region, you will access Azure services across all regions within a geopolitical region.

## ExpressRoute benefits

**Layer 3 connectivity**
Microsoft uses BGP to exchange routes between your on-premises network, your instances in Azure, and Microsoft public addresses. Multiple BGP sessions are created for different traffic profiles.

**Redundancy**
Each ExpressRoute circuit consists of two connections to two Microsoft Enterprise edge routers (MSEEs) from the connectivity provider/your network edge. Microsoft requires dual BGP connection from the connectivity provider/your network edge - one to each MSEE.

**Connectivity to Microsoft cloud services**
ExpressRoute connections enable access to Microsoft Azure services, Microsoft 365 services, and Microsoft Dynamics 365. Microsoft 365 was created to be accessed securely and reliably via the internet, so ExpressRoute requires Microsoft authorization.

**Connectivity to all regions within a geopolitical region**
You connect to Microsoft in one of our peering locations and access regions within the geopolitical region. For example, if you connect to Microsoft in Amsterdam through ExpressRoute, you'll have access to all Microsoft cloud services hosted in Northern and Western Europe.

**Global connectivity with ExpressRoute premium add-on**
You connect to Microsoft in one of our peering locations and access regions within the geopolitical region. For example, if you connect to Microsoft in Amsterdam through ExpressRoute, you will have access to all Microsoft cloud services hosted in all regions across the world, except national clouds.

**Bandwidth options**
You purchase ExpressRoute circuits for a wide range of bandwidths. Be sure to check with your connectivity provider to determine the bandwidths they support.

**Flexible billing models**
You pick a billing model that works best for you.

<hr>

## Coexist site-to-site and ExpressRoute
ExpressRoute is a direct, private connection from your WAN (not over the public internet) to Microsoft Services, including Azure. Site-to-Site VPN traffic travels encrypted over the public internet. Being able to configure Site-to-Site VPN and ExpressRoute connections for the same virtual network has several advantages.

You configure a Site-to-Site VPN as a secure failover path for ExpressRoute or use Site-to-Site VPNs to connect to sites that are not part of your network, but that are connected through ExpressRoute. Notice this configuration requires two virtual network gateways for the same virtual network, one using the gateway type VPN, and the other using the gateway type ExpressRoute.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-expressroute-virtual-wan/media/coexisting-connections-4af27ce9.png">

## ExpressRoute connection models
You create a connection between your on-premises network and the Microsoft cloud in three different ways. Colocated at a cloud exchange, Point-to-point Ethernet connection, and any-to-any (IPVPN) connection. Connectivity providers offer one or more connectivity models. You work with your connectivity provider to pick the model that works best for you.

## Colocated at a cloud exchange
If you are colocated in a facility with a cloud exchange, you order virtual cross-connections to the Microsoft cloud through the colocation provider's Ethernet exchange. Colocation providers offer either Layer 2 cross-connections, or managed Layer 3 cross-connections between your infrastructure in the colocation facility and the Microsoft cloud.

## Point-to-point Ethernet connections
You connect your on-premises datacenters/offices to the Microsoft cloud through point-to-point Ethernet links. Point-to-point Ethernet providers offer Layer 2 connections, or managed Layer 3 connections between your site and the Microsoft cloud.

## Any-to-any (IPVPN) networks
You integrate your WAN with the Microsoft cloud. IPVPN providers, typically Multiprotocol Label Switching (MPLS) VPN, offer any-to-any connectivity between your branch offices and datacenters. The Microsoft cloud can be interconnected to your WAN to make it appear just like any other branch office. WAN providers typically offer managed Layer 3 connectivity.

Note: Currently, the deployment options for S2S and ExpressRoute coexisting connections are only possible through PowerShell, and not the Azure portal.

<hr>

## Compare intersite connection options
There are many intersite connection choices. This table summarizes how to make a selection.

Connection | Azure Services Supported | Bandwidths | Protocols | Typical Use Case
---------- | ------------------------ | ---------- | --------- | ----------------
Virtual network, point-to-site | Azure IaaS services, Azure VMs | Based on the gateway SKU | Active/passive | Dev, test, and lab environments for cloud services and virtual machines.
Virtual network, site-to-site | Azure IaaS services, Azure VMs | Typically < 1 Gbps aggregate | Active/passive, Active/active | Dev, test, and lab environments. Small-scale production workloads, and virtual machines.
ExpressRoute | Azure IaaS and PaaS services, Microsoft 365 services | 50 Mbps up to 100 Gbps | Active/active | Enterprise-class and mission-critical workloads. Big data solutions

<hr>

## Determine Virtual WAN uses
Azure Virtual WAN is a networking service that provides optimized and automated branch connectivity to, and through, Azure. Azure regions serve as hubs that you can choose to connect your branches to. You use the Azure backbone to connect branches and enjoy branch-to-VNet connectivity. There is a list of partners that support connectivity automation with Azure Virtual WAN VPN.

Azure Virtual WAN brings together many Azure cloud connectivity services such as site-to-site VPN, User VPN (point-to-site), and ExpressRoute into a single operational interface. Connectivity to Azure VNets is established by using virtual network connections. The global transit network architecture is based on a hub-and-spoke connectivity model. The cloud hosted network 'hub' enables transitive connectivity between endpoints that may be distributed across different types of 'spokes'.

## Virtual WAN advantages
- **Integrated connectivity solutions in hub and spoke**: Automate site-to-site configuration and connectivity between on-premises sites and an Azure hub.
- **Automated spoke setup and configuration**: Connect your virtual networks and workloads to the Azure hub seamlessly.
- **Intuitive troubleshooting**: You can see the end-to-end flow within Azure, and then use this information to take required actions.

## Virtual WAN types
There are two types of virtual WANs: Basic and Standard.

**Basic**: is a basic hub type, and allows for Site-to-site VPN only.

**Standard** is a standard hub type, and allows for ExpressRoute, User VPN (P2S), VPN (S2S), Inter-hub, and VNet-to-VNet transiting through the virtual hub.

<hr>

## Knowledge check
1. What is the Azure ExpressRoute service? -> Azure ExpressRoute is a service that provides a direct connection from the on-premises datacenter to the Microsoft cloud.
2. What Microsoft service helps to simplify a complex hub-and-spoke virtual network WAN deployment? -> Azure Virtual WAN.
3. When should Azure ExpressRoute be used instead of Azure site-to-site connectivity? -> For handling enterprise-class and mission-critical workloads.

<hr>

## Summary
Azure ExpressRoute can be used to connect your on-premises networks to the Microsoft cloud infrastructure. ExpressRoute works with an approved connectivity provider to establish the connections via a dedicated circuit.

Azure Virtual WAN can also be used to establish network connections. Azure Virtual WAN provides any-to-any connectivity, custom routing, and security.

<hr>

## Review system routes
Azure uses **system routes** to direct network traffic between virtual machines, on-premises networks, and the internet. The following situations are managed by these system routes:

- Traffic between VMs in the same subnet.
- Between VMs in different subnets in the same virtual network.
- Data flow from VMs to the internet.

For example, consider this virtual network with two subnets. Communication between the subnets and from the frontend to the internet are all managed by Azure using the default system routes.

Note: Information about the system routes is recorded in a route table. A route table contains a set of rules, called routes, that specifies how packets should be routed in a virtual network. Routing tables are associated to subnets, and each packet leaving a subnet is handled based on the associated route table. Packets are matched to routes using the destination. The destination can be an IP address, a virtual network gateway, a virtual appliance, or the internet. If a matching route can't be found, then the packet is dropped.

<hr>

## Identify user-defined routes
Azure automatically handles all network traffic routing. But, what if you want to do something different? For example, you may have a VM that performs a network function, such as routing, firewalling, or WAN optimization. You may want certain subnet traffic to be directed to this virtual appliance. For example, you might plance an appliance between subnets or a subnet and the internet.

In these situations, you can configure user-defined routes (UDRs). UDRs control network traffic by defining routes that specify the next hop of the traffic flow. The hop can be a virtual network gateway, virtual network, internet, or virtual appliance. 

Each route table can be associated to multiple subnets, but a subnet can only be associated to a single route table.

There are no charges for creating route tables in Microsoft Azure.

## Determine service endpoint issues
A virtual network service endpoint provides the identity of your virtual network to the Azure service. Once service endpoints are enabled in your virtual network, you can secure Azure service resources to your virtual network by adding a virtual network rule to the resources.

Today, Azure service traffic from a virtual network uses public IP addresses as source IP addresses. With service endpoints, service traffic switches to use virtual network private addresses as the source IP addresses when accessing the Azure service from a virtual network. This switch allows you to access the services without the need for reserved, public IP addresses used in IP firewalls.

## Why use a service endpoint?
- **Improved security for your Azure service resources**: VNet private address spaces can be overlapping and so, cannot be used to uniquely identify traffic originating from your VNet. Service endpoints secure Azure service resources to your virtual network by extending VNet identity to the service. When service endpoints are enabled in your virtual network, you secure Azure service resources to your virtual network by adding a virtual network rule. The rule improves security by fulling removing public internet access to resources, and allowing traffic only from your virtual network.

- **Optimal routing for Azure service traffic from your virtual network**: Today, any routes in your virtual network that force internet traffic to your premises and/or virtual appliances, known as force-tunneling, also force Azure service traffic to take the same route as the internet traffic. Service endpoints provide optimal routing for Azure traffic.

- **Endpoints always take service traffic directly from your virtual network to the service on the Microsoft Azure backbone network**: Keeping traffice on the Azure backbone network allows you to continue auditing and monitoring outbound internet traffic from your virtual networks, through forced-tunneling without impacting service traffic.

- **Simple to set up with less management overhead**: You no longer need reserved, public IP addresses in your virtual networks to secure Azure resources through IP firewall. There are no NAT or gateway devices required to set up the service endpoints. Service endpoints are configured through the subnet. There is no additional overhead to maintaining the endpoints.

Note: With service endpoints, the virtual machine IP addresses switches from public to private IPv4 addresses. Existing Azure service firewall rules using Azure public IP addresses will stop working with this switch. Ensure Azure service firewall rules allow for this switch before setting up service endpoints. You may experience temporary interruption to service traffic from this subnet while configuring service endpoints.

<hr>

## Determine service endpoint services
It is easy to add a service endpoint to the virtual network. Several services are available including: Azure Active Directory, Azure Cosmos DB, EventHub, KeyVault, Service Bus, SQL, and Storage.

**Azure Storage**: Generally available in all Azure regions. This endpoint gives traffic an optimal route to the Azure Storage service. Each storage account supports up to 100 virtual network rules.

**Azure SQL Database and Azure SQL Data Warehouse**: Generally available in all Azure regions. A firewall security feature that controls whether the database server for your single databases and elastic pool in Azure SQL Database or for your database in SQL Data Warehouse accepts communications that are sent from particular subnets in virtual networks.

**Azure Database for PostgreSQL server and MySQL**: Generally available in Azure regions where database service is available. Virtual Network (VNet) services endpoints and rules extend the private address space of a Virtual Network to your Azure Database for PostgreSQL server and MySQL server.

**Azure Cosmos DB**: Generally available in all Azure regions. You can configure the Azure Cosmos account to allow access only from a specific subnet of virtual network (VNet). By enabling Service endpoint to access Azure Cosmost DB on the subnet within a virtual network, the traffic from that subnet is sent to Azure Cosmos DB with the identity of the subnet and Virtual Network. Once the Azure Cosmos DB service endpoint is enabled, you can limit access to the subnet by adding it to your Azure Cosmos account.

**Azure Key Vault**: Generally available in all Azure regions. The virtual network service endpoints for Azure Key Vault allow you to restrict access to a specified virtual network. The endpoints also allow you to restrict access to a list of IPv4 (internet protocol version 4) address ranges. Any user connecting to your key vault from outside those sources is denied access.

**Azure Service Bus and Azure Event Hub**: Generally available in all Azure regions. The integration of Service Bus with Virtual Network (VNet) service endpoints enables secure access to messaging capabilities fromm workloads like virtual machines that are bound to virtual networks, with the network traffic being secured on both ends.

## Identify private link uses
Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. It simplifies the network architecture and secures the connection between endpoints in Azure by eliminating data exposure to the public internet.

- **Private connectivity to services on Azure**: Traffic remains on the Microsoft network, with no public internet access. Connect privately to services running in other Azure regions. Private Link is global and has no regional restrictions.

- **Integration with on-premises and peered networks**: Access private endpoints over private peering or VPN tunnels from on-premises or peered virtual networks. Microsoft hosts the traffic, so you don't need to set up public peering or use the internet to migrate your workloads to the cloud.

- **Protection against data exfiltration for Azure resources**: Use Private Link to map private endpoints to Azure PaaS resources. When there is a security incident within your network, only the mapped resource would be accessible, eliminating the threat of data exfiltration.

- **Services delivered directly to your customers' virtual networks**: Privately consume Azure PaaS, Microsoft partner, and your own services in your virtual networks on Azure. Private Link works across Azure Active Directory (Azure AD) tenants to help unify your experience across services. Send, approve, or reject requests directly, without permissions or role-based access controls.

## How it works**
Use Private Link to bring services delivered on Azure into your private virtual network by mapping it to private endpoint. Or privately deliver your own services in your customers' virtual networks. All traffic to the service can be routed through the private endpoint, so no gateways, NAT devices, ExpressRoute or VPN connections, or public IP addresses are needed. Private Link keeps traffic on the Microsoft global network.

<hr>

## Knowledge check
1. What statement best describes Azure routing? -> When the next hop type is none, traffic is dropped.
2. When creating user-defined routes, what is a valid next hop type? -> Internet
3. A company needs to extend their private address space in Azure by providing a direct connection to Azure resources. What should be implemented? -> Virtual network endpoint.

<hr>

## Summary
Network routes control the flow of traffic through your network. You can customize these routes, implement service endpoints, and private links.

<hr>

## Configure Azure Load Balancer
Many apps need to be resilient to failure and scale easily when demand increases. You can address those needs by using Azure Load Balancer.

## Objectives
- Identify features and usage cases for Azure load balancer.
- Implement public and internal Azure load balancers.
- Configure load balancer SKUs, backend pools, session persistence, and health probes.

<hr>

## Determine Azure load balancer uses
The Azure Load Balancer delivers high availability and network performance to your applications. The load balancer distributes inbound traffic to backend resources using load-balancing rules and health probes.

- Load-balancing rules determine how traffic is distributed to the backend.
- Health probes ensure the resources in the backend are healthy.

The Load Balancer can be used for inbound and outbound scenarios and scales up to millions of TCP and UDP application flows.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-load-balancer/media/load-balancer-4caf947b.png">

Note: Keep this diagram in mind since it covers the four components that must be configured for your load balancer: **Frontend IP configuration, Backend pools, Health probes,** and **Load-balancing rules**.

<hr>

## Implement a public load balancer
There are two types of load balancers: **public** and **internal**.

A public load balancer maps the public IP address and port number of incoming traffic to the private IP address and port of the VM. Mapping is also provided for the response traffic from the VM. By applying load-balancing rules, you can distribute specific types of traffic across multiple VMs or services. For example, you can spread the load of incoming web request traffic across multiple web servers.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-load-balancer/media/public-load-balancer-46d5d9fe.png">

<hr>

## Implement an internal load balancer
An internal load balancer directs traffic to resources that are inside a virtual network or that use a VPN to access Azure infrastructure. Frontend IP addresses and virtual networks are never directly exposed to an internet endpoint. Internal line-of-business applications run in Azure and are accessed from within Azure or from on-premises resources. For example, an internal load balancer could receive database requests that need to be distributed to backend SQL servers.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-load-balancer/media/internal-load-balancer-5ae85589.png">

An internal load balancer enables the following types of load balancing:
- **Within a virtual network**: Load balancing from VMs in the virtual network to a set of VMs that reside within the same virtual network.
- **For a cross-premises virtual network**: Load balancing from on-premises computers to a set of VMs that reside within the same virtual network.
- **For multi-tier applications**: Load balancing from internet-facing multi-tier applications where the backend tiers are not internet-facing. The backend tiers require traffic load-balancing from the internet-facing tier.
- **For line-of-business applications**: Load balancing for line-of-business applications that are hosted in Azure without load balancer hardware or software. The scenario includes on-premises servers that are in the set of computer whose traffic is load-balanced.

Note: A public load balancer could be placed in front of the internal load balancer to create a multi-tier application.

<hr>

## Determine load balancer SKUs
When you create an Azure Load Balancer, you select the type (Internal or Public) of load balancer. You also select the SKU. The load balancer supports both Basic and Standard SKUs, each differing in scenario scale, features, and pricing. The Standard Load Balancer is the newer Load Balancer product with an expanded and more granular feature set over Basic Load Balancer. It is a superset of Basic Load Balancer.

Note: The Basic SKU can be upgraded to the Standard SKU. But, new designs and architectures should use the Standard Load Balancer.

## Create backend pools
To distribute traffic, a back-end address pool contains the IP addresses of the virtual NICs that are connected to the load balancer. How you configure the backend pool depends on whether you are using the Standard or Basic SKU.

Note: In the Standard SKU, you can have up to 1000 instances in the backend pool. In the Basic SKU, you can have up to 300 instances.

<hr>

## Create load balancer rules
A load balancer rule defines how traffic is distributed to the backend pool. The rule maps a given frontend IP and port combination to a set of backend IP addresses and port combinations. Before configuring the rule, create the frontend, backend, and health probe.

Load balancing rules can be used in combination with NAT rules. For example, you could use NAT from the load balancer's public address to TCP 3389 on a specific virtual machine. The allows remote desktop access from outside of Azure.

<hr>

## Configure session persistence
By default, Azure Load Balancer distributes network traffic equally among multiple VM instances. The load balancer uses a five-tuple (source IP, source port, destination IP, destination port, and protocol type) hash to map traffic to available servers. It provides stickiness only with a transport session.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-load-balancer/media/five-tuple-hash-214cf64b.png">

Session persistence specifies how traffic from a client should be handled. The default behavior (None) is that successive requests from a client may be handled by any virtual machine. You can change this behavior.

- **None (default)** specifies any virtual machine can handle the request.

- **Client IP** specifies that successive requests from the same client IP address will be handled by the same virtual machine.

- **Client IP and protocol** specifies that successive requests from the same client IP address and protocol combination will be handled by the same virtual machine.

<hr>

## Create health probes
A health probe allows the load balancer to monitor the status of your app. The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks. When a probe fails to response, the load balancer stops sending new connections to the unhealthy instances.

There are two main ways to configure health probes: **HTTP** and **TCP**.

**HTTP custom probe**: The load balancer regularly probes your endpoint (every 15 seconds, by default). The instance is healthy if it responds with an HTTP 200 within the timeout period (default of 31 seconds). Any status other than HTTP 200 causes the probe to fail. You can specify the port (Port), the URI for requesting the health status from the backend (URI), amount of time between probe attempts (Interval), and the number of failures that must occur for the instance to be considered unhealthy (Unhealthy threshold).

**TCP custom probe**: This probe relies on establishing a successful TCP session to a defined probe port. If the specified listener on the VM exists, the probe succeeds. If the connection is refused, the probe fails. You can specify the Port, Internal, and Unhealthy threshold.

Note: There is also a guest agent probe. This probe uses the guest inside the VM. It is not recommended when HTTP or TCP custom probe configurations are possible.

<hr>

## Knowledge check
1. A company provides customers a virtual network in the cloud. There are dozens of Linux virtual machines in another virtual network. Which Azure load balancer shuld be used to direct traffic between the virtual networks? -> An internal load balancer.
2. What is the default distribution type for traffic through a load balancer? -> Five-tuple hash.
3. Which configuration is required for an internal load balancer? -> Virtual machines should be in the same virtual network.

<hr>

## Configure Azure Application Gateway
You will learn how to configure an Azure Application Gateway.

## Objectives
- Identify features and usage cases for Azure Application Gateway.
- Implement Azure Application Gateway, including selecting a routing method.
- Confiugre gateway features such as routing rules.

<hr>

## Implement Application Gateway
Application Gateway manages the requests that client applications send to a web app.

The Application Gateway uses application layer routing. Application layer routing routes traffic to a pool of web servers based on the URL of the request. The back-end pool can include Azure virtual machines, Azure virtual machine scale sets, Azure App Service, and even on-premises servers.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-application-gateway/media/application-gateway-cb3392f4.png">

The Application Gateway uses round robin to send load balance requests to the servers in each back-end pool. The Application Gateway provides session stickiness. Use session stickiness to ensure client requests in the same session are routed to the same back-end server.

## Additional features
- Support for the HTTP, HTTPS, HTTP/2 and WebSocket protocols.
- A web application firewall to protect against web application vulnerabilities.
- End-to-end request encryption.
- Autoscaling, to dynamically adjust capacity as your web traffic load change.

<hr>

## Determine Application Gateway routing
Clients send requests to your web apps to the IP address or DNS name of the gateway. The gateway routes requests to a selected web server in the back-end pool, using a set of rules configured for the gateway to determine where the request should go.

There are two primary methods of routing traffic, path-based routing and multiple site routing.

## Path-based routing
Path-based routing sends requests with different URL paths to different pools of back-end servers. For example, you can direct requests with the path /video/* to a back-end pool containing servers that are optimized to handle video streaming, and direct /images/* requests to a pool of servers that handle image retrieval.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-application-gateway/media/path-based-routing-15bcef5f.png">

## Multiple site routing
Multiple site routing configures more than one web application on the same application gateway instance. In a multi-site configuration, you register multiple DNS names (CNAMEs) for the IP address of the Application Gateway, specifying the name of each site. Application Gateway uses separate listeners to wait for requests for each site. Each listener passes the request to a different rule, which can route the requests to servers in a different back-end pool.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/wwl-azure/configure-azure-application-gateway/media/site-based-routing-e686b605.png">

Multi-site configurations are useful for supporting multi-tenant applications, where each tenant has its own set of virtual machines or other resources hosting a web application.

## Other features
- **Redirection**: Redirection can be used to another site, or from HTTP to HTTPS.
- **Rewrite HTTP headers**: HTTP headers allow the client and server to pass parameter information with the request or the response.
- **Custom error pages**: Application Gateway allows you to create custom error pages instead of displaying default error pages. You can use your own branding and layout using a custom error page.

<hr>

## Application Gateway component set up
Application Gateway has a series of components that combine to route requests to a pool of web servers and to check the health of these web servers.

## Front-end IP address
Client requests are received through a front-end IP address. You can configure Application Gateway to have a public IP address, a private IP address, or both. Application Gateway can't have more than one public and one private IP address.

## Listeners
Application Gateway uses one or more listeners to receive incoming requests. A listener accepts traffic arriving on a specified combination of protocol, port, host, and IP address. Each listener routes requests to a back-end pool of servers following routing rules that you specify. A listener can be Basic or Multi-site. A Basic listener only routes a request based on the path in the URL. A Multi-site listener can also route requests using the hostname element of the URL.

Listeners also handle TLS/SSL certificates for securing your application between the user and Application Gateway.

## Routing rules
A routing rule binds a listener to the back-end pools. A rule specifies how to interpret the hostname and path elements in the URL of a request, and then direct the request to the appropriate back-end pool. A routing rule also has an associated set of HTTP settings. These HTTP settings indicate whether (and how) traffic is encrypted between Application Gateway and the back-end servers. Other configuration information includes Protocol, Session stickiness, Connection draining, Request timeout period, and Health probes.

## Back-end pools
A back-end pool references a collection of web servers. You provide the IP address of each web server and the port on which it listens for requests when configuring the pool. Each pool can specify a fixed set of virtual machines, a virtual machine scale-set, an app hosted by Azure App Services, or a collection of on-premises servers. Each back-end pool has an associated load balancer that distributes work across the pool.

## Web application firewall
The web application firewall (WAF) is an optional component that handles incoming requests before they reach a listener. The web application firewall checks each request for many common threats, based on the Open Web Application Security Project (OWASP). Common threats include SQL-injection, Cross-site scripting, Command injection, HTTP request smuggling, HTTP response splitting, Remote file inclusion, Bots, crawlers, and scanners, the HTTP protocol violations and anomalies.

## Health probes
Health probes determine which servers are available for load-balancing in a back-end pool. The Application Gateway uses a health probe to send a request to a server. When the server returns an HTTP response with a status code between 200 and 399, the server is considered healthy.

If you don't configure a health probe, Application Gateway creates a default probe that waits for 30 seconds before deciding that a server is unavailable.

<hr>

## Knowledge check
1. Which criteria does Application Gateway use to route requests to a web server? -> The hostname, port, and path in the URL of the request.
2. Which load balancing strategy does the Application Gateway implement? -> Distributes requests to each available server in the backend pool in turn, round-robin.
3. The web team is installing the Application Gateway. They want to ensure incoming requests are checked for common security threats like cross-site scripting and crawlers. To address the concerns, what should be done? -> Install Web Application Firewall.

<hr>

## Summary
The Application Gateway provides load balancing and application routing capabilities across multiple web sites. Several routing methods are available including path-based routing. Also, the Application Gateway includes the Web Application Firewall with built-in security features.

<hr>

## Design an IP addressing schema for your Azure deployment
The schema ensures that communication works for deployed resources, minimizes public exposure of systems, and gives the organization flexibility in its network.

## Objectives
- Identify the private IP addressing capabilities of Azure virtual networks.
- Identify the public IP addressing capabilities of Azure.
- Identify the requirements for IP addressing when integrating with on-premises networks.

<hr>

## Network IP addressing and integration
To integrate resources in an Azure virtual network with resources in your on-premises network, you must understand how you can connect those resources and how to configure IP addresses.

## On-premises IP addressing
A typical on-premises network design includes these components:
- Routers
- Firewalls
- Switches
- Network segmentation

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/design-ip-addressing-for-azure/media/2-on-premises-network.png">

The diagram shows a simplified version of a typical on-premises network. On the routers facing the internet service provider, you have public IP addresses that are used by your outbound internet traffic as their source. These addresses also are used for your inbound traffic across the internet. The ISP might issue you a block of IP addresses to assign to your devices, or you might have your own block of public IP addresses that your organization owns and controls. You can assign these addresses to systems that you would like to make accessible from the internet, such as web servers.

The perimeter network and internal zone have private IP addresses. In the perimeter network and internal zone, the IP addresses that are assigned to these devices aren't accessible over the internet. The administrator has full control over the IP address assignment, name resolution, security settings, and security rules. There are three ranges of non-routable IP addresses that are designed for internal networks that won't be sent over internet routers:

- 10.0.0.0 to 10.255.255.255
- 172.16.0.0 to 172.31.255.255
- 192.168.0.0 to 192.168.255.255

The administrator can add or remove on-premises subnets to accommodate network devices and services. The number of subnets and IP addresses you can have in your on-premises network depends on the Classless Inter-Domain Routing (CIDR) for the IP address block.

## Azure IP addressing
Azure virtual networks use private IP addresses. The ranges of private IP addresses are the same as for on-premises IP addressing. Like on-premises networks, the administrator has full control over the IP address assignment, name resolution, security settings, and security rules in an Azure virtual network. The administrator can add or remove subnets depending on the CIDR for the IP address block.

In a typical Azure network design, we usually have these components:
- Virtual networks
- Subnets
- Network security groups
- Firewalls
- Load balancers

In Azure, the network design has features and functions that are similar to an on-premises network, but the network's structure is different. The Azure network does not follow the typical on-premises hierarchical network design. The Azure network provides the ability to scale up and scale down infrastructure based on demand. Provisioning in the Azure network happens in a matter of seconds. There are no hardware devices, like routers or switches. The intire infrastructure is virtual, and you can slice it up into chunks that suit your requirements.

In Azure, you typically would implement a network security group and a firewall. You'd use subnets to isolate front-end services, including web servers and DNS, and back-end services like databases and storage systems. Network security groups filter internal and external traffic at a network layer. A firewall has more extensive capabilities for network layer filtering and application layer filtering. By deploying both network security groups and a firewall, you get improved isolation of resources for a secure network architecture.

## Basic properties of Azure virtual networks
A virtual network is your network in the cloud. You can divide your virtual network into multiple subnets. Each subnet has a portion of the IP address space that is assigned to your virtual network. You can add, remove, expand, and shrink a subnet if there are no VMs or services deployed to it.

By default, all subnets in an Azure virtual network can communicate with each other. However, you can use a network security group to deny communications between subnets. The smallest subnet that is supported uses a /29 subnet mask. The largest supported subnet uses a /2 subnet mask.

## Integrate Aure with on-premises networks
Before you start integrating Azure with on-premises networks, it's important to identify the current private IP address scheme used in the on-premises network. There can be no IP address overlap for interconnected networks.

For example, you can't use 192.168.0.0/16 on your on-premises network and use 192.168.10.0/24 on your Azure virtual network. These ranges both contain the same IP addresses, and won't be able to route trafic between each other.

You can, however, have the same class range for multiple networks. For example, you can use the 10.10.0.0/16 address space for your on-premises network and the 10.20.0.0/16 address space for your Azure network because they don't overlap.

It is vital to check for overlaps when you're planning an IP address scheme. If there's an overlap of IP addresses, you can't integrate your on-premises network with your Azure network.

## Knowlege check
1. What are some of the typical components involved in a network design? -> A VM, subnet, firewall, and load balancer.
2. Which of the folowing IP address ranges is routable over the internet? -> 215.11.0.0 to 215.11.255.255

<hr>

## Public and private IP addressing in Azure
In this unit, you'll explore the constraints and limitations for public and private IP addresses in Azure.

## IP address types
In Azure, you can use two types of IP addresses:
- Public IP addresses
- Private IP addresses

Both types of IP addresses can be allocated in one of two ways:

- Dynamic
- Static

## Public IP addresses
Use a public IP address for public-facing services. A public address can be either static or dynamic. A public IP address can be assigned to a VM, an internet-facing load balancer, a VPN gateway, or an application gateway.

- **Dynamic public IP addresses** are assigned addresses that can change over the lifespan of the Azure resource. The dynamic IP address is allocaed when you create or start a VM. The IP address is released when you stop or delete the VM. In each Azure region, public IP addresses are assigned from a unique pool of addresses. The default allocation method is dynamic.

- **Static public IP addresses** are assigned addresses that won't change over the lifespan of the Azure resource. To ensure that the IP address for the resource remains the same, you can set the allocation method to static. In this case, an IP address is assigned immediately and is released only when you delete the resource or change the IP allocation method to dynamic.

## SKUs for public IP addresses
For public IP addresses, there are two SKUs to choose from: **Basic** and **Standard**. All public IP addresses created before the introduction of SKUs are Basic SKU public IP addresses. With the introduction of SKUs, you can choose the scale, features, and pricing for load balancing internet traffic.

Both Basic and Standard SKUs:
- Have a default inbound originated flow idle timeout of 4 minutes, which is adjustable up to 30 minutes.
- Have a fixed outbound timeout originated flow idle timeout of 4 minutes.

## Basic SKU
Bsic public IPs can be assigned by using static or dynamic allocation methods. Basic public IPs can be assigned to any Azure resource that can be assigned a public IP address, including network interfaces, VPN gateways, application gateways, and internet-facing load balancers.

By default, Basic SKU IP addresses:
- Are open. Network security groups are recommended but optional for restricting inbound or outbound traffic.
- Are available for inbound only traffic.
- Are available when using instance meta data service (IMDS).
- Don't support Availability Zones.
- Don't support routing preferences.

## Standard SKU
By default, Standard SKU IP addresses:
- Always use static allocation.
- Are secure, and thus closed to inbound traffic. You must enable inbound traffic by using a network security group.
- Are zone-redundant; and optionally zonal (they can be created as zonal and guaranteed in a specific availability zone).
- Can be assigned to network interfaces, Standard public load balancers, application gateways, or VPN gateways.
- Can be utilized with the routing preference to enable more granular control of how traffic is routed between Azure and the Internet.
- Can be used as anycast frontend IPs for cross-region load balancers.

## Public IP address prefix
In Azure, a *public IP prefix* is a reserved, static range of public IP addresses. Azure assigns an IP address from a pool of available addresses that's unique to each region in each Azure cloud. When you define a Public IP address prefix, associated public IP addresses are assigned from a pool for an Azure region.

In a region with Availability Zones, Public IP address prefixes can be created as zone-redundant or associated with a specific availability zone.

The benefit of a public IP address prefix is that you can specify firewall rules for a known range of IP addresses. If your business needs to have datacenters in different regions, you need a different public IP address range for each region. You can assign the addresses from a public IP address prefix to any Azure resource that supports public IP addresses.

You can create a public IP address prefix by specifying a name and prefix size. The prefix size is the number of reserved addresses available for use.

- Public IP address prefixes consist of IPv4 or IPv6 addresses.
- You can use technology like Azure Traffic Manager to balance region-specific instances.
- You can't bring your own public IP addresses from on-premises networks into Azure.
- You can't specify addresses when you create a prefix; they're assigned by Azure. After a prefix is created, the IP addresses are fixed in a contiguous range.
- Public IP addresses can't be moved between regions; all IP addresses are region-specific.

## Private IP addresses
Private IP addresses are used for communication within an Azure Virtual Network, including virtual networks and your on-premises networks. Private IP addresses can be set to dynamic (DHCP lease) or static (DHCP reservation).

**Dynamic private IP addresses** are assigned through a DHCP lease and can change over the lifespan of the Azure resource.

**Static private IP addresses** are assigned through a DHCP reservation and don't change throughout the lifespan of the Azure resource. Static private IP addresses persist if a resource is stopped or deallocated.

## IP addressing for Azure virtual networks
In Azure, a virtual network is a fundamental component that acts as an organization's network. The administrator has full control over IP address assignment, security settings, and security rules. When you create a virtual network, you define a scope of IP addresses. Private IP addressing works the same way as it does in an on-premises network. You choose the private IP addresses that are reserved by Internet Assigned Numbers Authority (IANA) based on your network requirements:

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

A subnet is a range of IP address within the virtual network. You can dived a virtual network into multiple subnets. Each subnet must have a unique address range, which is specified in a classless interndomain routing (CIDR) format. CIDR is a way to represent a block of network IP addresses. An IPv4 CIDR, specified as part of the IP address, shows the length of the network prefix.

Consider, for example, CIDR 192.168.10.0/24. "192.168.10.0" is the network address, and "24" indicates that the first 24 bits are part of the network address, leaving the last 8 bits for specific host addresses. The address range of a subnet can't overlap with other subnets in the virtual network or with the on-premises network.

For all subnets in Azure, the first three IP addresses are reserved by default. For protocol conformance, the first and last IP addresses of all subnets also are reserved. In Azure, an internal DHCP service assigns and maintains the lease of IP addresses. The .1, .2, .3, and last IP addresses are not visible or configurable by the Azure customer. These addresses are reserved and used by internal Azure services.

In Azure virtual networks, IP addresses can be allocated to the following types of resources:
- Virtual machine network interfaces
- Load balancers
- Application gateways

## Knowledge check
1. What resource can you assign a public IP address to? -> A virtual machine.
2. What must a virtual machine have to communicate with other resources in the same virtual network? -> Network interface.

<hr>

## Plan IP addressing for your networks
In this unit, you'll explore the requirements for a network IP address scheme. You'll learn about classless inter-domain routing (CIDR) and how you use it to slice an IP block to meet your addressing needs.

## Gather your requirements
Before planning your network IP address scheme, you must gather the requirements for your infrastructure. These requirements also will help you prepare for future growth by reserving extra IP addresses and subnets.

Here are two of the questions you might ask to discover the requirements:
- How many devices do you have on the network?
- How many devices are you planning to add to the network in the future?

When your network expands, you don't want to redesign the IP address scheme. Here are some other questions you could ask:

- Based on the services running on the infrastructure, what devices do you need to separate?
- How many subnets do you need?
- How many devices per subnet will you have?
- How many devices are you planning to add to the subnets in the future?
- Are all subnets going to be the same size?
- How many subnets do you want or plan to add in the future?

You'll need to isolate some services. Isolation of services provides an additional layer of security, but also requires good planning. For example, your front-end servers can be accessed by public devices, but the back-end servers need to be isolated. Subnets help isolate the network in Azure. However, by default, all subnets within a virtual network can communicate with each other in Azure. To provide further isolation, you can use a network security group. You might isolate services depending on the data and its security requirements. For example, you might choose to isolate HR data and the company's financial data from customer databases.

When you know the requirements, you'll have a greater understanding of the total number of devices on the network per subnet and how many subnets you'll need. CIDR allows more flexible allocation of IP addresses than was possible with the original system of IP address classes. Depending on your requirements, you'll slice the IP block into the required subnets and hosts.

Remember that Azure uses the first three addresses of each subnet. The first and last IP addresses of the subnets also are reserved for protocol conformance. Therefore, the number of possible addresses on an Azure subnet is (2^n)-5, where n represents the number of host bits.

<hr>

## Connect services by using virtual network peering
You can use virtual network peering to directly connect Azure virtual networks together. When you use peering to connect virtual networks, virtual machines (VMs) in these networks can communicate with each other as if they're in the same network.

With peered virtual networks, traffic between virtual machines are routed through the Azure network. The traffic uses only private IP addresses. It doesn't rely on internet connectivity, gateways, or encrypted connections. The traffic is always private, and it takes advantage of the high bandwidth and low latency of the Azure backbone network.

The two types of peering connections are created in the same way:
- **Virtual network peering** connects virtual networks in the same Azure region, such as two virtual networks in North Europe.
- **Global virtual network peering** connects virtual networks that are in different Azure regions, such as a virtual network in North Europe and virtual network in West Europe.

Virtual network peering doesn't affect or disrupt any resources that you've already deployed to the virtual networks. But when you use virtual network peering, consider the key features that the following sections define.

## Reciprocal connections
When you create a virtual network peering connection with Azure PowerShell or Azure CLI, only one side of the peering gets created. To complete the virtual network peering configuration, you'll need to configure the peering in reverse direction to establish connectivity. When you create the virtual network peering connection through the Azure portal, the configuration for both sides are completed at the same time.

Think of how you connect two network switches together. You connect a cable to each switch and maybe configure some settings so that the switches can communciate. Virtual network peering requires similar connections in each virtual network. Reciprocal connections provide this functionality.

## Cross-subscription virtual network peering
You can use virtual network peering even when both virtual networks are in different subscriptions. This set up might be necessary for mergers and acquisitions or to connect virtual networks in subscriptions that different departments manage. Virtual networks can be in different subscriptions, and the subscriptions can use the same or different Azure Active Directory tenants.

When you use virtual network peering across subscriptions, you might find that an administrator of one subscription doesn't administer the peer network's subscription. The administrator might not be able to configure both ends of the connection. To peer the virtual networks when both subscriptions are in different Azure Active Directory tenants, the administrators of each subscription must grant the peer subscription's administrator the `Network Contributor` role on their virtual network.

## Transitivity
Virtual network peering is nontransitive. Only virtual networks that are directly peered can communicate with each other. Virtual networks can't communicate with peers of their peers.

Suppose, for example, that your three virtual networks (A, B, C) are peered like this: A <-> B <-> C. Resources in A can't communicate with resources in C because that traffic can't transit through virtual network B. If you need communication between virtual network A and virtual network C, you must explicitly peer these two virtual networks.

## Gateway transit
You can connect to your on-premises network from a peered virtual network if you enable gateways transit from a virtual network that has a VPN gateway. Using gateway transit, you can enable on-premises connectivity without deploying virtual network gateways to all your virtual networks. This method can reduce the overall cost and complexity of your network. By using virtual network peering with gateway transit, you can configure a single virtual network as a hub network. Connect this hub network to your on-premises datacenter and share its virtual network gateway with peers.

To enable gateway transit, configure the **Allow gateway transit** option in the hub virtual network where you deployed the gateway connection to your on-premises network. Also connect the **Use remote gateways** option in any spoke virtual networks.

Note: if you want to enable the **Use remote gateways** option in a spoke network peering, you can't deploy a virtual network gateway in the spoke virtual network.

## Overlapping address spaces
IP address spaces of connected networks within Azure, between Azure and on your on-premises network can't overlap. This is also true for peered virtual networks. Keep this rule in mind when you're planning your network design. In any networks you connect through virtual network peering, VPN, or ExpressRoute, assign different address spaces that don't overlap.

## Alternative connectivity methods
Virtual network peering is the least complex way to connect virtual networks together. Other methods focus primarily on connectivity between on-premises and Azure networks rather than connections between virtual networks.

You can also connect virtual networks together through an ExpressRoute circuit. ExpressRoute is a dedicated, private connection between an on-premises datacenter and the Azure backbone network. The virtual networks that connect to an ExpressRoute circuit are part of the same routing domain and can communicate with each other. ExpressRoute connections don't go over the public internet, so your communications with Azure services are as secure as possible.

VPNs use the internet to connect your on-premises datacenter to the Azure backbone through an encrypted tunnel. You can use a site-to-site configuration to connect virtual networks together through VPN gatways. VPN gateways have higher latency than virtual network peering setups. They're more complex and can cost more to manage.

When virtual networks are connected through both a gateway and virtual network peering, traffic flows through the peering configuration.

## When to choose virtual network peering
Virtual network peering can be a great way to enable network connectivity between services that are in different virtual networks. Because it's easy to implement and deploy, and it works well across regions and subscriptions, virtual network peering should be your first choice when you need to integrate Azure virtual networks.

Peering might not be your best option if you have existing VPN or ExpressRoute connections or services behind Azure Basic Load Balancers that would be accessed from a peered virtual network. In these cases, you should research alternatives.

<hr>

## Summary
Peering connections will enable communication for services that run on the VMs. This method allows low-latency communication between resources in virtual networks. It supports scenarios where resources are in different regions or subscriptions. Virtual network peering should be your first chioce when you need to connect virtual networks.

<hr>

## Host your domain on Azure DNS
Azure DNS lets you host your DNS records for your domains on Azure infrastructure. With Azure DNS, you can use the same credentials, APIs, tools, and billing as your other Azure services.

## Objectives
- Configure Azure DNS to host your domain.

<hr>

## What is Azure DNS?
Azure DNS is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure.

## What is DNS?
DNS, or the Domain Name System, is a protocol within the TCP/IP standard. DNS serves an essential role of translating human-readable domain names, for example `www.wideworldimports.com`, into a known IP address. IP addresses enable computers and network devices to identify and route requests between themselves.

DNS uses a global directory hosted on servers around the world. Microsoft is part of that network that provides a DNS service through Azure DNS.

A DNS server is also known as a DNS name server, or just a name server.

## How does DNS work?
A DNS server carries out one of two primary functions:
- Maintains a local cache of recently accessed or used domain names and their IP addresses. This cache provides a faster response to a local domain lookup request. If the DNS server can't find the requested domain, it passes the request to another DNS server. This process repeats at each DNS server until either a match is made or the search times out.
- Maintains the key-value pair database of IP addresses and any host or subdomain over which the DNS server has authority. This function is often associated with mail, web, and other internet domain services.

## DNS server assignment
In order for a computer, server, or other network-enabled device to access web-based resources, it must reference a DNS server.

When you connect by using your on-premises network, the DNS settings come from your server. When you connect by using an external location, like a hotel, the DNS settings come from the internet service provider (ISP).

## Domain lookup requests
Here's a simplified overview of the process a DNS server uses when it resolves a domain name lookup request:

- Checks to see if the domain name is stored in the short-term cache. If so, the DNS server resolves the domain request.
- If the domain isn't in the cache, it contacts one or more DNS servers on the web to see if they have a match. When a match is found, the DNS server updates the local cache and resolves the request.
- If the domain isn't found after a reasonable number of DNS checks, the DNS server responds with a *domain cannot be found* error.

## IPv4 and IPv6
Every computer, server, or network-enabled device on your network has an IP address. An IP address is unique within your domain. There are two standards of IP address: IPv4 and IPv6.

- **IPv4** is composed of four sets of numbers, in the range of 0 to 255, each separated by a dot. Example: 127.0.0.1. Today, IPv4 is the most commonly used standard. Yet, with the increase in IoT devices, the IPv4 standard will eventually be unable to keep up.

- **IPv6** is a relatively new standard and will eventually replace IPv4. It's made up of eight groups of hexadecimal numbers, each separated by a colon. Example: fe80:11a1:ac15:e9gf:e884:edb0:ddee:fea3.

Many network devices are now provisioned with both an IPv4 and an IPv6 address. The DNS name server can resolve domain names to both IPv4 and IPv6 addresses.

## DNS settings for your domain
Whether the DNS server for your domain is hosted by a third party or managed in-house, you'll need to configure it for each host type you're using. Host types include web, email, or other services you're using.

## DNS record types
Configuration information for your DNS server is stored as a file within a zone on your DNS server. Each file is called a record. The following record types are the most commonly created and used:

- **A** is the host record, and is the most common type of DNS record. It maps the domain or host name to the IP address.
- **CNAME** is a Canonical Name record that's used to create an alias from one domain name to another domain name. If you had different domain names that all accessed the same website, you would use CNAME.
- **MX** is the mail exchange record. It maps mail requests to your mail server, whether hosted on-premises or in the cloud.
- **TXT** is the text record. It's used to associate text strings with a domain name. Azure and Microsoft 365 use TXT records to verify domain ownership.

Additionaly, there are the following record types:
- Wildcards
- CAA (certificate authority)
- NS (name server)
- SOA (start of authority)
- SPF (sender policy framework)
- SRV (server locations)

The SOA and NS records are created automatically when you create a DNS zone by using Azure DNS.

## Record sets
Some record types support the concept of record sets, or resource record sets. A record set allows for multiple resources to be defined in a single record. For example, here is an A record that has one domain with two IP addresses:

        www.wideworldimports.com        3600    IN      A       127.0.0.1
        www.wideworldimports.com        3600    IN      A       127.0.0.2

SOA and CNAME records can't contain record sets.

## What is Azure DNS?
Azure DNS allows you to host and manage your domains by using a globally distributed name server infrastructure. It allows you to manage all of your domains by using your existing Azure credentials.

Azure DNS acts as the SOA for the domain.

You can't use Azure DNS to register a domain name. You use a third-party domain registrar to register your domain.

## Why use Azure DNS to host your domain?
Azure DNS is built on the Azure Resource Manager service, which offers the following benefits:
- Improved security
- Ease of use
- Private DNS domains
- Alias record sets

At this time, Azure DNS doesn't support Domain Name System Security Extensions. If you require this security extension, you should host those portions of your domain with a third-party provider.

## Security features
Azure DNS provides the following security features:

- Role-based access control, which gives you fine-grained control over users' access to Azure resources. You can monitor their usage and control the resources and services to which they have access.
- Activity logs, which let you track changes to a resource and pinpoint where faults occurred.
- Resource locking, which gives a greater level of control to restrict or remove access to resource groups, subscriptions, or any Azure resources.

## Ease of use
Azure DNS can manage DNS records for your Azure services, and provide DNS for your external resources. Azure DNS uses the same Azure credentials, support contract, and billing as your other Azure services.

You can manage your domains and records by using the Azure portal, Azure PowerShell cmdlets, or the Azure CLI. Applications that require automated DNS management can integrate with the service by using the REST API and SDKs.

## Private domains
Azure DNS handles the translation of external domain names to an IP address. Azure DNS lets you create private zones. These provide name resolution for virtual machines (VMs) within a virtual network, and between virtual networks, without having to create a custom DNS solution. This allows you to use your own custom domain names rather than the Azure-provided names.

To publish a private DNS zone to your virtual network, you'll specify the list of virtual networks that are allowed to resolve records within the zone.

Private DNS zones have the following benefits:
- There's no need to invest in a DNS solution. DNS zones are supported as part of the Azure infrastructure.
- All DNS record types are supported: A, CNAME, TXT, MX, SOA, AAAA, PTR, and SRV.
- Host names for VMs in your virtual network are automatically maintained.
- Split-horizon DNS support allows the same domain name to exist in both private and public zones. It resolves to the correct one based on the originating request location.

## Alias record sets
Alias records sets can point to an Azure resource. For example, you can set up an alias record to direct traffic to an Azure public IP address, an Azure Traffic manager profile, or an Azure Content Delivery Network endpoint.

The alias record set is supported in the following DNS record types:
- A
- AAAA
- CNAME

## Knowledge check
1. What does Azure DNS allow you to do? -> Manage and host your registered domain and associated records.
2. What security features does Azure DNS provide? -> Role-based access control, activity logs, and resource locking.
3. What type of DNS record should you create to map one or more IP addresses against a single domain? -> A or AAAA

<hr>

## Configure Azure DNS to host your domain
In this unit, you'll learn how to:
- Create and configure a DNS zone for your domain by using Azure DNS.
- Understand how to link your domain to an Azure DNS zone.
- Create and configure a private DNS zone.

## Configure a public DNS zone
You'll use a DNS zone to host the DNS records for a domain, such a wideworldimports.com.

## Step 1: Create a DNS zone in Azure
To host a domain name with Azure DNS, you first need to create a DNS zone for that domain. A DNS zone holds all the DNS entries for your domain.

When creating a DNS zone, you need to supply the following details:

- **Subscription**: The subscription to be used.
- **Resource group**: The name of the resource group to hold your domains. If one doesn't exist, create one to allow for better control and management.
- **Name**: The name of your domain.
- **Resource group location**: The location defaults to the location of the resource group.

## Step 2: Get your Azure DNS name servers
After you create a DNS zone for the domain, you need to get the name server details from the name servers (NS) record. You'll use these details to update your domain registrar's information and point to the Azure DNS zone.

<img width="517" alt="image" src="https://docs.microsoft.com/en-us/learn/modules/host-domain-azure-dns/media/3-name-server.png">

## Step 3: Update the domain registrar setting
As the domain owner, you need to sign in to the domain-management application provided by your domain registrar. In the management application, edit the NS record, and change the NS details to match your DNS name server details.

Change the NS details is called *domain delegation*. When you delegate the domain, you must use all four name servers provided by Azure DNS.

## Step 4: Verify delegation of domain name services
The next step is to verify that the delegated domain now points to the Azure DNS zone you created for the domain. This can take as few as 10 minutes, but might take longer.

To verify the success of the domain delegation, query the start of authority (SOA) record. The SOA record was automatically created when the Azure DNS zone was set up. You can do this by using a third-party tool like nslookup.

The SOA record represents your domain and will become the reference point when other DNS servers are searching for your domain on the internet.

To verify the delegation, use nslookup like this:

        nslookup -type=SOA wideworldimports.com

## Step 5: Configure your custom DNS settings
The domain name is wideworldimports.com. When it's used in a browser, the domain resolves to your website. But what if you want to add in web servers or load balancers? These resources need to have their own custom setttings in the DNS zone, either as an A record or a CNAME.

## A record
Each A record requires the following details:
- **Name**: The name of the custom domain, for example *webserver1*.
- **Type**: In this instance, it's A.
- **TTL**: Represents the "time-to-live" as a whole unit, where 1 is one second. The value indicates how long the A record lives in a DNS cache before it expires.
- **IP address**: The IP address of the server to which this A record should resolve.

## CNAME record
The CNAME is the canonical name, or the alias for an A record. Use CNAME when you have different domain names that all access the same website. For example, you might need a CNAME in the *wideworldimports* zone, if you want both www.wideworldimports.com and wideworldimports.com to resolve to the same IP address.

You would create the CNAME record in the *wideworldimports* zone with the following information:

- Name: www
- TTL: 600 seconds
- Record type: CNAME

If you expose a web function, you would create a CNAME record that resolves to the Azure function.

## Configure private DNS zone
To provide name resolution for virtual machines within a virtual network and between virtual networks, create a private DNS zone.

<hr>

## Dynamically resolve resource name by using alias record
You know that the A record and CNAME record don't support direct connection to Azure resources like your load balancers. You've been tasked with finding out how to linke the apex domain with a load balancer.

## What is an apex domain?
The apex domain is the highest level of your domain. The apex domain is also sometimes referred to the *zone apex* or *root apex*. It's often represented by the @ symbol in your DNS zone records.

CNAME records that you might need for an Azure Traffic Manager profile or Azure Content Delivery Network endpoints aren't supported at the zone apex level. However, other *alias records* are supported at the zone apex level.

## What are alias records?
Azure alias records enable a zone apex domain to reference other Azure resources from the DNS zone. You don't need to create complex redirection policies. You can also use an Azure alias to route all traffic through Traffic Manager.

The Azure alias record can point to the following Azure resources:
- A Traffic Manager profile
- Azure Content Delivery Network endpoints
- A public IP resource
- A front door profile

Alias records provide lifecycle tracking of target resources, ensuring that changes to any target resource are automatically applied to the DNS zone. Alias records also provide support for load-balanced applications in the zone apex.

The alias record set supports the following DNS zone record types:
- **A**: The IPv4 domain name-mapping record.
- **AAAA**: The IPv6 domain name-mapping record.
- **CNAME**: The alias for your domain, which links to the A record.

## Uses for alias records
The following are some of the advantages of using alias records:
- **Prevents dangling DNS records**: A dangling DNS record occurs when the DNS zone records aren't up to date with changes to IP addresses. Alias records prevent dangling references by tightly coupling the lifecycle of a DNS record with an Azure resource.
- **Updates DNS record set automatically when IP addresses change**: When the underlying IP address of a resource, service, or application is changed, the alias record ensures that any associated DNS records are automatically refreshed.
- **Hosts load-balanced applications at the zone apex**: Alias records allow for apex resource routing to Traffic Manager.
- **Points zone apex to Azure Content Delivery Network endpoints**: With alias records, you can now directly reference your Azure Content Delivery Network instance.

An alias record allows you to link the zone apex to a load balancer. It creates a link to the Azure resource, rather than a direct IP-based connection. So, if the IP address of your load balancer changes, the zone apex record continues to work.

<hr>

## Manage and control traffic flow in your Azure deployment with routes
Learn how to control Azure virtual network traffic by implementing custom routes.

## Objectives
- Identify the routing capabilities of an Azure virtual network
- Configure routing within a virtual network
- Deploy a basic network virtual appliance
- Configure routing to send traffic through a network virtual appliance

<hr>

## Azure routing
Network traffic in Azure is automatically routed across Azure subnets, virtual networks, and on-premises networks. This routing is controlled by system routes, which are assigned by default to each subnet in a virtual network.

You can't create or delete system routes, but you can override the system routes by adding custom routes to control traffic flow to the next hop.

Within Azure, there are other system routes. Azure will create these routes if the following capabilities are enabled:
- Virtual network peering
- Service chaining
- Virtual network gateway
- Virtual network service endpoint

## Virtual newtork service endpoint
Virtual network endpoints extend your private address space in Azure by providing a direct connection to your Azure resources. This connection restricts the flow of traffic: you Azure virtual machines can access your storage account directly from the private address space and deny access from a public virtual machine. As you enable service endpoints, Azure creates routes in the route table to direct this traffic.

## Custom routes
Custom routes allow for more control when routing traffic (say through an NVA or through a firewall). 

You have two options for implementing custom routes: create a user-defined route, or use Border Gateway Protocol (BGP) to exchange routes between Azure and on-premises networks.

## User-defined routes
You can use a user-defined route to override the default system routes so traffic can be routed through firewalls or NVAs.

## Border gateway protocol
A network gateway in your on-premises network can exchange routes with a virtual network gateway in Azure by using BGP. BGP is the standard routing protocol that is normally used to exchange routing and information among two or more networks. BGP is used to transfer data and information between host gateways llike on the internet or between autonomous systems.

You'll typically use BGP to advertise on-premises routes to Azure when you're connected to an Azure datacenter through Azure ExpressRoute. you can also configure BGP if you connect to an Azure virtual network by using a VPN site-to-site connection.

BGP offers network stability, because routers can quickly change connections to send packets if a connection path goes down.

## Route selection and priority
If multiple routes are available in a route table, Azure uses the route with the longest prefix match.

The longer the route prefix, the shorter the list of IP addresses through that prefix. When you use the longer prefixes, the routing algorithm can select the intended address more quickly.

You can't configure multiple user-defined routes with the same address prefix.

If there are multiple routes with the same address prefix, Azure selects the route based on the type of the following order of priority:
1. User-defined routes
2. BGP routes
3. System routes

## Knowledge check
1. Why would you use a custom route in a virtual network? -> To control the flow of traffic within your Azure virtual network.
2. Why might you use virtual network peering? -> To connect virtual networks together in the same region or across regions.

<hr>

## What is an NVA?
A network virtual appliance (NVA) is a virtual appliance that consists of various latyers like:
- a firewall
- a WAN optimizer
- application-delivery controllers
- routers
- load balancers
- IDS/IPS
- proxies

You can use an NVA to filter traffic inbound to a virtual network, to block malicious requests, and to block requests made from unexpected resources.

## Network virtual appliance
Network virtual appliances (NVAs) are virtual machines that control the flow of network traffic by controlling routing. You'll typically use them to manage traffic flowing from a perimeter-network environment to other networks or subnets.

## User-defined routes
For most environments, the default system routes already defined by Azure are enough to get the environments up and running. In certain cases, you should create a routing table and add custom routes. Examples include:
- Access to the internet via on-premises network using forced tunneling.
- Using virtual appliances to control traffic flow.

You can create multiple route tables in Azure. Each route table can be associated with one or more subnets. A subnet can only be associated with one route table.

## Network virtual appliances in a highly available architecture
If traffic is routed through an NVA, the NVA becomes a critical piece of your infrastructure. Any NVA failures will directly affect the ability of your services to communicate. It's important to include a highly available architecture in your NVA deployment.

## Knowledge check
1. What is the main benefit of using a network virtual appliance? -> To control incoming traffic from the perimeter network and allow only traffic that meets security requirements to pass through.
2. How might you deploy a network virtual appliance? -> You can configure a Windows virtual machine and enable IP forwarding after route tables, user-defined routes, and subnets have been updated. Or you can use a partner image from Azure Marketplace.

<hr>

## Azure Load Balancer features and capabilities
With Azure Load Balancer, you can spread user requests across multiple virtual machines or other services, allowing you to scale the app to larger sizes than a single virtual machine can support, and ensuring that users get service even when a virtual machine fails.

## Distribute traffic with Azure Load Balancer
Azure Load Balancer is a service you can use to distribute traffic across multiple virtual machines. Use Load Balancer to scale applications and create high availability for your virtual machines and services. Load balancers use a hash-based distribution algorithm. By default, a five-tuple has is used to map traffic to available servers. The hash is made from the following elements:
- **Source IP**: The IP address of the requesting client.
- **Source port**: The port of the requesting client.
- **Destination IP**: The destination IP of the request.
- **Destination port**: The destination port of the request.
- **Protocol type**: The specified protocol type, TCP or UDP.

Load Balancer supports inbound and outbound scenarios, provides low latency and high throughput, and scales up to millions of flows for TCP and UDP applications.

Load balancers aren't physical instances. Load balancer objects are used to express how Azure configures its infrastructure to meet your requirements.

## Availability sets
An availability set is a logical grouping that you use to isolate virtual machine resources from each other when they're deployed. Azure ensures that the virtual machines you put in an availability set run across multiple physical servers, compute racks, storage units, and network switches. If there's a hardware or software failure, only a subnet of your virtual machines is affected. Your overall solution stays operational. Availability sets are essential for building reliable cloud solutions.

## Availability zones
An availability zone offers groups of one or more datacenters that have independent power, cooling, and networking. The virtual machines in an availability zone are placed in different physical locations within the same region. Use this architecture when you want to ensure that, when an entire datacenter fails, you can continue to serve users.

Availability zones don't support all virtual machine sizes and aren't available in all Azure regions. Check that they're supported in your region before you use them in your architecture.

## Select the right Load Balancer product
Two products are available when you create a load balancer in Azure: basic load balancers and standard load balancers.

Basic load balancers allow:
- Port forwarding
- Automatic reconfiguration
- Health probes
- Outbound connections through source network address translation (SNAT)
- Diagnostics through Azure Log Analytics for public-facing load balancers

Basic load balancers can be used only with a single availability set or scale set.

Standard load balancers support all of the basic load balancer features. They also allow:
- HTTPS health probes
- Availability zones
- Diagnostics through Azure Monitor, for multidimensional metrics
- High availability (HA) ports
- Outbound rules
- A guaranteed SLA (99.99% for two or more VMs)

## Internal and external load balancers
An external load balancer operates by distributing client traffic across multiple virtual machines. An external load balancer permits traffic from the internet. The traffic might come from browsers, mobile apps, or other sources.

An internal load balancer distributes a load from internal Azure resources to other Azure resources. 

## Knowledge check
1. What is the default distribution type for traffic through a load balancer? -> Five-tuple hash.
2. What is the main advantage of an availability set? -> It allows virtual machines to be available across physical server failures.

<hr>

## Configure a public load balancer
A public load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of a virtual machine in the back-end pool. The responses are then returned to the client. By applying load-balancing rules, you distribute specific types of traffic across multiple virtual machines or services.

## Distribution modes
By default, Azure Load Balancer distributes network traffic equally among virtual machine instances. The following distribution modes are also possible if a different behavior is required:

- **Five-tuple hash**: The default distribution mode for Load Balancer is a five-tuple hash. The tuple is composed of source IP, source port, destination IP, destination port, and protocol type. Because the source port is included in the hash and the source port changes for each session, clients might be directed to a different virtual machine for each session.

- **Source IP affinity**: This distribution mode is also known as *session affinity* or *client IP affinity*. To map traffic to the available servers, the source IP affinity mode uses a two-tuple hash (from the source IP address and destination IP address) or a three-tuple hash (from the source IP address, destination address, and protocol type). The hash ensures that requests from a specific client are always sent to the same virtual machine behind the load balancer.

## Load Balancer and Remote Desktop Gateway
Remote Desktop Gateway is a Windows service that you can use to enable clients on the internet to make Remote Desktop Protocol (RDP) connections through firewalls to Remove Desktop servers on your private network. The default five-tuple has in Load Balancer is incompatible with this service. If you want to use Load Balancer with your Remote Desktop servers, use source IP affinity.

## Load Balancer and media upload
Another use case for source IP affinity is media upload. In many implementations, a client initiates a session through a TCP protocol and connects to a destination IP address. This connection remains open throughout the upload to monitor progress, but the file is uploaded through a separate UDP protocol.

With the five-tuple hash, the load balancer likely will send the TCP and UDP connections to different destination IP addresses and the upload won't finish sucessfully. Use source IP affinity to resolve this issue.

<hr>

## Internal load balancer
In addition to balancing requests from users to front-end servers, you can use Azure Load Balancer to distribute traffic from front-end servers evenly among back-end servers.

## Configure an internal load balancer
You can configure an internal load balancer in almost the same way as an external load balancer, but with these differences:
- When you create the load balancer, select **Internal** for the **Type** value. When you select this setting, the front-end IP address of the load balancer isn't exposed to the internet.
- Assign a private IP address instead of a public IP address for the front end of the load balancer.
- Place the load balancer in the protected virtual network that contains the virtual machines you want to handle the requests.

The internal load balancer should be visible only to the web tier. All the virtual machines that host the databases are in one subset. You can use an internal load balancer to distribute traffic to those virtual machines.

## Check your knowledge
1. Which configuration is required to configure an internal load balancer? -> Virtual machines should be in the same virtual network.
2. Which of the following statement about external load balancers is correct? -> They have a public IP address.

<hr>
<hr>

## Monitor and back up Azure resources

<hr>

## Protect virtual machine data
You can protect your data by taking backups at regular intervals. There are several backup options available for VMs, depending on your use-case.

1. Snapshots
2. Azure Backup
3. Azure Site Recovery

## Azure Backup
For backing up Azure VMs running production workloads, use Azure Backup. Azure Backup supports application-consistent backups for both Windows and Linux VMs. Azure Backup creates recovery points that are stored in geo-redundant recovery vaults. When you restore from a recovery point, you can restore the whole VM or just specific files.

## Azure Site Recovery
Azure Site Recovery protects your VMs from a major disaster scenario when a whole region experiences an outage due to major natural disaster or widespread service interruption. You can configure Azure Site Recovery for your VMs so that you can recover your application with a single click in matter of minutes. You can replicate to an Azure region of your choice.

## Managed disk snapshots
In development and test environments, snapshots provide a quick and simple option for backing up VMs that use Managed Disks. A managed disk snapshot is a read-only full copy of a managed disk that is stored as a standard managed disk by default. With snapshots, you can back up your managed disks at any point in time. These snapshots exist independent of the source disk and can be used to create new managed disks. They are billed based on the used size. For example, if you create a snapshot of a managed disk with provisioned capacity of 64 GiB and actual used data size of 10GiB, that snapshot is billed only for the used data size of 10 GiB.

## Images
Managed disks also support creating a managed custom image. You can create an image from your custom VHD in a storage account or directly from a generalized (sysprepped) VM. This process captures a single image. This image contains all managed disks associated with a VM, including both the OS and data disks. This managed custom image enables creating hundreds of VMs using your custom image without the need to copy or manage any storage accounts.

## Images vs snapshots
With managed disks, you can take an image of a generalized VM that has been deallocated. This image includes all of the disks attached to the VM. You can use this image to create a VM, and it includes all of the disks.

- A snapshot is a copy of a disk at the point in time the snapshot is taken. It applies only to one disk. If you have a VM that has one disk (the OS disk), you can take a snapshot or an image of it and create a VM from either the snapshot or the image.
- A snapshot doesn't have awareness of any disk except the one it contains. This makes it problematic to use in scenarios that require the coordination of multiple disks, such as striping. Snapshots would need to be able to coordinate with each other and this is currently not supported.

<hr>

## Create virtual machine snapshots
An Azure backup job consists of two phases. First, a virtual machine snapshot is taken. Second, the virtual machine snapshot is transferred to the Azure Recovery Services vault.

A recovery point is considered created only after both steps are completed. As a part of the upgrade, a recovery point is created as soon as the snapshot is finished. The recovery point is used to perform a restore. You can identify the recovery point in the Azure portal by using "snapshot" as the recovery point type. After the snapshot is transferred to the vault, the recovery point type changes to "snapshot and vault".

## Capabilities and considerations
- Ability to use snapshots taken as part of a backup job that is available for recovery without waiting for data transfer to the vault to finish.
- Reduces backup and restore times by retaining snapshots locally, for two days by default. This default snapshot retention value is configurable to any value between 1 and 5 days.
- Support disk sizes up to 32 TB. Resizing of disks is not recommended by Azure Backup.
- Supports Standard SSD disk along with Standard HDD disks and Premium SSD disks.
- Incremental snapshots are stored as page blobs. All the users using unmanaged disks are charged for the snapshots in their local storage account. Since the restore point collections used by Managed VM backups use blob snapshots at the underlying storage level, for managed disks you wlil see costs corresponding to blob snapshot pricing and they are incremental.
- For premium storage accounts, the snapshots taken for instant recovery points count towards the 10-TB limit of allocated space.
- You get an ability to configure the snapshot retention based on the restore needs. This will help you save cost for snapshot retention if you don't perform restores frequently.

Note: By default, snapshots are retained for two days. This feature allows restore operation from these snapshots there by cutting down the restore times. It reduces the time that is required to transform and copy data back from the vault.

<hr>

## Setup recovery services vault backup options
**Recovery Services vault** is a storage entity in Azure that houses data. The data is typically copies of data, or configuration information for virtual machines (VMs), workloads, servers, or workstations. You can use Recovery Services vaults to hold backup data for various Azure services such as IaaS VMs (Linux or Windows) and Azure SQL databases. Recovery Services vaults support System Center DPM, Windows Server, Azure Backup Server, and more. Recovery Services vaults make it easy to organize your backup data, while minimizing management overhead.

- The Recovery Services vault can be used to backup Azure machines.
- The Recovery Services vault can be used to backup on-premises virtual machines including: Hyper-V, VMware, System State, and Bare Metal Recovery.

<hr>

## Backup virtual machines
Backing up Azure virtual machines using Azure Backup is easy and follows a simple process.

1. **Create a recovery services vault**: To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data. You also need to determine how you want your storage replicated, either geo-redundant (default) or locally redundant. By default, your vault has geo-redundant storage. If you are using Azure as a primary backup storage endpoint, use the default geo-redundant storage. If you are using Azure as a non-primary backup storage endpoint, then choose locally redundant storage, which will reduce the cost of storing data in Azure.
2. **Use the Portal to define the backup**: Protect your data by taking snapshots of your data at defined intervals. These snapshots are known as recovery points, and they are stored in recovery service vaults. If or when it is necessary to repair or rebuild a VM, you can restore the VM from any of the saved recovery points. A backup policy defines a matrix of when the data snapshots are taken, and how long those snapshots are retained. When defining a policy for backing up a VM, you can trigger a backup job once a day.
3. **Backup the virtual machine**: The Azure VM Agent must be installed on the Azure virtual machine for the Backup extension to work. However, if your VM was created from the Azure gallery, then the VM Agent is already present on the virtual machine. VMs that are migrated from on-premises data centers would not have the VM Agent installed. In such a case, the VM Agent needs to be installed.

## Restore virtual machines
Once your virtual machine snapshots are safely in the recovery services vault it is easy to recover them.

Once your trigger the restore operation, the Backup service creates a job for tracking the restore operation. The Backup service also creates and temporarily displays notification, so you monitor how the backup is proceeding.

<hr>

## Implement Azure Backup Server
Another method of backing up virtual machines is using a Data Protection Manager (DPM) or Microsoft Azure Backup Server (MABS) server. This method can be used for specialized workloads, virtual machines, or files, folders, and volumes. Specialized workloads can include SharePoint, Exchange, and SQL server.

## Advantages
The advantages of backing up machines and apps to MABS/DPM storage, and then backing up DPM/MABS storage to a vault are as follows:
- Backing up to MABS/DPM provides app-aware backups optimized for common apps. Tese apps include SQL Server, Exchange, and SharePoint. Also, file/folder/volume backups, and machine state backups. Machine state backups can be bare-metal or system state.
- For on-premises machines, you don't need to install the MARS agent on each machine you want to back up. Each machine runs the DPM/MABS protection agent, and the MARS agent runs on the MABS/DPM only.
- You have more flexibility and granular scheduling options for running backups.
- You can manage backups for multiple machines that you gather into protection groups in a single console. Grouping machines is useful when apps are tiered over multiple machines and you want to back them up at the same time.

## Backup steps
1. Install the DPM or MABS protection agent on machines you want to protect. You then add the machines to a DPM protection group.
2. To protect on-premises machines, the DPM or MABS server must be located on-premises.
3. To protect Azure VMs, the MABS server must be located in Azure, running as an Azure VM.
4. With DPM/MABS, you can protect backup volumes, shares, files, and folders. You can also protect a machine's system state (bare metal), and you can protect specific apps with app-aware backup settings.
5. When you set up protection for a machine or apps in DPM/MABS, you select to back up to the MABS/DPM local disk for short-term storage and to Azure for online protection. You also specify when the backup to local DPM/MABS storage should run and when the online backup to Azure should run.
6. The disk of the protected workload is backed up to the local MABS/DPM disks, according to the schedule you specified.
7. The DPM/MABS disks are backed up to the vault by the MARS agent that's running on the DPM/MABS server.

## Manage soft delete
Azure Storage now offers soft delete for blob objects so that you can more easily recover your data when it is erroneously modified or deleted by an application or other storage account user. Soft delete for VMs protects the backups of your VMs from unintened deletion. Even after the backups are deleted, they're preserved in soft-delete state for 14 additional days.

## How soft delete works for virtual machines
1. To delete the backup data of a VM, the backup must be stopped.
2. You can then choose to delete or retain the backup data. If you choose to **Delete backup data** and then **Stop backup**, the VM backup won't be permanently deleted. Rather, the backup data will be retained for 14 days in the soft deleted state.
3. During those 14 days, in the Recovery Services vault, the soft deleted VM will appear with a red **soft-delete** icon next to it. If any soft-deleted backup items are present in the vault, the vault can't be deleted at that time. Try deleting the vault after the backup items are permanently deleted, and there are no items in soft deleted state left in the vault.
4. To restore the soft-deleted VM, it must first be undeleted. To undelete, choose the soft-deleted VM, and then select the option **Undelete**. At this point, you can also restore the VM by selecting **Restore VM** from the chosen restore point.
5. After the undelete process is completed, the status will return to **Stop backup with retain data** and then you can choose **Resume backup**. The Resume backup operation brings back the backup item in the active state, associated with a backup policy selected by the user defining the backup and retention schedules.

Note: Soft delete only protects deleted backup data. If a VM is deleted without a backup, the soft-delete feature won't preserve the data. All resources should be protected with Azure Backup to ensure full resilience.

<hr>

## Implement Azure Site Recovery
Site Recovery helps ensure business continuity by keeping business apps and workloads running during outages. Site Recovery replicates workloads running on physical and virtual machines (VMs) from a primary site to a secondary location. When an outage occurs at your primary site, you fail over to secondary location, and access apps from there. After the primary location is running again, you can fall back to it.

## Replication scenarios
- Replicate Azure VMs from one Azure region to another.
- Replicate on-premises VMware VMs, Hyper-V VMs, physical servers (Windows and Linux), Azure Stack VMs to Azure.
- Replicate AWS Windows instances to Azure.
- Replicate on-premises VMware VMs, Hyper-V VMs managed by System Center VMM, and physical servers to a secondary site.

## Features
- Using Site Recovery, you can set up and manage replication, failover, and failback from a single location in the Azure portal.
- Replication to Azure eliminates the cost and complexity of maintaining a secondary datacenter.
- Site Recovery orchestrates replication without intercepting application data. When you replicate to Azure, data is stored in Azure storage, with the resilience that it provides. When failover occurs, Azure VMs are created, based on the replicated data.
- Site Recovery provides continuous replication for Azure VMs and VMware VMs, and replication frequency as low as 30 seconds for Hyper-V.
- You can replicate using recovery points with application-consistent snapshots. These snapshots capture disk data, all data in memory, and all transactions in process.
- You can run planned failovers for expected outages with zero-data loss, or unplanned failover with minimal data loss (depending on replication frequency) for unexpected disasters. You can easily fall back to your primary site when it's available again.
- Site Recovery integrates with Azure for simple application network management, including reserved IP addresses, configuring load-balancing, and integrating Azure Traffic Manager for efficient network switchovers.

<hr>

## Knowledge check
1. A company has several Azure VMs that are currently running production workloads. There is a mix of production Windows Server and Linux servers. What would be the choice for production backups? -> Azure Backup (best option for production workloads)
2. What option is recommended to backup a database disk used for deployment? -> Disk snapshot (readonly and specifically for dev and test envs)
3. A malware attak has deleted several virtual machine backups. How long are items available in the soft delete state? -> 14 days.

<hr>

## Configure Azure Monitor
You will learn how to configure Azure Monitor including querying the activity log.

<hr>

## Skills measured
Monitor resources by using Azure resources
- Configure and interpret metrics
- Configure Azure Monitor logs

<hr>

## Describe Azure Monitor key capabilities
- **Monitor and visualize metrics**: Metrics are numerical values available from Azure resources helping you understand the health, operation and performance of your system.
- **Query and analyze logs**: Logs are activity logs, diagnostic logs, and telemetry from monitoring solutions; analytics queries help with troubleshooting and visualizations.
- **Setup alerts and actions**: Alerts notify you of critical conditions and potentially take automated corrective actions based on triggers from metrics or logs.

<hr>

## Metrics
For many Azure resources, the data collected by Azure Monitor is displayed on the Overview page in the Azure portal.

## Logs
Log data collected by Azure Monitor is stored in Log Analytics which includes a rich query language to quickly retrieve, consolidate, and analyze collected data. You can create test queries using the Log Analytics page in the Azure portal. You can use the query results to directly analyze the data, save queries, visualize the data, or create alert rules.

Azure Monitor uses a version of the Data Explorer query language that is suitable for simple log queries but also includes advanced functionality such as aggregations, joins, and smart analytics.

<hr>

## Identify data types
Azure Monitor collects data from each of the following tiers:
- **Application monitoring data**: Data about the performance and functionality of the code you have written, regardless of its platform.
- **Guest OS monitoring data**: Data about the operating system on which your application is running. The application could be running in Azure, another cloud, or on-premises.
- **Azure resource monitoring data**: Data about the operation of an Azure resource.
- **Azure subscription monitoring data**: Data about the operation and management of an Azure subscription, as well as data about the health and operation of Azure itself.
- **Azure tenant monitoring data**: Data about the operation of tenant-level Azure services, such as Azure Active Directory.

Azure Monitor starts collecting data as soon as you create an Azure subscription and add resources. Activity Logs record when resources are created or modified.

Note: Azure Monitor can collect log data from any REST client using the Data Collector API. The Data Collector API lets you create custom monitoring scenarios and extend monitoring to resources that don't expose data through other resources.

<hr>

## Describe activity log events
The Azure Activity Log is a subscription log that provides insight into subscription-level events that have occurred in Azure. This includes a range of data, from Azure Resource Manager operational data to updates on Service Health events.

With the Activity Log, you determine the 'who, what, and when' for any write operations taken on the resources in your subscription.

Note: Activity logs are kept for 90 days. You can query for any range of dates, as long as the starting date isn't more than 90 days in the past. You can retrieve events from your Activity Log using the Azure portal, CLI, PowerShell cmdlets, and Azure Monitor REST API.

<hr>

## Knowledge check
1. Someone has deleted a network security group through Azure Resource Manager. What category would include this information? -> Administrative (create, update, delete, and action operations).
2. What data does Azure Monitor collect? -> Data from many different sources, such as application event logs.
3. How long are activity logs kept? -> 90 days.

<hr>

## Alert states
You can set the state of an alert to specify where it is in the resolution process. When the criteria specified in the alert rule is met, an alert is created or fired, it has a status of **New**. You can change the status when you acknowledge an alert and when you choose it. All state changes are stored in the history of the alert. The following alert states are supported:

- **New**: The issue has been detected and has not yet been reviewed.
- **Acknowledged**: An administrator has reviewed the alert and stared working on it.
- **Closed**: The issue has been resolved. After an alert has been closed you can reopen it by changing it to another state.

Note: Alert state is different and independent of the monitor condition. Alert state is set by the user. Monitor condition is set by the system. When an alert fires, the alert's monitor condition is set to fired. When the underlying condition that caused the alert to fire clears, the monitor condition is set to resolved. The alert state isn't changed until the user changes it.

<hr>

## Create action groups
An action group is a collection of notification preferences defined by the owner of an Azure subscription. Azure Monitor and Service Health alerts use action groups to notify users that an alert has been triggered. Various alerts may use the same action group or different action groups depending on the user's requirements.

**Notifications** configure the method in which uses will be notified when the action group triggers.
- **Email Azure Resource Manager role**: Send email to the members of the subscription's role. Email will only be sent to Azure AD user members of the role. Email will not be sent to Azure AD groups or service principals.
- **Email/SMS message/Push/Voice**: Specify any email, SMS, push, or voice actions.

**Actions** configure the method in which actions are performed when the action group triggers.
- **Automation runbook**: The ability to define, build, orchestrate, manage, and report on workflows that support system and network operational processes. A runbook workflow can potentially interact with all types of infrastructure elements, such as applications, databases, and hardware.
- **Azure Function**
- **ITSM**: Connect Azure and a supported IT Service Management (ITSM) product/service. This requires an ITSM Connection.
- **Webhook**: A webhook is a HTTPS or HTTP endpoint that allows external applications to communicate with your system.

<hr>

## Knowledge check
1. The Alerts page has an alert marked as Acknowledged. What does this status indicate? -> An administrator has reviewed the alert and started working on it.
2. A major retailer has an app that is used across the business. The performance of this app is critical to day-to-day operations. Because the app is so important, four infrastructure administrators have been identified to address any issues. What would ensure the administrators are notified if there is a problem? -> Configure an Action Group.
3. An alert rule consists of which of the following? -> Resource, condition, actions, alert details.

<hr>

## Configure Log Analytics
Access Log Analytics through Azure Monitor. To get started with Log Analytics you need to add a workspace.

- Provide a name for the new Log Analytics workspace.
- Select a Subscription form the drop-down list.
- For Resource Group, select an existing resource group that contains one or more Azure virtual machines.
- Select the Location your VMs are deployed to.
- The workspace will automatically use the Per GB pricing plan.

## Structure Log
When you build a query, you start by determining which tables have the data that you're looking for. Each data source and solution stores its data in dedicated tables in the Log Analytics workspace. Documentation for each data source and solution includes the name of the data type that it creates and a description of each of its properties. Many queries will only require data from a single table, but others may use a variety of options to include data from multiple tables.

Some common query tables are: Event, Syslog, Heartbeat, and Alert.

The basic structure of a query is a source table followed by a series of operators separated by a pipe character. You can chain together multiple operators to refine the data and perform advanced functions.

<hr>

## Knowledge check
1. Azure Monitor organizes log data into which of the following? -> Tables
2. An online retailer has a very large web farm with more than 100 virtual machines. They would like to use Log Analytics to ensure these machines are responding to requests. To automate the process, the team will create a search query. Which source table should be used? -> Heartbeat
3. Log analytics agents can run on which of the following? -> On many different platforms including other cloud providers.

<hr>

## Configure Network Watcher
When you have to diagnose network problems in Azure, use Azure Network Watcher. Administrators use Network Watcher to monitor, diagnose, and gain insight into their network health and performance with metrics. The elements can be broken down into four areas: monitoring, network diagnostic tools, metrics, and logs. Additionally, Network Watcher provides tools for troubleshooting connection problems.

<hr>

## Describe Network Watcher features
Network Watcher provides tools to monitor, diagnose, view metrics, and enable or disable logs for resources in an Azure virtual network. Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level.

- **Automate remote network monitoring with packet capture**: Monitor and diagnose networking issues without logging in to your VMs using Network Watcher. Trigger packet capture by setting alerts, and gain access to real-time performance information at the packet level. When you observe an issue, you can investigate in detail for better diagnostics.

- **Gain insight into your network traffic using flow logs**: Build a deeper understanding of your network traffic pattern using Network Security Group flow logs. Information provided by flow logs helps you gather dtaa for compliance, auditing and monitoring your network security profile.

- **Diagnose VPN connectivity issues**: Network Watcher provides you the ability to diagnose your most common VPN Gateway and Connections issues. Allowing you, not only, to identify the issue but also to use the detailed logs created to help further investigate.

- **Verify IP Flow**: Quickly diagnose connectivity issues from or to the internet and from or to the on-premises environment.

- **Next Hop**: To determine if traffic is being directed to the intended destination by showing the next hop. This will help determine if networking routing is correctly configured. Next hop also returns the route table associated with the next hop. If the route is defined as a user-defined route, that route is returned. Otherwise, next hop returns System Route. Depending on your situation the next hop could be internet, Virtual Appliance, Virtual Network Gateway, VNet Local, VNet Peering, or None. None lets you know that while there may be a valid system route to the destination, there is no next hop to route the traffic to the destination. When you create a virtual network, Azure creates several default outbound routes for network traffic. The outbound traffic from all resources, such as VMs, deployed in a virtual network, are routed based on Azure's default routes. You might override Azure's default routes or create additional routes.

Note: To use Network Watcher, you must be an Owner, Contributor, or Network Contributor. If you create a custom role, the role must be able to read, write, and delete the Network Watcher.

<hr>

## Review flow verify diagnostics 
**IP Flow Verify Purpose**: Checks if a packet is allowed or denied to or from a virtual machine. For example, confirming if a security rule is blocking ingress or egress traffic to or from a virtual machine.

The IP Flow Verify capability enables you to specify a source and destination IPv4 address, port, protocol (TCP or UDP), and traffic direction (inbound and outbound). IP Flow Verify then tests the communication and informs you if the connection succeeds or fails. If the connection fails, IP Flow Verify identifies which security rule allowed or denied the communication.

Can be found in 'Network Diagnostic Tools' in Azure Portal.

<hr>

## Review next hop diagnostics
**Next Hop Purpose**: To determine if traffic is being directed to the intended destination. Next hop information will help determine if network routing is correctly configured.

When you create a virtual network, Azure creates several default outbound routes for network traffic. The outbound traffic from all resources, such as VMs, deployed in a virtual network, are routed based on Azure's default routes. You might override Azure's default routes or create additional resources.

<hr>

## Visualize the network topology
You can use the topology tool to visualize and understand the infrastructure you're dealing with before you start troubleshooting.

Network Watcher's Topology capability enables you to generate a visual diagram of the resources in a virtual network, and the relationships between the resources. The topology tool generates a graphical display of your Azure virtual network, its resources, its interconnections, and their relationships with each other.

Note: To generate the topology, you need a Network Watcher instance in the same region as the virtual network.

<hr>

## Knowledge check
1. The infrastructure team thinks it would be helpful to get a visual representation of the company's networking elements. Which Network Watcher feature provides this capability? -> Network Watcher Topology.
2. Users are reporting connectivity errors and timeouts. The help desk thinks a security rule may be blocking traffic to or from one of the virtual machines. To quicky troubleshoot the problem use which Network Watcher feature? -> IP Flow Verify
3. Which of the following is a good use for Network Watcher? -> Diagnose network traffic filtering problems to or from a virtual machine.

<hr>

## Data types in Azure Monitor
Azure Monitor receives data from target resources like applications, operating systems, Azure resources, Azure subscriptions, and Azure tenants. The nature of the resource defines which data types are available. A data type will be a *metric*, a *log*, or both a metric and a log:
- The focus for *metric*-based data types is the numerical time-sensitive values that represent some aspect of the target resource.
- The focuse for *log*-based data types is the querying of content data held in structured, record-based log files that are relevant to the target resource.

- **Metric** alerts provide an alert trigger when a specified threshold is exceeded. For example, a metric alert can notify you when CPU usage is greater than 95 percent.
- **Activity log** alerts notify you hwne Azure resources change state. For example, an activity log alert can notify you when a resource is deleted.
- **Log** alerts are based on things written to log files. For example, a log alert can notify you when a web server has returned a number of 404 or 500 responses.

## Composition of an alert rule
Every alert or notification in Azure Monitor is the product of a rule. Some of these rules are built into the Azure platform. You use alert rules to create custom alerts and notifications. No matter which target resource or data source you use, the composition of an alert rule remains the same.

- **Resource**: The target resource to be used for the alert rule. It's possible to assign multiple target resources to a single start rule. The type of resource will define the available signal types.
- **Condition**: The signal type to be used to assess the rule. The signal type can be a metric, an activity log, or logs. The alert logic applied to the data that's supplied via the signal type. The structure of the alert logic will change depending on the signal type.
- **Actions**: The action, like sending an email, sending an SMS message, or using a webhook. An action group, which typically contains a unique set of recipients for the action.
- **Alert Details**: An alert name and an alert description that should specify the alert's purpose. The severity of an alert if the criteria or logic test evaluates `true`. The five severity levels are:
0: Critical
1. Error
2. Warning
3. Informational
4. Verbose

## Manage alert rules
With Azure Monitor, you can specify one or more alert rules, and enable or disable them as needed.

## Knowledge check
1. What is the composition of an alert rule? -> Resource, condition, actions, alert details.
2. Which of the following is an example of a log data type? -> HTTP response records.

<hr>

## Use dynamic threshold metric alerts
Dynamic metric alerts use machine-learning tools that Azure provides to automatically improve the accuracy of the thresholds defined by the initial rule.

There's no hard threshold in dynamic metrics. However, you'll need to define two more parameter:
- The *look-back period* defines how many previous periods need to be evaluated. For example, if you set the look-back period to 3, then in the example used here, the assessed data range would be 30 minutes (three sets of 10 minutes).
- The *number of violations* expresses how many times the logic condition has to deviate from the expected behavior before the alert rule fires a notification. In this example, if you set the number of violations to two, the alert would be triggered after two deviations from the calculated threshold.

## Understand dimensions
You can use dimensions to define one metric alert rule and have it applied to multiple related instances. For example, you can monitor CPU utilization across all the servers running your app. You can then receive an individual notification for each server instance when the rule conditions are triggered.

## Scale metric alerts
Scaling is currently limited to Azure virtual machines. However, a single metric alert can monitor resources in one Azure region.

Like dimensions, a scaling metric alert is individual to the resource that triggered it.

<hr>

## Composition of log search rules
Every log alert has an associated search rule. The composition of these rules is as follows:
- **Log query**: Query that runs every time the alert rule fires.
- **Time period**: Time range for the query.
- **Frequency**: How often the query should run.
- **Threshold**: Trigger point for an alert to be created.

Log search results are one of two types: number of records or metric measurement.

## Number of records
Consider using the number-of-records type of log search when you're working with an event of event-driven data. Examples are syslog and web-app responses.

This type of log search returns a single alert when the number of records in a search result reaches or exceeds the value for the number of records (threshold). For example, when the threshold for the search rule is greater or equal to five, the query results have to return five or more rows of data before the alert is triggered.

## Metric measurement
Metric measurement logs offer the basic functionality as metric alert logs.

Unlike number-of-records search logs, metric measurement logs require additional criteria to be set:

- **Aggregate function**: The calculation that will be made against the result data. An example is count or average. The result of the function is called **AggregatedValue**.
- **Group field**: A field by which the result will be grouped. This criterion is used with the aggregated value. For example, you might specify that you want the average grouped by computer.
- **Interval**: The time interval by which data is aggregated. For example, if you specify 10 minutes, an alert record is created by each aggregated block of 10 minutes.
- **Threshold**: A point defined by an aggregated value and the total number of breaches.

Consider using this type of alert when you need to add a level of tolerance to the results found. One use for this type of alert is to respond if a particular trend or pattern is found. For example, if the number of breaches is five, and any server in your group exceeds 85 percent CPU utilization more than five times within a given period, an alert is fired.

## Stateless nature of log alerts
One of the primary considerations when you're evaluating the use of log alerts is that they're stateless (stateful log alerts are currently in preview). A stateless log alert will generate new alerts every time the rule criteria are triggered, regardless of whether the alert was previously recorded.

<hr>

## Use activity log alerts to alert on events within your Azure infrastructure
Activity log alerts allow you to be notified when a specific event happens on some Azure resource. For example, you can be notified when someone creates a new VM in a subscription.

## When to use activity log alerts
Metric alerts are ideally suited to monitoring for threshold breaches or spotting trends; log alerts allow for greater analytical monitoring of historical data.

There are two types of activity log alerts:
- **Specific operations**: Applies to resources within your Azure subscription, and often has a scope with specific resources or a resource group. You'll use this type when you need to receive an alert that reports a change to an aspect of your subscription. For example, you can receive an alert if a VM is deleted or new roles are assigned to a user.
- **Service health events**: Include notice of incidents and maintenance of target resources.

## Composition of an activity log alert
Activity log alerts will monitor events only in the subscription where the log alert was created.

To begin the creation process, you'll then select **Add activity log alert**. 

Like the previous alerts, activity log alerts have their own attributes:
- **Category**: Administrative, service health, autoscale, policy, or recommendation.
- **Scope**: Resource level, resource group level, or subscription level.
- **Resource group**: Where the alert rule is saved.
- **Resource type**: Namespace for the target of the alert.
- **Operation name**: Operation name.
- **Level**: Verbose, informational, warning, error, or critical.
- ***Status**: Started, failed, or succeeded.
- **Event initiated by**: Email address or Azure Active Directory identifier (known as the "caller") for the user.

## Create a resource-specific log alert
When you create your activity log alert, you'll select **Activity Log** for the signal type. You'll then see all the available alerts for the resource you select. 

## Create a service health alert
To create a new alert, on the Azure portal, search for and select **Service Health**. Next, select **Health alerts**. After you select **Create service health alert**, the steps to create the alert are similar to the steps you've seen to create other alerts.

The only difference is that you no longer need to select a resource, because the alert is for a whole region in Azure. What you can select is the kind of health event on which you want to be alerted. It's possible to select service issues, planned maintenance, or health advisories, or choose all of the events. The remaining steps of performing actions and naming the alerts are the same.

## Perform actions when an alert happens
When any event is triggered, you can create an associated action in an action group. **Action groups allow you to define actions that will be run.** You can run one or more actions for each triggered alert.

The available actions are:
- Send an email
- Send an SMS message
- Create an Azure app push notification
- Make a voice call to a number
- Call an Azure function
- Trigger a logic app
- Send a notification to a webhook
- Create ITSM ticket
- Use a runbook (to restart a VM, or scale a VM up or down)

You can also reuse action groups on multiple alerts after you've created them. For example, after you've created an action to email your company's operations team, you can add that action group to all the service health events.

You can add or create action groups at the same time that you create your alert. You can also edit an existing alert to add an action group after you've created it.

<hr>

## What are smart groups?
Smart groups are an automatic feature of Azure Monitor. By using machine learning algorithms, Azure Monitor joins alerts based on repeat occurrence or similarity. Smart groups let you address a group of alerts instead of each alert individually.

The name of the smart group (its taxonomy) is assigned automatically, and is the name of the first alert in the group. It's important to assign meaningful names to each alert that you create, because the name of the smart group can't be changed or amended.

## When to use smart groups
Think of smart groups as a dynamic filter applied to all the alerts in Azure Monitor. The machine-learning algorithm in Azure Monitor joins alerts based on information, such as historical patterns, similar properties, or structure. Using smart groups can reduce alert noise by more than 90 percent.

The power of smart groups is that they show you all related alerts and give improved analytics. They can often identify a perviously unseen root cause.

## Manage smart groups
There are two ways to get to your smart groups: from the **Alert Summary** pane or from the **All Alerts** pane. Next, select **Alerts by Smart Group**.

Either method results in a new page that shows all the smart groups. Selecting a smart group opens its details page, which splits into two sections:
- **Summary**: Lists all the alerts included in the smart group.
- **History**: Provides a history of all the changes made to the smart group.

## Smart group states
Smart groups, like regular alerts, have their own state. The state shows the progress of the smart group in the resolution process. Changing the state of a smart group doesn't alter the state of the individual alerts.

To change the state, select **Change smart group state**.

The states are:
- **New**: The smart group has been created with a collection of alerts, but it hasn't yet been addressed.
- **Acknowledged**: When an admin starts the resolution process, they change to this state.
- **Closed**: When the source of the alert is fixed, the admin changes to this state.

Changing the state of the smart group doesn't affect the underlying alert. Each alert member shown in the smart group can have a different state.

## Knowledge check
1. How are smart groups created? -> Automatically, using machine learning algorithms.
2. Which of the following is NOT a state of a smart group alert? -> Failed.

<hr>

## Analyzing logs by using Kusto
To retrieve, consolidate, and analyze data, you can specify a query to run in Azure Monitor logs. You can write a log query with the Kusto query language, which Azure Data Explorer also uses.

<hr>

## Create basic Azure Monitor log queries to extract information from log data
You can use Azure Monitor log queries to extract information from log data. Querying is an important part of examining the log data that Azure Monitor captures.

## Write Azure Monitor log queries by using Log Analytics
You can find the Log Analytics tool in the Azure portal and use it to run sample queries or to create your own queries:

1. In the Azure portal, in the left menu pane, select **Menu**.
2. Select **Query & Analyze Logs*.

## Write queries by using the Kusto language
You use the Kusto Query Language to query log information for your services running in Azure. A Kusto query is a read-only request to process data and return results. You'll state the query in plain text by using a data-flow model that's designed to make the syntax easy to read, write, and automate. The query uses schema entities that are organized in a hierarchy similar to that of Azure SQL Database: databases, tables, and columns.

A Kusto query consists of a sequence of query statements, delimited by a semicolon. At least one statement is a tabular expression statement. A tabular expression statement formats the data arranged as a table of columns and rows. 

The syntax of a tabular expression statement has a tabular data flow from one tabular query operator to another, starting with a data source. A data source might be a table in a database, or an operator that produces data. The data then flows through a set of data-transformation operators that are bound together with the pipe delimiter.

For example, the following Kusto query has a single tabular expression statement. The statement starts with a reference to a table called `Events`. The database that hosts this table is implicit here, and is part of the connection information. The data for that table, stored in rows, is filtered by the value of the `StartTime` column. The data is filtered further by the value of the `State` column. The query then returns the count of the resulting rows.

        Events
        | where StartTime >= datetime(2018-11-01) and StartTime < datetime(2018-12-01)
        | where State == 'FLORIDA'
        | count

Note: The Kusto query language is case-sensitive. Language keywords are typically written in lowercase. When you're using names of tables or columns in a query, make sure to use the correct case.

The following example retrieves the most recent heartbeat record for each computer. The computer is identified by its IP address. In this example, the `summarize` aggregation with the `arg_max` function returns the record with the most recent value for each IP address.

        Heartbeat
        | summarize arg_max(TimeGenerated, *) by ComputerIP