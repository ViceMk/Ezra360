# Entities

A<mark style="color:blue;">**dd New Entity**</mark>\
Configuring **Entities** on Admin Module, you can click on <mark style="color:orange;">**#1**</mark> dropdown select to view list of modules, or you can click on the  <mark style="color:orange;">**#2**</mark> directly on the entity to open the entities with its properties in a table.\
Each Entity has a **display name** shown in <mark style="color:orange;">**#3**</mark> and has its own unique **schema name**, represented on <mark style="color:orange;">**#4**</mark> inside the angle brackets "**<>**".

{% hint style="info" %}
A **schema name** refers to a unique identifier that is assigned to an entity within Ezra. It is used as a reference consistently across different parts of the platform, including customization, configuration, plugins and integration. The schema name is specified upon creation. It must be unique. This name should be in Pascal case. The schema name is used to create the class for the entity.
{% endhint %}

To add a new entity on the platform, you will click the <mark style="color:orange;">**#5**</mark> button. The display will appear as follow on the below representation.\
To view entity properties, click the dropdown to collapse the entity for more information.

<figure><img src="../../../.gitbook/assets/Untitled design 19.png" alt=""><figcaption><p><mark style="color:red;">Click image to view full screen</mark></p></figcaption></figure>

<mark style="color:blue;">**Filling the Entity form:**</mark>\
When the add button is clicked it will display the form below on <mark style="color:orange;">**#2**</mark> to add a new entity.\
<mark style="color:orange;">**#1**</mark> Shows properties of an entity.

* The **Listing** dropdown refers to a view or display of records within a specific entity. It's a way to present and access information in a structured format, making it easier for users to interact with and analyze data. The listing dropdown has _views, buttons and linked resources_. <mark style="color:blue;">For more information see</mark> [listing](listing/ "mention")
* The **Attributes** dropdown describes the elementary feature of an entity. The attributes also has  Display name, Schema name, and Data type. You can configure each attribute to _Visible_, _Required, Read-Only, Customizable,_ and _Enable Audit_ by checking the checkboxes. <mark style="color:blue;">For more information see</mark> [attributes.md](attributes.md "mention")
* The **Forms** dropdown has _views, buttons, and linked resources_. If you want any JavaScript to run on your form, you can link it in the Linked Resources under forms. <mark style="color:blue;">For more information regarding resources please see</mark> [forms](forms/ "mention")

To **save** the new Entity click <mark style="color:orange;">**#3**</mark> once (<mark style="color:red;">Do not click twice as it will duplicate the entity</mark>).&#x20;

{% hint style="danger" %}
Duplication detection functionality is pending.
{% endhint %}

Alternatively if you want to remove entity you can click the **Delete** button on <mark style="color:orange;">**#4,**</mark> this include to existing entities as well. &#x20;

<figure><img src="../../../.gitbook/assets/Untitled design 20.png" alt=""><figcaption><p><mark style="color:red;">Click image to view full screen</mark></p></figcaption></figure>

### Form Fields

<mark style="color:orange;">**#2**</mark> **Filling Form Fields**

<table><thead><tr><th width="277">Field Record</th><th>Description</th></tr></thead><tbody><tr><td>Display name</td><td>it represents the display name of your field. The name that is displayed to the user on the interface. For example, "<mark style="color:blue;">Sales Order</mark>".</td></tr><tr><td>Schema Name</td><td>It must be unique. It is used to create the logical name. This name should be in Pascal case. The schema name is used to create the class for the entity. For example, "<mark style="color:blue;">SalesOrder</mark>". </td></tr><tr><td>Plural name</td><td>refers to the name used to represent multiple instances of a data record entity. For example, "<mark style="color:blue;">Sales Orders</mark>"</td></tr><tr><td>Entity Type</td><td><strong>Data Record:</strong> refers to the structured data components that represent various business objects or concepts within the system. Each entity is associated with a set of fields that capture relevant information.<br><strong>Document Library:</strong> refers to a centralized repository where you can store and manage various types of documents, files, and attachments associated with records within the Ezra environment.<br></td></tr><tr><td>Owner Type</td><td>Defines the scope and nature of ownership for records within a specific entity.<br><strong>User</strong>: refers to individual users within the system that can be assigned as the owners of records that will have control over those records and can perform actions.<br><strong>Everyone</strong>: It signifies that records are owned by the system itself or are considered to be shared across the organization.</td></tr><tr><td>Business Process</td><td></td></tr><tr><td>Social Timeline</td><td></td></tr><tr><td>Enable Audit</td><td></td></tr><tr><td>Duplicate Detection</td><td></td></tr><tr><td>Attachments</td><td></td></tr><tr><td>Customizable</td><td>it's set to default, which is false.</td></tr><tr><td>Searchable</td><td>It refers that the field value is searchable by the user in a case of categorized search or relevance search.</td></tr><tr><td>Publish Create Event</td><td>Boolean</td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>



