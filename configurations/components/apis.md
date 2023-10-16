# API's

An API that can be used with soft-aer.com's Elastic-XRM.\
In OpenAPI 3.0, parameters are defined in the `parameters` section of an operation or path. To describe a parameter, you specify its `name`, location (`in`), data type (defined by either `schema` or `content`) and other attributes, such as `description` or `required`. Here is an example:\


For more information please see #

```csharp
paths:
  /users/{userId}:
    get:
      summary: Get a user by ID
      parameters:
        - in: path
          name: userId
          schema:
            type: integer
          required: true
          description: Numeric ID of the user to get// 
```

### Entity

### GET configuration <a href="#get-configuration" id="get-configuration"></a>

```json

  "entityTypeId": 0,
  "schemaName": "string",
  "displayName": "string",
  "displayPluralName": "string",
  "attributes": [
    {
      "format": "string",
      "formPositionX": 0,
      "formPositionY": 0,
      "objectMetadataId": "",
      "parentObjectId": "",
      "dataType": "string",
      "isAudit": true,
      "isPicklist": true,
      "picklistUniqueName": "string",
      "isLookup": true,
      "lookUpEntity": "string",
      "isMultiline": true,
      "noLines": 0,
      "maxLength": 0,
      "control": "string",
      "isId": true,
      "schemaName": "string",
      "displayName": "string",
      "pickList": {
        "pickListId": "",
        "uniqueName": "string",
        "displayName": "string",
        "parentPicklistId": "",
        "options": [
          {
            "pickListId": "",
            "sortIndex": 0,
            "optionValue": 0,
            "optionText": "string",
            "parentOptionValue": 0,
            "picklistItemId": ""
          }
        ]
      },
```



### PUT configuration <a href="#put-configuration" id="put-configuration"></a>

```json

  "entityTypeId": 0,
  "schemaName": "string",
  "displayName": "string",
  "displayPluralName": "string",
  "attributes": [
    {
      "format": "string",
      "formPositionX": 0,
      "formPositionY": 0,
      "objectMetadataId": "",
      "parentObjectId": "",
      "dataType": "string",
      "isAudit": true,
      "isPicklist": true,
      "picklistUniqueName": "string",
      "isLookup": true,
      "lookUpEntity": "string",
      "isMultiline": true,
      "noLines": 0,
      "maxLength": 0,
      "control": "string",
      "isId": true,
      "schemaName": "string",
      "displayName": "string",
      "pickList": {
        "pickListId": "",
        "uniqueName": "string",
        "displayName": "string",
        "parentPicklistId": "",
        "options": [
          {
            "pickListId": "",
            "sortIndex": 0,
            "optionValue": 0,
            "optionText": "string",
            "parentOptionValue": 0,
            "picklistItemId": ""
          }
        ]
      },
```

### Testing Parameters

\
**entityName** <mark style="color:red;">\*</mark> string:\
string\
**version** <mark style="color:red;">\*</mark> :\
string\
**api-version string :**
