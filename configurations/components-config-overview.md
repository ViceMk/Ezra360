# Components Config Overview

### Overview&#x20;

A **Component** is like a basic building block that you can attach to an application, or web. Once you attach a component, you can give it specific details to match your requirements. These details might be about what users need, its features, or rules for using it. A component can even have smaller parts within it. You can use a component multiple times in your space.

The following Components are all configured under **Settings Entity**. The settings entity will redirect you to Admin module where configuration is done.

{% hint style="warning" %}
Please Note:

Your access to this module within the Ezra360 platform depends on your access settings. For more information, see [security-roles-and-privileges.md](../security-and-roles/security-roles-and-privileges.md "mention") and [user-roles.md](../security-and-roles/user-roles.md "mention")
{% endhint %}

To navigate it, go to **Modules** > **Settings** > **Customization**\
<mark style="color:orange;">**#1**</mark> navigate to list of **Modules**, and click <mark style="color:orange;">**#2**</mark> **settings** Entity, and Click <mark style="color:orange;">**#3**</mark> **Customization**.

<figure><img src="../.gitbook/assets/Untitled design 18.png" alt=""><figcaption><p><mark style="color:red;">Click image to view full screen</mark></p></figcaption></figure>

The **Customization** direct link will take you to an Admin Module, that has a layout like this:

<figure><img src="../.gitbook/assets/Screenshot 2023-08-11 154242.png" alt=""><figcaption><p><mark style="color:red;">Click image to view full screen</mark></p></figcaption></figure>

From the Admin Module Screen we will navigate to see the listed Entities. <mark style="color:blue;">See the</mark> [entities](components/entities/ "mention")<mark style="color:blue;">configuration.</mark>



***

## Data Types

The following table of data types acts as guide when configuring Ezra for your form fields.

<table><thead><tr><th width="173.33333333333331">Data Type</th><th width="547">Description</th></tr></thead><tbody><tr><td>Single Text</td><td>A field that allows entering a single line of text. It's commonly used for short pieces of information. This field can contain up to 4,000 text characters. You can set the maximum length to be less than this.</td></tr><tr><td>Multi-Line Text</td><td>This is a scrolling text box. The field that allows entering multiple lines of text. It's useful for capturing longer descriptions or comments. When you add this field to a form, you can specify the size of the field.</td></tr><tr><td>Whole Number</td><td>field that stores whole numbers (integers). It's suitable for counting items or quantities. You can set the minimum and maximum values too.</td></tr><tr><td>Decimal</td><td>A field that stores decimal numbers (floating-point numbers). It's used for storing fractional or precise numeric values.</td></tr><tr><td>Currency</td><td>A field that stores currency values with currency symbols. It's used for storing monetary amounts. </td></tr><tr><td>Lookup</td><td>A field that creates a relationship with another entity and displays a searchable list of records from that entity.</td></tr><tr><td>Generic Lookup</td><td>Similar to a lookup, but more flexible as it can reference multiple entity types. It's used for creating relationships with different types of records.</td></tr><tr><td>Partylist</td><td>A special type of field used to reference multiple party records (like Contacts, Accounts) involved in an activity. It's often used for scheduling and tracking interactions.</td></tr><tr><td>Select Option</td><td>A field that provides a list of predefined options for selection. Users choose from the provided values.</td></tr><tr><td>DateTime</td><td>A field that stores both date and time information. It's used to capture specific points in time.</td></tr><tr><td>Date</td><td>A field that stores only date information. It's used for capturing dates without time details.</td></tr><tr><td>Boolean</td><td>A field that stores true/false values. It's used for capturing binary choices. Example: Is Active - True, Accept Terms - False.</td></tr><tr><td>Rich Text</td><td>It allows formatting text with styles, links, and images. It's used for creating well-structured content.</td></tr><tr><td>Web Resource</td><td>A field that can display web content within the form. It's used for embedding external resources.</td></tr><tr><td>Hyperlink</td><td>A field that stores a URL and displays as a clickable link. It's used for linking to external resources.</td></tr></tbody></table>

