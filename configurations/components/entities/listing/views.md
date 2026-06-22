# Views

The image below shows the views available for the entity. <mark style="color:orange;">**#1**</mark> shows the list of a views available , and also in the grid table. The default views that gets created with the entity are, **Active**, **Inactive**, **Lookup** and **My** Views. To add a new view, click the "**Add New**" button the the form will open where you can add view information.

<figure><img src="../../../../.gitbook/assets/1 (1) (7).png" alt=""><figcaption><p><mark style="color:red;">Click image to view full screen</mark></p></figcaption></figure>

\
\
Then when modifying a existing view, you click at the point <mark style="color:$warning;">**#1**</mark> and then form will appear for you to modify. The following form will be display at point <mark style="color:$warning;">**#2**</mark> with the fields to be filled.&#x20;

<figure><img src="../../../../.gitbook/assets/1 (2) (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/1 (1) (8).png" alt=""><figcaption></figcaption></figure>

On the following example on <mark style="color:orange;">**#1**</mark> View Type we have the following different types of views.

<figure><img src="../../../../.gitbook/assets/1 (13).png" alt=""><figcaption></figcaption></figure>

| View Type        | Description |
| ---------------- | ----------- |
| System View      |             |
| Document View    |             |
| Search View      |             |
| Lookup View      |             |
| Quick Grid View  |             |
| Portal List View |             |
| Calendar View    |             |



<figure><img src="../../../../.gitbook/assets/1 (1) (9).png" alt=""><figcaption></figcaption></figure>

&#x20;Select <mark style="color:$warning;">**#1**</mark> **View Query** then type the SQL query in the  <mark style="color:$warning;">**#2**</mark> **Design View Query field** to filter your views.

<pre class="language-sql" data-title="The query for Active View"><code class="lang-sql"><strong>Select TOP 50 AccommodationId, Name FROM FilteredAccommodation WHERE IsActive = 1
</strong></code></pre>