***



### Data Types

The fields define the individual data items that can be used to store data in an entity. Each field is represented by a data type. The following table contains information about the field data types.

<table><thead><tr><th width="173.33333333333331">Data Type</th><th width="547">Description</th></tr></thead><tbody><tr><td>Single Text</td><td>A field that allows entering a single line of text. It's commonly used for short pieces of information. This field can contain up to 4,000 text characters. You can set the maximum length to be less than this.</td></tr><tr><td>Multi-Line Text</td><td>This is a scrolling text box. The field that allows entering multiple lines of text. It's useful for capturing longer descriptions or comments. When you add this field to a form, you can specify the size of the field.</td></tr><tr><td>Whole Number</td><td>field that stores whole numbers (integers). It's suitable for counting items or quantities. You can set the minimum and maximum values too.</td></tr><tr><td>Decimal</td><td>A field that stores decimal numbers (floating-point numbers). It's used for storing fractional or precise numeric values.</td></tr><tr><td>Currency</td><td>A field that stores currency values with currency symbols. It's used for storing monetary amounts. </td></tr><tr><td>Lookup</td><td>A field that creates a relationship with another entity and displays a searchable list of records from that entity.</td></tr><tr><td>Generic Lookup</td><td>Similar to a lookup, but more flexible as it can reference multiple entity types. It's used for creating relationships with different types of records.</td></tr><tr><td>Partylist</td><td>A special type of field used to reference multiple party records (like Contacts, Accounts) involved in an activity. It's often used for scheduling and tracking interactions.</td></tr><tr><td>Select Option</td><td>A field that provides a list of predefined options for selection. Users choose from the provided values.</td></tr><tr><td>DateTime</td><td>A field that stores both date and time information. It's used to capture specific points in time.</td></tr><tr><td>Date</td><td>A field that stores only date information. It's used for capturing dates without time details.</td></tr><tr><td>Boolean</td><td>A field that stores true/false values. It's used for capturing binary choices. Example: Is Active - True, Accept Terms - False.</td></tr><tr><td>Rich Text</td><td>It allows formatting text with styles, links, and images. It's used for creating well-structured content.</td></tr><tr><td>Web Resource</td><td>A field that can display web content within the form. It's used for embedding external resources.</td></tr><tr><td>Hyperlink</td><td>A field that stores a URL and displays as a clickable link. It's used for linking to external resources.</td></tr></tbody></table>

***

### Adding New View

**Views** are added under Entity, Navigate to **Entity**>**Listing**> **Views** or collapse it to view existing **Views,** Click **Add New** button to open the form for adding new view.  The table below shows descriptive details about the form fields.

<figure><img src="../../../.gitbook/assets/Untitled design 28 (1).png" alt=""><figcaption><p><mark style="color:red;">click Image to view full screen</mark></p></figcaption></figure>

| Field Record          | Description |
| --------------------- | ----------- |
| View Type             |             |
| Unique Name           |             |
| Entity Sub-Type       |             |
| Query                 |             |
| Override Columns Text |             |
| Page Limit            |             |
| Sort Index            |             |
| Is Default            |             |

***

### Adding New Form

When adding a new form you navigate to the forms <mark style="color:orange;">**#1**</mark>, when you click the dropdown of the forms you see available form types, you can click the <mark style="color:blue;">Add New</mark> button to add a new form. The <mark style="color:orange;">**#2**</mark> form field will be displayed.

<figure><img src="../../../.gitbook/assets/Untitled design 29.png" alt=""><figcaption><p><mark style="color:red;">Click Image to view full screen</mark></p></figcaption></figure>

The Table below shows different types of forms, You select one of the following forms. <mark style="color:blue;">For more information regarding forms see,</mark> [forms](../../../overview/components-overview/forms/ "mention")

* Main Form
* Mobile Form
* Quick Create Form
* Quick Peak Form
* Portal Form
