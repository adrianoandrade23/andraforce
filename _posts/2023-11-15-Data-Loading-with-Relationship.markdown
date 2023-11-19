---
layout: post
title:  "Data Loading with Relationship"
date:   2023-11-18 23:13:17 -0500
categories: Salesforce articles
---
<h3>Loading Data into Salesforce</h3>
<img src="{{site.baseurl}}/images/2023-11-15-Data-Loading-with-Relationship-Img-001.png" alt="External ID" class="article-image">

<p>Loading data into Salesforce that involves related records, like Accounts and Contacts, can seem challenging at first, especially for someone new to Salesforce. However, the process can be straightforward if you follow these steps. I'll explain it in simple terms, using an example of importing Account and Contact records, where Contacts are related to Accounts.</p>
<p>If you read this article until the end, you will be able to understand clearly the Loading Data Concept.</p>
<p>But before we jump into the example of loading an Account and Contact, let's first understand a very special concept: "External ID". After understanding this, everything will be clearer for you on how to load related data in Salesforce.</p>

<h3>What is an External ID?</h3>
<p>An External ID is a custom field that has been marked as an External ID. This field contains unique record identifiers from an external system. For example, if you're integrating Salesforce with an ERP system, the ERP's unique record ID can be stored in Salesforce as an External ID.</p>

<h4>Characteristics of External IDs</h4>
<ol>
  <li>Uniqueness: External IDs should ideally contain unique values to prevent duplication and ensure accurate mapping between systems.</li>
  <li>Indexed: Salesforce automatically indexes External IDs, which makes record lookups and queries faster and more efficient.</li>
  <li>Integration-Friendly: They are extremely useful in upsert operations (insert or update) where Salesforce uses the External ID to determine whether to create a new record or update an existing one.</li>
</ol>

<h4>Creating an External ID</h4>
<ol>
  <li>Choose or Create a Field: Identify which field you want to use as an External ID. This can be a custom field you create specifically for this purpose. Common data types for External ID fields are Text, Number, or Email.</li>
  <li>Marking the Field as an External ID:
    <ul>
      <li>Navigate to the Object Manager in Salesforce Setup.</li>
      <li>Select the object where you want to add the External ID.</li>
      <li>Create a new field or edit an existing one.</li>
      <li>In the field definition, check the option "External ID". Optionally, you can also enable "Unique" to prevent duplicate values.</li>
    </ul>
  </li>
  <li>Field Population: Ensure that the field is populated with unique values from the external system during data migration or integration.</li>
</ol>

<h4>Using External IDs in Data Migration</h4>
<ul>
  <li>Data Matching: When migrating data into Salesforce, use the External ID to match records in Salesforce with records in the external data source. This prevents duplicate record creation.</li>
  <li>Upsert Operations: In data migration or integration tools (like Data Loader), you can perform upsert operations. Salesforce uses the External ID to determine if it should create a new record or update an existing one.</li>
  <li>Relationships: External IDs can also be used to create relationships between records. For example, when importing contact records, you can use an account's External ID to associate each contact with the right account.</li>
</ul>

<h4>Best Practices</h4>
<ul>
  <li>Plan Ahead: Determine which fields will be External IDs before starting the migration process.</li>
  <li>Maintain Uniqueness: Ensure the values are unique in both Salesforce and the external system.</li>
  <li>Data Quality: Cleanse and standardize data in the external system to ensure smooth migration.</li>
</ul>

<h3>Is it a good idea to create external IDs in advance?</h3>
<p>Creating an External ID for every object in Salesforce as a proactive measure for potential future data export and load operations is an interesting strategy. Let's examine the pros, cons, and best practices for this approach.</p>

<h4>Advantages of Proactive External ID Creation</h4>
<ol>
  <li>Readiness for Integration and Migration: Having External IDs in place can simplify future data migration or integration with other systems. It ensures a unique identifier is available for each record, which can be critical for upsert operations.</li>
  <li>Data Consistency: External IDs can help maintain consistency between Salesforce and external systems, making it easier to track and update records across platforms.</li>
</ol>

