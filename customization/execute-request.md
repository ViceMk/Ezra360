# Execute Request

## 1. Overview: Execute Requests

### 1.1 What is an Execute Request?

An Execute Request is a class that implements the **IExecuteRequest** interface. Unlike plugins, execute&#x20;requests are not triggered automatically — they are invoked on demand by a button click, workflow,&#x20;scheduled job, or another plugin.\
Execute requests must return an **ExecuteResponse** object that communicates success or failure back&#x20;to the caller.

#### Common use cases for execute requests:

* Bulk processing operations across many records
* Sending reminder emails on a scheduled basis  &#x20;
* Generating or exporting reports
* External system integrations triggered by a user action
* Complex multi-step operations that are too heavy for synchronous plugin execution

#### Execute Request (IExecuteRequest) Characteristics

* [ ] Triggered on demand by user or process
* [ ] Implements IExecuteRequest
* [ ] Return type: ExecuteResponse
* [ ] Execution step: ExecuteRequest
* [ ] Access: PluginExecutionContext
* [ ] Example: Send bulk reminder emails

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
├── Execute Requests/ ← On-demand logic (IExecuteRequest)\
│ └── SendEmailReminderReq.cs\
├── Models/ ← C# classes mapping to SQL query results\
│ ├── Directives.cs\
│ └── DirectiveSubmissions.cs\




### 3.2 Folder Descriptions

#### Execute Requests/&#xD;

Contains all on-demand logic classes. Each file in this folder implements IExecuteRequest. These are&#x20;invoked by buttons, workflows, or scheduled processes — never fired automatically by the platform.

#### Models/&#xD;

Contains plain C# classes (POCOs) that map to the results of your SQL queries. When you execute a&#x20;SQL query through context.BusinessActions.ExecuteListQuery(), the SDK maps each result row to&#x20;an instance of your model class. Column names in the SQL SELECT must match property names on&#x20;the model.

## 4. Creating an Execute Request (IExecuteRequest)

Execute requests give you an on-demand alternative to event-driven plugins. They expose an entry&#x20;point that can be called from a button, workflow, or scheduled job, and they return a structured result.

### 4.1 Interface Structure

A class implementing IExecuteRequest must declare the following properties and the ExecutePlugin&#x20;method:

```csharp
using System;
using System.Collections.Generic;
using Crm.SoftAer.Com.Model;
using Ezra360.MSCOA.Statement.PDFGenerators;
using Xrm.Soft_Aer.SDK;

namespace Ezra360.MSCOA.Statement.ExecuteRequest
{
    public class PreviewStatementExecuteRequest : IExecuteRequest
    {
        public string EntityName { get; set; }
        public Guid RecordId { get; set; }
        public BusinessEntity Record { get; set; }
        public Dictionary<string, object> InputParameters { get; set; }

        public ExecuteResponse ExecutePlugin(PluginExecutionContext context)
        {
            try
            {
                context.Trace("PreviewStatementExecuteRequest: Execution started.");

                // Validate context
                if (context == null)
                {
                    context.Trace("PreviewStatementExecuteRequest: Plugin context is null.");
                    return CreateErrorResponse("Plugin context is null.");
                }

                context.Trace($"PreviewStatementExecuteRequest: Resolving Statement record. RecordId: {RecordId}, Record: {(Record != null ? Record.Id?.ToString() : "null")}");

                var statement = ResolveStatement(context);
                if (statement == null)
                {
                    context.Trace("PreviewStatementExecuteRequest: Unable to resolve Statement record for preview.");
                    return CreateErrorResponse("Unable to resolve Statement record for preview.");
                }

                context.Trace($"PreviewStatementExecuteRequest: Statement resolved successfully. StatementId: {statement.Id}");

                var statementPdfGenerator = new StatementPdfGenerator();
                context.Trace("PreviewStatementExecuteRequest: Generating PDF...");

                var docArray = statementPdfGenerator.Generate(context, statement, "Statement");

                if (docArray == null || docArray.Length == 0)
                {
                    context.Trace("PreviewStatementExecuteRequest: Failed to generate Statement PDF - returned null or empty array.");
                    return CreateErrorResponse("Failed to generate Statement PDF.");
                }

                context.Trace($"PreviewStatementExecuteRequest: PDF generated successfully. Size: {docArray.Length} bytes.");
                
                var base64Result = Convert.ToBase64String(docArray);
                context.Trace($"PreviewStatementExecuteRequest: PDF converted to Base64. Length: {base64Result.Length} characters.");

                return new ExecuteResponse
                {
                    IsSuccess = true,
                    Results = base64Result
                };
            }
            catch (Exception ex)
            {
                context.Trace($"PreviewStatementExecuteRequest ERROR: {ex.Message}");
                context.Trace($"PreviewStatementExecuteRequest Stack Trace: {ex.StackTrace}");
                
                // Log inner exception if exists
                if (ex.InnerException != null)
                {
                    context.Trace($"PreviewStatementExecuteRequest Inner Exception: {ex.InnerException.Message}");
                    context.Trace($"PreviewStatementExecuteRequest Inner Stack Trace: {ex.InnerException.StackTrace}");
                }

                return CreateErrorResponse($"PreviewStatement Error: {ex.Message}");
            }
            finally
            {
                context.Trace("PreviewStatementExecuteRequest: Execution completed.");
            }
        }

        private BusinessEntity ResolveStatement(PluginExecutionContext context)
        {
            context.Trace("ResolveStatement: Starting resolution process.");

            // Check Record property first (highest priority)
            if (Record != null && Record.Id.HasValue && Record.Id.Value != Guid.Empty)
            {
                context.Trace($"ResolveStatement: Using Record property with Id: {Record.Id.Value}");
                return context.BusinessActions.RetrieveRecord("Statement", Record.Id.Value);
            }

            // Check RecordId property
            if (RecordId != Guid.Empty)
            {
                context.Trace($"ResolveStatement: Using RecordId property: {RecordId}");
                return context.BusinessActions.RetrieveRecord("Statement", RecordId);
            }

            // Check InputParameters for StatementId
            if (InputParameters != null && InputParameters.TryGetValue("StatementId", out var statementIdObj))
            {
                context.Trace($"ResolveStatement: Found StatementId in InputParameters: {statementIdObj}");
                if (Guid.TryParse(statementIdObj?.ToString(), out var statementId) && statementId != Guid.Empty)
                {
                    context.Trace($"ResolveStatement: Successfully parsed StatementId: {statementId}");
                    return context.BusinessActions.RetrieveRecord("Statement", statementId);
                }
                else
                {
                    context.Trace($"ResolveStatement: Failed to parse StatementId from InputParameters: {statementIdObj}");
                }
            }

            // Check context target
            if (context.Target is BusinessEntity target && target.Id.HasValue && target.Id.Value != Guid.Empty)
            {
                context.Trace($"ResolveStatement: Using context.Target with Id: {target.Id.Value}");
                return context.BusinessActions.RetrieveRecord("Statement", target.Id.Value);
            }

            context.Trace("ResolveStatement: No valid Statement identifier found.");
            return null;
        }

        private ExecuteResponse CreateErrorResponse(string message)
        {
            return new ExecuteResponse
            {
                IsSuccess = false,
                ClientMessage = message
            };
        }
    }
}
```

