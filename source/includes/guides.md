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
Note that it is necessary to check <b>Advanced Search</b> in order to make the field available in queries
</aside>

Let's also create another subject type and name it "Sample" with the field Amount (Numeric). What we want to do now is to link the sample to the patient it has been collected from.

1. Open the "Sample" subject type
1. Click on `Add User Defined Field` button and choose `Limfinity` -> `Subject` as the field type
2. Enter "Patient" as the User Field Name and select `Patient` as the `Subject Type
![New UDF](/images/guides/subject_udf_patient.png)
3. Click **OK** to Save


## Step 3: Creating a subject
Our 'Patient' subject type has fields defined, but we do not yet have any actual patients. 

Let's make a patient:

1. Click on `Explorer`
2. Click on Patients
3. Click the plus button to add a new subject
4. Enter values for DOB, Gender, Consent Signed, Phone number, Comments and click OK to save the new subject
![New UDF](/images/guides/new_patient.png)

## Configuring the subject type grid
Our newly added fields are not  yet showing up on patient result grids. Let's configure the grid and add our new fields:
<aside class="notice">
Configuring grid View Options allows you to specify which columns should be shown in a grid for that subject as well as the order/width of the columns.
</aside>

1. Click on `Settings and Preferences`
2. Click on `Subject Types`
3. Select Patient and click `View Options`
4. Add fields DOB, Gender, Phone Number, Consent Signed and Comments
5. Remove the UID field
6. Move the fields DOB, Gender, Phone Number and Consent Singed to the top and click **OK**
![New UDF](/images/guides/grid_config.png)


# Creating workflows
This guide walks you through the process of creating a workflow. In this guide you will learn how to:

* Create a new new workflow
* Create workflow states
* Add tools to a workflow
* Take a subject through a workflow

In the sections below, we'll create an "Issue" workflow. Let's start by creating an "Issue" subject type with fields:

1. Description (Text Area)
2. Status (Choice)
3. Assignee (User)
4. Resolved Date (Date)

## Step 1: Create a new Workflow
Each workflow can only be attached to one single subject type, but subject types can belong to multiple workflows. A workflow describes how subjects go through processes and what actions are available for each state

1. Click on `Settings and Preferences`
2. Click on the plus button and select the desired subject type ("Issue)
<aside class="notice">
If you just created a new subject type, you will need to refresh the page for it to show up in the dropdown
</aside>
3. Enter a name for the workflow. The convention is to name it the same as the subject type, so let's put in "Issue" as the worklfow name
4. Click on the workflow to open it in the workflow editor window
![New UDF](/images/guides/new_workflow.png)

## Step 2: Add states
We have a brand new "Issue" workflow, but it has no states. Each record in the workflow will go through a sequence of states that model the workflow.
For our "Issue" workflow, we know that it can either be "Open" or "Resolved". 
1. Click on the plus button to add a new state
![New UDF](/images/guides/new_workflow_state.png)
2. Let's call the new state "Open". This new state will appear on the canvas. By default, this state will be the entry state for all new subjects in the "Issue" workflow, but you can always change the first state by specifying a new "Entry State" in workflow editor
3. Create another state called "Resolved"
![New UDF](/images/guides/resolved_state.png)

## Step 3: Adding a workflow transition
Now that we have both "Open" and "Resolved" states, let's link it together. To do this, you will need to create a tool (it can be any type of tool)
1. Click on the plus button in the `Tools` tab
2. Give your tool a name, and a title. `Name` is the internal identifier, while `Title` is the user-facing value
3. Specify the `Input Subject Type` = Issue
4. Specify the `Workflow Transition` = Resolved
5. Drag the tool into the "Open" state
![New UDF](/images/guides/transition.png)
5. You should now see an arrow stretch the "Open" state to the "Resolved" state
![New UDF](/images/guides/linked_states.png)

Let's add another tool to create a new Issue:

1. Create a `Create New Subject` tool and name it "New Issue"
2. Fill out the `Name`, `Title`, `Output Subject Type`, and `Workflow`
3. Click `OK` to create a new Issue record
 

## Step 4: Adding tool groups
Starting with Limfinity 7.1 you can now reuse individual tools or multiple tools in many workflows by placing them into tool groups. Tool groups can be used in many places such as `Quick Links` (buttons that appear in the left nav bar) or workflows.

To create a toolgroup:

1. Click on the plus button in the `Tool Groups` tab
2. Fill out the `Name` = Issues and the `Title` = Issues fields
![New UDF](/images/guides/new_tool_group.png)
3. You can specify whether to display the tool group as a quick link, or as a set of buttons on the workflow state. Let's place this toolgroup in the Quick Links bar.
4. Add the "Resolve" and the "New Issue" tools to this tool group
5. Refresh to see a folder with the workflow tools

## Step 5: Taking a subject record through a workflow
Let's take an Issue from being open to resolution:

1. Click on `Explorer`. and select `Issues`
2. Click `New Issue`
3. A "New Issue" dialog will open
4. Fill out the data to create a new Issue
4. Upon saving, the page for the new record will open and you should see a "Resolve" button on the right of the page

