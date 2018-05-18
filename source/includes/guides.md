# Guides

# Modelling your data with Limfinity

This guide walks you through the process of replicating your data model with Limfinity. In this guide you'll learn how to:

* Create a new subject type
* Create a subject
* Create new user-defined-fields
* Configuring grid options for a subject

In the sections below, we'll create a "Patient" subject that will be linked to a "Sample.


## Step 1: Create a new Subject Type
A `Subject Type` defines the type of data you can store. Think of it as a form, or a table.

Each subject type is annotated with `user-defined fields` (UDFs) that capture the metadata for that object. Each instance of a subject type is called a subject.

1. Log in and click on Settings and Preferences
2. Click on Subject Types
3. Click the plus button to create a new Subject Type New Subject Type
![New Subject Type](/images/guides/new_subject_type.png)
4. Name your Subject Type "Patient" and specify the `Plural Name` - Patients
![Plural Name](/images/guides/plural_name.png)
5. Click **OK** to save

## Step 2: Adding User-Defined-Fields to your Subject Type

Now we have a new "Patient" subject type, but it has no fields. Let's add a couple:

1. Click on the "Patient" subject type
2. Click on the `Add User Defined Field` button and choose `Date` as the field type
3. Enter "DOB" for the User Field Name and check `Advanced Search`
![New UDF](/images/guides/new_udf.png)
4. Let's add a couple other fields: Phone Number (Text Field), Consent Signed (Checkbox), Gender (Choice - Male, Female, Both), Comments (Text Area)
5. Click **OK** to Save 

<aside class="notice">
Note that it is necessary to check *Advanced Search* in order to make the field available in queries
</aside>

