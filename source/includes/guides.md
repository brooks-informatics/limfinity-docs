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
4. Name your Subject Type "Patient" and specify the Plural Name - Patients Patient
![Plural Name](/images/guides/plural_name.png)
5. Click **OK** to save
