# JavaScript  Development Guide

## 1. Overview

The platform provides a rich client-side JavaScript framework that enables developers to extend, customize, and automate application behavior. JavaScript can be used throughout the system to implement business logic, improve user experience, integrate with external systems, and interact with platform data and services.

This documentation serves as the primary reference for all JavaScript development within the platform. It covers the available JavaScript libraries, APIs, object models, events, and development patterns used to build client-side customizations.

### 1.1 Where JavaScript Can Be Used

JavaScript can be used in several areas of the application, including:

#### Form Customizations

Form scripts allow developers to implement business logic that executes when users interact with records. Common scenarios include:

* Setting default values
* Validating user input
* Showing or hiding fields
* Enabling or disabling controls
* Executing logic during save operations
* Automating data population

#### Command and Action Buttons

Custom buttons and commands can execute JavaScript functions to perform actions such as:

* Creating records
* Running business processes
* Opening related records
* Calling external services
* Executing custom workflows

#### Lists and Grids

JavaScript can be used to interact with record lists and subgrids, including:

* Retrieving selected records
* Executing bulk actions
* Navigating between views
* Exporting data
* Refreshing grid content

#### Application-Level Functionality

Application-level scripts can be used for:

* Navigation
* Dashboard interactions
* Global notifications
* Session-related operations
* User context retrieval

#### Integrations

Developers can use JavaScript APIs to communicate with:

* Internal platform services
* REST APIs
* External applications
* Third-party systems

### 1.2 Development Principles

When developing JavaScript customizations, developers should follow these principles:

* Use documented and supported APIs only.
* Avoid direct manipulation of internal page elements.
* Prefer platform-provided APIs over browser-specific implementations.
* Write reusable and maintainable code.
* Use asynchronous operations whenever possible.
* Ensure compatibility across supported browsers and environments.

### 1.3 JavaScript Architecture

The platform exposes functionality through structured JavaScript namespaces and modules. These modules provide access to forms, records, lists, application services, and platform APIs.

Developers should use the documented APIs rather than relying on internal implementation details. This ensures compatibility with future platform updates and reduces maintenance overhead.

### 1.4 Purpose of This Documentation

This documentation provides:

* Detailed API references
* Method and property descriptions
* Parameter and return type information
* Usage examples
* Best practices
* Common implementation scenarios
* Troubleshooting guidance

Whether you are creating simple form validations or building complex integrations, this documentation is intended to be the authoritative source for client-side development within the platform.

## 2. Uploading and Registering JavaScript in Ezra360

This section explains how to create, upload, and configure JavaScript web resources within Ezra360. You will learn how to add a JavaScript file to the system, publish it as a web resource, and associate it with forms and events.

By the end of this guide, you will be able to:

* Create and upload JavaScript web resources.
* Register JavaScript libraries within Ezra360.
* Link JavaScript functions to form events.
* Execute custom business logic when forms load, save, or when field values change.
* Test and troubleshoot JavaScript customizations.

Understanding how to properly register and associate JavaScript with forms is essential for implementing client-side customizations and extending the functionality of Ezra360.

To upload or update a JavaScript web resource in Ezra360, navigate to **Modules** and open the **Customisation** module. Within the Customisation module, select **Custom Resources**, where all web resources, including JavaScript, HTML, and CSS files, are managed.

If you are updating an existing JavaScript file, use the search functionality to locate the resource by name and open it. Once opened, you can upload the latest version of the file and save your changes.

If you are creating a new JavaScript web resource, click the **New** button, provide the required details, upload the JavaScript file, and save the record.

After saving, ensure that the resource is published and available before associating it with forms or other components within the system.





## 3. ElasticXrm Documentation

### Introduction

#### What is ElasticXrm?

ElasticXrm is a client-side JavaScript SDK used to interact with the Elastic CRM platform. It provides developers with a consistent API for working with:

* Forms
* Records
* Lists
* Subgrids
* Application navigation
* CRUD operations
* Quick Create forms
* Dashboards
* Custom requests

The library is exposed globally through the `ElasticXrm` object.

ElasticXrm\
│\
├── List\
├── Form\
├── Api\
├── App\
└── Internal

| Module   | Purpose               |
| -------- | --------------------- |
| List     | List view operations  |
| Form     | Form manipulation     |
| Api      | CRUD operations       |
| App      | Application utilities |
| Internal | Framework utilities   |

## ElasticXrm.List

Provides functionality for working with entity lists.

***

### getSelectedRecords()

Returns all selected records from the current list.

```js
 ElasticXrm.List.getSelectedRecords();
```

## **Returns Your ARRAY In this Form**

```javascript
Array<{
    SchemaName: string;
    Id: string;
}>
```

EXAMPLE

```javascript
var records = ElasticXrm.List.getSelectedRecords();
console.log(records);
//Results
[
    {
        SchemaName: "ContactId",
        Id: "8f32fsj737t733uugwfjhg"
    }
]
```

