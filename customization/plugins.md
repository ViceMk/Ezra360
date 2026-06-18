# Plugins

## 1. Overview: Plugins

### 1.1 What is a Plugin?

A Plugin is a class that implements the IXrmPlugin interface. It is triggered automatically by the&#x20;Ezra360 platform when a specific event occurs on a specific entity — for example, when a record is&#x20;created, updated, or deleted.\
Plugins run as part of the entity lifecycle. They receive a **PluginExecutionContext** object that provides&#x20;access to the triggering record and platform services.

#### Common use cases for plugins:

* Auto-creating related child records when a parent is saved
* Enforcing business rules before a record is committed to the database
* Sending notifications or emails when a record reaches a certain state
* Calculating rollup or summary values on related records
* Preventing deletion of records that have active dependencies

#### Plugin (IXrmPlugin) Characteristics

* [ ] Triggered automatically by entity events
* [ ] Implements IXrmPlugin
* [ ] Return type: void
* [ ] Execution step: PostUpdate, PreCreate, etc.
* [ ] Access: PluginExecutionContext
* [ ] Example: Enforce approval rules on save

## 2. Prerequisites & NuGet Packages

Before writing any code, ensure your development environment is properly set up.

### 2.1 Required Software

* Visual Studio 2022 (Community, Professional, or Enterprise edition)
* .NET Core 3.1 SDK — download from [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
* Git (optional, for source control)

### 2.2 Creating the Project

In Visual Studio, create a new Class Library project targeting .NET Core 3.1:

1. Open Visual Studio 2022
2. Click Create a new project
3. Search for "Class Library" and select Class Library (.NET Core)
4. Name the project using the convention: CompanyName.Module.Feature — e.g. ERP.Ezra360.Package
5. Set the framework to .NET Core 3.1
6. Click Create

### 2.3 Required NuGet Packages

Add the following packages via NuGet Package Manager (Tools > NuGet Package Manager > Manage&#x20;&#x20;NuGet Packages for Solution):

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



## 3. Project Structure in Visual Studio

A well-organised Ezra360 plugin project follows a consistent folder structure. Each folder serves a&#x20;specific purpose and maps to a distinct category of code responsibility.

### 3.1 Recommended Folder Layout

YourProject/\
├── Models/ ← C# classes mapping to SQL query results\
│ ├── Directives.cs\
│ └── DirectiveSubmissions.cs\
├── Plugins/ ← Event-driven logic (IXrmPlugin)\
│ ├── CreateDirectiveSubmissions.cs\
│ └── SendConfirmationEmail.cs\




### 3.2 Folder Descriptions

#### Plugins/&#xD;

Contains all event-driven plugin classes. Each file implements IXrmPlugin. These are triggered&#x20;automatically by entity lifecycle events such as PostUpdate, PreCreate, PostDelete, etc.

#### Models/&#xD;

Contains plain C# classes (POCOs) that map to the results of your SQL queries. When you execute a&#x20;SQL query through context.BusinessActions.ExecuteListQuery(), the SDK maps each result row to&#x20;an instance of your model class. Column names in the SQL SELECT must match property names on&#x20;the model.

## 4. Creating a Plugin (IXrmPlugin)

A plugin class must be public and implement the IXrmPlugin interface from the Xrm.Soft.Aer.SDK&#x20;package. There is one required method: ExecutePlugin(PluginExecutionContext context).

{% stepper %}
{% step %}
### Add a New Class File

Right-click the Plugins folder in Solution Explorer, select Add > Class, and give it a descriptive name&#x20;that reflects what the plugin does. For example:\
• CalculatePackageTotals.cs\
• CreateDirectiveSubmissions.cs
{% endstep %}

{% step %}
### Implement IXrmPlugin

Your class must use the correct using directives and implement the required interface. Below is the&#x20;standard boilerplate structure:

```cs
using Xrm.Soft.Aer.SDK; 
using System; 
using System.Linq; 
using System.Collections.Generic; 
 
namespace ERP.Ezra360.Package.Plugins 
{ 
    public class CalculatePackageTotals : IXrmPlugin 
    { 
        public void ExecutePlugin(PluginExecutionContext context) 
        { 
            try 
            { 
                // 1. Get the triggering record 
                var target = (BusinessEntity)context.Target; 
                var packageLineId = target.Id.Value; 
 
                // 2. Run a SQL query to get related data 
                var lines = context.BusinessActions 
                    .ExecuteListQuery<PackageLine>(PackageSQL.GetPackageLines 
                        .Replace("@PackageLineId", $"'{packageLineId}'")) 
                    .ToList(); 
 
                // 3. Calculate total 
                decimal total = lines.Sum(l => l.Amount); 
 
                // 4. Update the parent record 
                var package = new BusinessEntity("Package") 
                { 
                    Id = lines.First().PackageId, 
                    Attributes = new List<EntityAttribute> 
                    { 
                        new EntityAttribute { SchemaName = "TotalAmount", Value = total } 
                    } 
                }; 
                context.BusinessActions.Update(package); 
            } 
            catch (Exception ex) 
            { 
                Log.Error($"CalculatePackageTotals Error: {ex}"); 
            } 
        } 
    } 
} 
```
{% endstep %}

{% step %}
### Key Context Methods

All platform operations are accessed through context.BusinessActions. Below are the most commonly&#x20;used methods:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Tip: Always wrap your plugin logic in a try/catch block. Unhandled exceptions in plugins will surface\
as generic platform errors and will not include your error message.
{% endhint %}
{% endstep %}
{% endstepper %}

### 4.1 Accessing the Triggering Record

The PluginExecutionContext exposes a Target property that represents the record that triggered the&#x20;event. Cast it to BusinessEntity to access its fields:

```c#
// Get the target record (the one that was created/updated/deleted) 
var target = (BusinessEntity)context.Target; 
 
// Access its Id 
Guid recordId = target.Id.Value; 
 
// Access an attribute value (if it was included in the trigger) 
var amount = target.Attributes 
.FirstOrDefault(a => a.SchemaName == "Amount")?.Value;
```

{% hint style="info" %}
Note: The Target entity only contains attributes that were changed in the triggering operation, not&#x20;the full record. To access fields that were not changed, use context.BusinessActions.RetrieveRecord()&#x20;to fetch the full record from the database.
{% endhint %}



## 5. Building the DLL in Visual Studio

Once your plugin code is complete, you need to compile the project to produce a&#x20;DLL file that will be uploaded to Ezra360.

### 5.1 Build the Solution

Go to the menu bar and select Build > Build Solution, or press Ctrl + Shift + B. Visual Studio will&#x20;compile all projects in the solution.

{% hint style="info" %}
The build must complete with zero errors. Warnings are acceptable. If there are&#x20;errors, the DLL file will not be generated and any existing DLL at the output path may&#x20;be stale or absent.
{% endhint %}

### 5.2 Locate the Output DLL

After a successful build, the compiled DLL is placed in:

_<mark style="color:$primary;">ProjectFolder\bin\Debug\netcoreapp3.1\YourProjectName.dll</mark>_

Navigate to this folder in Windows Explorer. The DLL filename matches your project name exactly. You&#x20;will also see a .pdb file (debug symbols) and a .deps.json file — only the .dll needs to be uploaded.

{% hint style="info" %}
✔ Tip: Always build in Debug mode when uploading to a sandbox or UAT environment. Use Release&#x20;mode only for production deployments. Debug builds include additional diagnostic information that&#x20;helps trace issues in Plugin Trace Logs.
{% endhint %}

### 5.3 Build Configuration Reference

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



## 6. Uploading the DLL to Ezra360

After building the DLL, it must be uploaded to Ezra360 as an Xrm Assembly record. The assembly&#x20;record holds the compiled code and serves as the container for all plugin classes and plugin message&#x20;registrations.

{% stepper %}
{% step %}
### Navigate to Xrm Assemblies&#x20;

Log into your Ezra360 environment. From the top navigation bar, open the Customizations menu. Scroll&#x20;down to Xrm Assemblies and click it.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Create or Update the Assembly

If this is the first time you are uploading this DLL, click New to create a new assembly record. If the&#x20;assembly already exists and you are updating it after a code change, click on the existing record.

The assembly record form contains:

* File — a file upload control to select your .dll file
* Assembly Name — auto-populated from the DLL metadata after upload  &#x20;
* Version — auto-populated with your build version number (e.g. 2026.5.28.1422)

{% hint style="info" %}
The version number in Ezra360 reflects your assembly version attribute in the .csproj&#x20;file. It typically follows the format **Year.Month.Day.BuildNumber.** You can confirm you&#x20;have the right version after upload.
{% endhint %}
{% endstep %}

{% step %}
### Upload the DLL File

Click the Choose File button in the assembly form, navigate to your bin/Debug/netcoreapp3.1/ folder,&#x20;and select the .dll file. The Assembly Name and Version fields will auto-populate once the file is&#x20;selected.

Click Save & Close to complete the upload. The assembly is now registered in Ezra360 and ready for&#x20;plugin message registration.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## 7. Registering Plugin Classes

After uploading your assembly, you must register each plugin class you want to use. Plugin classes are&#x20;listed under the XRM Plugin Classes tab on the assembly record. This step tells Ezra360 which C#&#x20;classes in your DLL are valid plugins.

### 7.1 Adding a New Plugin Class

On the assembly record, click the XRM Plugin Classes tab. Click Add New to open the Quick Create&#x20;dialog.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Fill in the following fields:

*  Name — must match the class name exactly as it appears in your C# code&#x20;
* Plugin Type — select the appropriate type for  &#x20;plugins
* Assembly — pre-filled with the current assembly record

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>



## 8. Registering Plugin Messages

A Plugin Message is the configuration record that tells Ezra360 when to fire a specific plugin class. It&#x20;binds a plugin class to an entity and an execution step.\
Plugin messages are registered under the Plugin Messages tab on the assembly record.

### 8.1 Viewing Existing Plugin Messages

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Each row in the Plugin Messages table represents one registration — one plugin bound to one&#x20;entity and one execution step.

### 8.2 Adding a New Plugin Message

Click Add New on the Plugin Messages tab to open the Quick Create dialog:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Fill in all required fields (marked with a red asterisk):

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>



## 9. Updating an Existing Assembly&#x20;

After your initial deployment, subsequent code changes require re-uploading the DLL. The process is&#x20;similar to the initial upload but with a few important differences.

### 8.1 Re-Upload Process&#x20;

1. Make your code changes in Visual Studio&#x20;
2. Rebuild the solution (Ctrl + Shift + B) and confirm zero errors&#x20;
3. Navigate to Customizations > Xrm Assemblies in Ezra360&#x20;
4. Click on the existing assembly record&#x20;
5. Click the upload/attachment icon next to the Name field (the paperclip icon)&#x20;
6. Select the updated .dll from your bin/Debug/netcoreapp3.1/ folder&#x20;
7. The Version field will update to reflect the new build version&#x20;
8. Click Save

{% hint style="info" %}
✔ Tip: You do not need to re-register plugin messages after re-uploading a DLL. Your existing plugin&#x20;message registrations continue to work as long as the class names remain the same. Only add new&#x20;registrations when you add new plugin classes.
{% endhint %}



## 10. Execution Steps Reference

Execution steps define the precise moment in the entity's lifecycle when a plugin fires. Choosing the&#x20;wrong execution step is a common source of bugs — for example, using PreUpdate when you need the&#x20;record to already be saved before reading it back.

### 10.1 Create Events

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

### 10.2 Update Events

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### 10.3 Retrieve Events

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### 10.4 Delete Events

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