<h4>Disadvantages or Considerations</h4>
<ol>
  <li>Complexity and Overhead: Adding an External ID to every object can increase the complexity of your Salesforce schema. More fields mean more potential points of maintenance and confusion, especially for users unfamiliar with the purpose of these fields.</li>
  <li>Potential Underutilization: If there's no immediate need for data integration or migration, these fields may remain unused, adding unnecessary clutter to your Salesforce environment.</li>
</ol>

<h4>Best Practices and Strategies</h4>
<ol>
  <li>Assess Future Needs: Evaluate the likelihood of needing integrations or data migrations. If it’s high, it makes sense to proactively add External IDs.</li>
  <li>Automate ID Generation: To avoid extra work for users:
    <ul>
      <li>Use auto-number fields as External IDs. Salesforce will automatically generate a unique number for each new record.</li>
      <li>Alternatively, use formulas or workflows to auto-populate External IDs based on existing data, ensuring they are unique.</li>
    </ul>
  </li>
  <li>Maintain Simplicity: Only create External IDs on objects where you foresee a realistic need for integration or data migration.</li>
  <li>Educate Users: Ensure that users understand the purpose of these fields, especially if they are involved in data entry or maintenance.</li>
  <li>Regular Review and Cleanup: Periodically review your use of External IDs. If certain fields are not being used and are not likely to be needed, consider removing them to simplify your system.</li>
</ol>

<h3>Scenario: Importing Account and Contact Records</h3>
<p>External IDs are particularly useful when importing related records, such as parent and child records, into Salesforce. They enable you to establish relationships between these records during the import process without needing to know the Salesforce ID of the parent record. Here's an example to illustrate how this works:</p>

<h4>Step 1: Preparing Account Records with External IDs</h4>
<ol>
  <li>In the External System:
    <ul>
      <li>Each Account record has a unique identifier, say "Account_Number".</li>
    </ul>
  </li>
  <li>In Salesforce:
    <ul>
      <li>Create a custom field on the Account object, named something like "External_Account_Number".</li>
      <li>Mark this field as an External ID.</li>
    </ul>
  </li>
</ol>

<h4>Step 2: Importing Account Records</h4>
<p>Import the Account records into Salesforce. Populate the "External_Account_Number" field with the unique "Account_Number" from your external system.</p>

<h4>Step 3: Preparing Contact Records for Import</h4>
<p>In your external system, each Contact record is linked to an Account via the "Account_Number". In your import file for Contacts, include a column for the Account relationship. Instead of using Salesforce Account IDs, use the "Account_Number" from the external system.</p>

<h4>Step 4: Importing Contact Records</h4>
<p>When importing the Contacts, map the column containing the "Account_Number" to the "External_Account_Number" field in Salesforce. Salesforce will use this mapping to associate each Contact with the correct Account based on the External ID.</p>

<h3>Example Data Table</h3>
<p>Here's a simplified view of what the data might look like:</p>


<h3>Account Data (External System)</h3>
<table border="1" cellspacing="0" cellpadding="5">
  <tr>
    <th>Contact_Name</th>
    <th>Account_Number</th>
  </tr>
  <tr>
    <td>John Doe</td>
    <td>1001</td>
  </tr>
  <tr>

    <td>Jane Smith</td>
    <td>1001</td>
  </tr>
  <tr>

    <td>Mike Ross</td>
    <td>1002</td>
  </tr>
</table>

 <h3>Contact Data (External System)</h3>
<table border="1" cellspacing="0" cellpadding="5">
  <tr>
    <th>Contact_Name</th>
    <th>Account_Number</th>
  </tr>
  <tr>
    <td>John Doe</td>
    <td>1001</td>
  </tr>
  <tr>
    <td>Jane Smith</td>
    <td>1001</td>
  </tr>
  <tr>
    <td>Mike Ross</td>
    <td>1002</td>
  </tr>
</table>

<h3>Salesforce Import Process</h3>
<p>Import Account data first, mapping "Account_Number" to "External_Account_Number". Then, import Contact data, using the "Account_Number" to relate Contacts to their respective Accounts in Salesforce.</p>

<h3>Why This is Useful</h3>
<p>This approach allows you to maintain relationships between records across systems without having to first import the parent records, obtain their new Salesforce IDs, and then update the child records with these IDs. It streamlines the process and reduces the chance of errors.</p>