### getAllRecords()

Returns all records currently displayed in the list.

```javascript
ElasticXrm.List.getAllRecords();

//Returns

Array<{
    SchemaName: string;
    Id: string;
    IsSelected: boolean;
}>
//Example
var records = ElasticXrm.List.getAllRecords();
```

### gotoView(viewName)

Changes the currently displayed list view to the specified system view and reloads the list data associated with that view.

This method is typically used when custom buttons, scripts, or automation processes need to direct users to a specific view without requiring manual navigation through the user interface.

For example, a developer may use this method after completing an operation to automatically redirect users from "All Records" to "My Active Records".

#### Syntax

```javascript
ElasticXrm.List.gotoView(viewName);
```

#### Syntax

```javascript
ElasticXrm.List.gotoView(viewName);

//Example

ElasticXrm.List.gotoView("Active Contacts");
```

Parameters

| Parameter | Type   | Description             |
| --------- | ------ | ----------------------- |
| viewName  | string | Name of the target view |

### currentView()

Returns the active view name.

```javascript
ElasticXrm.List.currentView();

//Returns
a string
//Example
var currentView = ElasticXrm.List.currentView();


```

## ElasticXrm.Form

Provides access to the current form and its fields.

***

### getId()

Returns the current record ID.

#### Syntax

```javascript
ElasticXrm.Form.getId();

//Returns
an "Id" of a record in a string format.

//Example
var id = ElasticXrm.Form.getId();

```

### getFormMode()

Returns the current form mode.

#### Syntax

```javascript
ElasticXrm.Form.getFormMode();
```

#### Returns

| Value    |
| -------- |
| Create   |
| Update   |
| ReadOnly |

```javascript
if (ElasticXrm.Form.getFormMode() === "Create") {
    console.log("New Record");
}
else if(ElasticXrm.Form.getFormMode() === "Update"){
     console.log("New Record");
}
```

### getReadOnly()

Checks whether the form is read-only.

#### Syntax

```javascript
ElasticXrm.Form.getReadOnly();

//Returns
a boolean value
```

### getEntitySchemaName()

Returns the current entity schema name.

#### Syntax

```js
ElasticXrm.Form.getEntitySchemaName();

//Example
var entity = ElasticXrm.Form.getEntitySchemaName();
if(entity==="Employee"){
    //Put your logic here
}
```

### getEntityDisplayName()

Returns the display name of the current entity.

#### Syntax

```javascript
ElasticXrm.Form.getEntityDisplayName();
```

### saveRecord()

Saves the current record.

#### Syntax

ElasticXrm.Form.saveRecord();

```javascript
ElasticXrm.Form.saveRecord();
```

### closeRecord()

Closes the current form and returns to the list.

```javascript
ElasticXrm.Form.closeRecord();
```

### newRecord()

Opens a new record form.

#### Syntax

```javascript
ElasticXrm.Form.newRecord();
```

### deleteRecord()

Displays a confirmation and deletes the record.

```javascript
ElasticXrm.Form.deleteRecord();
```

### recordsStateChange()

Activates or deactivates records.

```javascript
ElasticXrm.Form.recordsStateChange(active);
```

Parameters

| Parameter | Type    |
| --------- | ------- |
| active    | boolean |

```javascript
ElasticXrm.Form.recordsStateChange(false);
```

## Working With Form Fields

### getFormField()

Returns a field object.

Syntax

```javascript
ElasticXrm.Form.getFormField(fieldName);
//Example
var field = ElasticXrm.Form.getFormField("FirstName");
```

Field Object

```javascript
ElasticXrm.Form.getFormField();
//getting a value of a formfield
ElasticXrm.Form.getFormfield("Amount").getValue();
```

getValue();

Returns the value of the field you are attrying to access.

```javascript
//Example
var amount = ElasticXrm.Form.getFormfield("Amount").getValue();
if(amount>100){
    console.log("Expensive");
}
if(amount<100){
    console.log("This is Cheaper");
}
```

### setValue()

Sets field value.

```javascript
ElasticXrm.Form.getFormfield("Name").setValue("John");
```

### setLabelText()

Changes field label.

```javascript
ElasticXrm.Form.setLabelText("Customer Name");
```

### setDisabled()

Enables or disables field.

```javascript
ElasticXrm.Form.getFormfield("Name").setDisabled(true);
```

### setVisible()

Shows or hides field.

```javascript
ElasticXrm.Form.getFormfield("Name").setVisible(false);
```

### setError()

Displays validation message.

```javascript
ElasticXrm.Form.getFormfield("Name").setError(
    "This field is required"
);
```

### clearError()

Removes validation message.

```javascript
ElasticXrm.Form.getFormfield("Name").clearError();
```

### editAsText()