### 4.2 The ExecuteResponse Object

The ExecuteResponse tells the caller whether the operation succeeded and provides a message and&#x20;optional data back:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



## 5. Building the DLL in Visual Studio

Once your execute request code is complete, you need to compile the project to produce a&#x20;DLL file that will be uploaded to Ezra360.

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
Tip: Always build in Debug mode when uploading to a sandbox or UAT environment. Use Release&#x20;mode only for production deployments. Debug builds include additional diagnostic information that&#x20;helps trace issues in Plugin Trace Logs.
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
* Plugin Type — select the appropriate type for  &#x20;Execute Requests
* Assembly — pre-filled with the current assembly record

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>



## 8. Registering Plugin Messages

A Plugin Message is the configuration record that tells Ezra360 when to fire a specific plugin class. It&#x20;binds a plugin class to an entity and an execution step.\
Plugin messages are registered under the Plugin Messages tab on the assembly record.

### 8.1 Viewing Existing Plugin Messages

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Each row in the Plugin Messages table represents one registration — one execute request bound to one&#x20;entity and one execution step.

### 8.2 Adding a New Plugin Message

Click Add New on the Plugin Messages tab to open the Quick Create dialog:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Fill in all required fields (marked with a red asterisk):

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>



## 9. Updating an Existing Assembly&#x20;

After your initial deployment, subsequent code changes require re-uploading the DLL. The process is&#x20;similar to the initial upload but with a few important differences.

### 9.1 Re-Upload Process&#x20;

1. Make your code changes in Visual Studio&#x20;
2. Rebuild the solution (Ctrl + Shift + B) and confirm zero errors&#x20;
3. Navigate to Customizations > Xrm Assemblies in Ezra360&#x20;
4. Click on the existing assembly record&#x20;
5. Click the upload/attachment icon next to the Name field (the paperclip icon)&#x20;
6. Select the updated .dll from your bin/Debug/netcoreapp3.1/ folder&#x20;
7. The Version field will update to reflect the new build version&#x20;
8. Click Save

{% hint style="info" %}
Tip: You do not need to re-register plugin messages after re-uploading a DLL. Your existing plugin&#x20;message registrations continue to work as long as the class names remain the same. Only add new&#x20;registrations when you add new plugin classes.
{% endhint %}