\


## Common Form Records Fields

Below is the common records fields, when configuring Ezra, you will most likely to come across this fields, here is an overview of each records. Refer to this Table if you don't understand some text fields.

{% hint style="info" %}
Please note that Record fields may differ according to the component and the Business Process.
{% endhint %}

<table><thead><tr><th width="232">Field Record</th><th>Description</th></tr></thead><tbody><tr><td>Display Name</td><td>it represents the display name of your field. The name that is displayed to the user on the interface. For example, "<mark style="color:blue;">Sales Order</mark>".</td></tr><tr><td>Schema Name</td><td>It must be unique. It is used to create the logical name. This name should be in Pascal case. The schema name is used to create the class for the entity. For example, "<mark style="color:blue;">SalesOrder</mark>". </td></tr><tr><td>Plural name</td><td>refers to the name used to represent multiple instances of a data record entity. For example, "<mark style="color:blue;">Sales Orders</mark>"</td></tr><tr><td>Entity Type</td><td><strong>Data Record:</strong> refers to the structured data components that represent various business objects or concepts within the system. Each entity is associated with a set of fields that capture relevant information.<br><strong>Document Library:</strong> refers to a centralized repository where you can store and manage various types of documents, files, and attachments associated with records within the Ezra environment.<br></td></tr><tr><td>Owner Type</td><td>Defines the scope and nature of ownership for records within a specific entity.<br><strong>User</strong>: refers to individual users within the system that can be assigned as the owners of records that will have control over those records and can perform actions.<br><strong>Everyone</strong>: It signifies that records are owned by the system itself or are considered to be shared across the organization.</td></tr><tr><td><strong>Boolean Data Types</strong></td><td></td></tr><tr><td>☑ Business Process</td><td>it indicates that whether the associated entity or record type uses a business process flow to streamline and standardize workflows.</td></tr><tr><td>☑ Social Timeline </td><td>Indicates whether the entity or record supports the social timeline feature. The social timeline allows users to see social activities and interactions related to a record, such as posts and mentions from social media platforms.</td></tr><tr><td>☑ Enable Audit </td><td>Specifies whether auditing is enabled for the entity or record. Auditing tracks changes to records and provides a history of data modifications.</td></tr><tr><td>☑ Duplicate Detection </td><td>Determines whether duplicate detection rules are applied to the entity or record. Duplicate detection helps prevent the creation of duplicate records in the system.</td></tr><tr><td>☑ Attachments </td><td>Indicates whether the entity or record can have file attachments associated with it. If set to true, users can attach documents and files to the record.</td></tr><tr><td>☑ Customizable </td><td>It is set to default on Ezra. Specifies whether the entity or its forms, views, and fields can be customized. If set to true, users with appropriate permissions can customize the entity's structure and behavior.</td></tr><tr><td>☑ Searchable </td><td>It refers that the field value is searchable by the user in a case of categorized search or relevance search.</td></tr><tr><td>☑ Publish Create Event </td><td>Determines whether the entity is searchable. If set to true, records of this entity can be included in search results.</td></tr><tr><td>☑ Visible </td><td>Specifies whether the entity or field is visible to users. If set to false, the entity or field may be hidden from users, but it can still be used in the system.</td></tr><tr><td>☑ Read-Only</td><td>Specifies whether the entity or field is read-only. If set to true, users can view the data but cannot edit it.</td></tr><tr><td>☑ Required</td><td>Indicates whether a field is required for the creation or update of a record. If set to true, users must provide a value for this field when creating or modifying a record.</td></tr><tr><td>☑ Publish Create Event </td><td>Indicates whether an update event is published when a record of this entity type is created.  It can be used to trigger external processes or integrations when records are created.</td></tr><tr><td>☑ Publish Update Event</td><td>Indicates whether an update event is published when a record of this entity type is updated. It can be used to trigger external processes or integrations when records are updated.</td></tr></tbody></table>