Converts the field into a text area.

```javascript
ElasticXrm.Form.getFormfield("Name").editAsText();
```

## Lookup Fields

### Get Lookup Value

```javascript
var account = ElasticXrm.Form.getFormField("AccountId").getValue(true);

//Output

{
    lookupText: "Contoso",
    lookupValue: "0hjkhcfgddhjmvjkcgh",
    lookupEntity: "Account"
}
```

&#x20;&#x20;

### Set Lookup Value

```javascript
ElasticXrm.Form.getFormField("AccountId").setValue({
              lookupText: "Contoso",
              lookupValue: recordId,
              lookupEntity: "Account"
          });
```

## Buttons

### getButton()

Returns a button object.

#### Syntax

```javascript
ElasticXrm.Form.getButton("buttonId");

//Example
var btn = ElasticXrm.Form.getButton("btnActionSave");
```

setVisible()

```javascript
ElasticXrm.Form.getButton("buttonId").setVisible(false);
```

setDisabled()

```javascript
ElasticXrm.Form.getButton("buttonId").setDisabled(true);
```

## SubGrids

### getSubGrid()

Returns a subgrid object.

#### Syntax

```javascript
var grid = ElasticXrm.Form.getSubGrid("ContactsGrid");
```

getSelectedRecords()

```javascript
ElasticXrm.Form.getSubGrid("ContactsGrid").getSelectedRecords();
```

### deleteSelected()

Deletes selected rows.

```javascript
ElasticXrm.Form.getSubGrid("ContactsGrid").deleteSelected();
```

## ElasticXrm.Api

Provides CRUD\[Create, Read, Update and Delete] operations.

ElasticXrm.Api.createRecord(entityName,fields,callback);

### createRecord()

Creates a record.

```javascript
ElasticXrm.Api.createRecord(entityName, fields, callback);
```

Example

```javascript
ElasticXrm.Api.createRecord("Contact", fields, function(result){
        console.log(result);
    }
);
```

### retrieveRecord()

Retrieves a record.

```javascript
ElasticXrm.Api.retrieveRecord(
    entityName,
    recordId,
    callback
);
```

Example

```javascript
var recordId = ElasticXrm.Form.getFormField("ContactId", true).getValue();
ElasticXrm.Api.retrieveRecord("Contact", recordId, function(results){
        console.log(results);
    }
);
```

### updateRecord()

Updates a record.

```javascript
ElasticXrm.Api.updateRecord(entityName, recordId, fields, callback);
```

### deleteRecord()

Deletes a record.

```javascript
ElasticXrm.Api.deleteRecord(entityName, recordId);
```

### retrieveMultiple()

Retrieves multiple records.

```java
ElasticXrm.Api.retrieveMultiple(query, callback);
```

### executeRequest()

Executes a custom server-side request.

```javascript
ElasticXrm.Api.executeRequest(requestName, entityName, recordId, callback);
```

### currentUser()

Returns the current user.

```javascript
ElasticXrm.Api.currentUser(function(user){
    }
);
```

### defaultCurrency()

Returns system default currency.

```javascript
ElasticXrm.Api.defaultCurrency(null, function(currency){
    }
);
```

### exportList()

Exports list data.

```javascript
ElasticXrm.Api.exportList("Contact", "Active Contacts", true, "xlsx");
```

Supported formats:

* xlsx
* csv (if configured)

## ElasticXrm.App

Application utilities.

***

### getActiveModule()

Returns current module ID.

```javascript
ElasticXrm.App.getActiveModule();
```

### getQueryString()

Gets query string value.

```javascript
ElasticXrm.App.getQueryString("Id");
```

### loadDashboard()

Loads a dashboard.

```javascript
ElasticXrm.App.loadDashboard(dashboardId);
```

## ElasticXrm.Internal

⚠️ Internal APIs. Use only when necessary.

***

### start\_loading()

Shows loading indicator.

```javascript
ElasticXrm.Internal.start_loading();
```

### end\_loading()

Hides loading indicator.

```javascript
ElasticXrm.Internal.end_loading();
```

### quickForm()

Opens a Quick Create form.

```javascript
ElasticXrm.Internal.quickForm("Contact");
```

## Common Examples

### Get Current Record

```javascript
var recordId = ElasticXrm.Form.getId();
```

### Get Current Entity

```javascript
var entity = ElasticXrm.Form.getEntitySchemaName();
```

### Read Field

```javascript
var email = ElasticXrm.Form.getFormField("Email").getValue();
```

### Update Field

```javascript
ElasticXrm.Form.getFormField("Email").setValue("admin@company.com");
```

### Disable Field

```javascript
ElasticXrm.Form.getFormField("Email").setDisabled(true);
```

### Create a Record

```javascript
ElasticXrm.Api.createRecord("Contact", fields,
    function(result){
        console.log(result);
    }
);
```

