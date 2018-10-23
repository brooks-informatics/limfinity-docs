# Creating Custom Views and Forms
Custom views and forms may be created in Limfinity, in lieu of the default form formatting. By default, Limfinity
will build a default form containing all the UDFs that were passed in as parameters to the tool. The parameter UDFs
can be modified in before and after scripts (making fields required or disabled, adding input validation, etc). However, it is possible
to completely override the default form look and feel by replacing it with your own custom view.

## Creating a simple form
```ruby
# Before script: Open a form when on or more sample records are selected in the grid. Display a dialog with storage fields
raise "Please select at least one sample" unless subjects.length > 0

params[:dialog_options] = {maximizable: true, resizable: true, layout: ‘fit’}
params[:required] = {
'Freezer'=>true,
'Rack'=>true,
'Shelf'=>true,
'Date Stored'=>true
```

```ruby
# After script: Update each selected sample with storage information from the form and update their status. Display alert to the user when operation is completed.
subjects.each do |s|
  s.set_value('Freezer', params['Freezer'])
  s.set_value('Rack', params['Rack'])
  s.set_value('Shelf', params['Shelf'])
  s.set_value('Box', params['Box'])
  s.set_value('Slot', params['Slot'])
  s.set_value('Date Stored', params['Date Stored'])

  advance_workflow('Sample', 'Stored', s)
end

show_alert("#{subjects.length} Samples successfully stored to #{params['Freezer'].name}" )
```
1. Create a quick link or a workflow tool
2. Enter UDF names into the `Script Parameters` field (these UDFs must already exist in Limfinity)
![Script Parameters](/images/guides/script_params.png)
3. Create a before script (if you would like to apply attributes to the UDFs), or perform validation.
4. Create an after script with the funcionality that needs to happen after the user clicks on the tool (this can be creation of new subjects, updating data, or displaying alerts)

In this example, we are using `params[:dialog_options] = {maximizable: true, resizable: true, layout: ‘fit’}` to create a resizable dialog.

![Store sample](/images/guides/store_sample.png)

## Creating a custom form
```ruby
extend ScriptRunner::UI
params[:custom_fields] = encode_fields([item1, item2...])
params[:submit_disabled_as_empty] = true
```
In order to create custom forms and UIs, a few scripts must be used. The layout form the form can either be defined in the before script, or in a helper script (if the layout is very complicated).
view. A before script and after script are necessary for the tool. Once these three scripts are written
and in place, the custom form may be used.

<aside class="notice">
You must override the default fields by providing an array of items as the <code>custom_fields</code> property.
</aside>


To create a custom form:

1. Create a quick link or a workflow tool
2. Right-click on the workflow tool and click on **Before Script**
3. You must extend  the `ScriptRunner:UI` class
4. Create an array of items that you want to display as fields on the form, and encode them by using the `encode_fields` method
5. You must set the value of `params[:custom_fields]` property to the value returned by the `encode_fields`
6. If you wish to submit disabled fields as empty values, you can use the property `params[:submit_disabled_as_empty]`



## Custom form with a fieldset
```ruby
# This example creates a form with multiple fieldsets and rows containing UDFs
extend ScriptRunner::UI
params[:custom_fields] =  encode_fields([
  field_set(title:'2012-0846', items:[
    field_row([
      udf('Patient Name', subj)
    ]),
    field_row([
      udf('MRN_Number',subj)
    ]),
    field_row([
      udf('Consenting Protocol', subj, labelWidth:150)
    ]),
    field_row([
      udf'PSC Date', subj),
      udf('PSC Type', subj, flex:3)
    ]),
    field_row([
      udf('Principal Investigator', subj, labelWidth:150)
    ]),
    field_row([
      udf('Title', subj.get_value('Consenting Protocol'))
    ]),
    field_row([
    udf('Requesting Investigator', subj)
    ]),
    field_row([
      (udf'Surgery Date', subj),
      udf('Surgeon', subj),
      udf('Building ID', subj),
      udf('Room ID', subj)
    ]),
    field_row([
      udf('Preliminary Diagnosis', subj)
    ])
  ]),
  field_set(title:'Tissue Specific Information', items:[
    field_row([
      udf('Sample Type', subj)
    ]),
    field_row([
      udf'Organ/Sites', subj)
    ]),
    field_row([
      udf'Processing Details', subj)
    ])
  ]),
  field_set(title:'Special Instructions', items:[
    field_row([
      udf('Special Instructions', subj)
    ]),
  field_set(title:'Pickup Information', items:[
    field_row([
      udf('Pick Up Date/Time', subj)
    ]),
    field_row([
      udf('Contact for Pickup', subj)
    ])
  ])
])
```
This example demonstrates how to create a form with multiple fieldsets and multiple rows. Each row is created
by calling `field_row` and passing it one or more udfs.

![Form with multiple fieldsets](/images/guides/form_with_fieldsets.png)


## Custom form with hide/show logic
```ruby
# Create a custom form with a radio button and a field set. Here we want to show the field set only when
# the value of the 'Find Requsitions' radio button is 'Submitted'
extend ScriptRunner::UI
params[:custom_fields] = encode_fields([
    udf('Find Requisitions', nil, labelWidth:160, required:false),
    field_set(title:'Find By Properities', border:'1 0 0 0', items:[
        udf('External Source ID', nil, info:'This field can be used for the Sample Barcode Entry',
          allowCreate:false, labelWidth:160, anchor:'100%'),
        udf('Requisition', nil, labelWidth:160, allowCreate:false),
        udf('Patient', nil, labelWidth:160, allowCreate:false),
        udf('Requesting Physician', nil, labelWidth:160, allowCreate:false),
        udf('Collection Date', nil, labelWidth:160, anchor:'50%'),
      ],
      react: {
        shown_when: 'value == "Submitted"',
        only: 'Find Requisitions'
    })
  ])
```
You can attach listeners to any field by using the `react` property. In this example, we will hide/show a fieldset based on a value
of a radio button. In order to hide or show a component you must use the `shown_when` property, and pass it an expression which evaluates to true or false.
The `only` property can specify a component that the field will listen to. If `only` is missing, the field will listen to changes on all
fields on the form.

![Find Requisitions](/images/guides/find_requisitions.png)


## Populating form fields with values from other fields
```ruby
# In this example we first pick a Billing Facility, and then auto-populate the Billing Facility Address field with the
# value of the facility's address stored in the database.
field_container([
      udf('Billing Facility', subj, labelWidth:160, required: true, allowCreate:false, filter: "#{facility_filter}"),
      udf('Billing Facility Address', subj, labelWidth:160, anchor:'100%', height:40, required: true, validateAddress:true, react:{
        change:'form.setValuesAsync({id: value, udfs: {"Address":"Billing Facility Address"}})',
        only: 'Billing Facility'
      })
    ],
    react: {
      shown_when: "value == 'Facility'",
      only: 'Bill To'
    }
)
```
This example demonstrates how you can populate fields with values from another field. You can use the `change` listener to
call either `setValue` or `setValuesAsync` (if you want to suspend listeners while the form is being populated).


## Filtering dropdowns in forms
```ruby
# In this example we only want to show tests that are already on the requisition, and are applicable to all specimen types or our specimen types
udf('Tests', nil, value:tests, required:true, allowCreate:false,
    filter: "<-Requisition::\"Tests\" = #{req.id} AND (\"Specimen Types\" = #{specimen.id} OR \"Specimen Types\" = null)",
    react: {
      change: "
        this.setFilter(\'<-Requisition::\"Tests\" = #{req.id} AND (\"Specimen Type\" = \'+value+' OR \"Specimen Types\" = null)');
        this.setValue(#{tests_by_specimen.to_json}[value]);",
      only: 'Specimen Type'
    }),
# Here we only want to see tests that are not archived and are orderable (i.e clients can order them)
udf('Non-Orderable Tests', nil, value:non_orderable_tests, filter: 'terminated = false AND "Non-Orderable" = true', allowCreate:false),
```

This example demonstrates how you can filter the values of dropdowns by a criteria or an expression. There are multiple ways to set filters.

1. The simplest way to filter something is to set the `filter` property on a UDF, like so: `filter:"terminated is false",`.
2. You can plug in your variables into the ruby template by following this example: `filter:"Facility = #{facility ? facility.id : 0} and terminated is false"`
3. You can also pass in a filter expression like so: `filter: "#{facility_filter}")`
4. Filter by state: `filter: "#state = Active""`

## UI class methods

```ruby
udf('Requesting Physician', subj, addCustomFields: physician_layout(subj, 2), labelWidth:160, required:true,
  filter:"Facility = #{facility ? facility.id : 0} and terminated is false",
  react: {
    shown_when: 'value',
    change: "this.setFilter('terminated is false and Facility='+(value || 0));",
    only:'Facility'
}),
```
### udf
Creates a UDF (user defined field).

Parameter | Description
--------- | -----------
:name | UDF name
:subj | Subject containing the UDF
:options | The options object

```ruby
 field_row([
  udf('Decimal Places', subj),
  udf('Unit', subj, labelWidth:60)
]),
```
### field_row
Creates a field row by taking an array of fields and laying them out in one  row

Parameter | Description
--------- | -----------
:items | Array of items to place into a form row
:options | The options object

```ruby
field_container(fields, defaults: {anchor:'100%', labelWidth:160}, react:{
  init: 'this.setDisabled(!value)',
  only: 'Patient'
})
```
### field_container
Lays out passed in items in a field container. Useful if you want to apply the same styling to all items within a container

Parameter | Description
--------- | -----------
:items | Array of items to place into a form row
:options | The options object

```ruby
# Example fieldset
field_set(title:'General Information', border:'1 0 0 0', defaults:{anchor:'100%'}, items:[])
```
### field_set
Creates a ExtJs 4.2.2 fieldset with the given options

Parameter | Description
--------- | -----------
:options | The options object

### choice_by_query
Creates a Choice type UDF but with the drop-down list filled by query

Parameter | Description
--------- | -----------
:name | The UDF name
:subj | Subject containing the UDF
:query | The query object that will produce a set of results to populate the picklist
:options | The options object to pass to the picklist

### custom_radio_group
Creates a radio group from given options

Parameter | Description
--------- | -----------
:args | A configuration object, consisting of args[0] = name for the radio group and args[1] = the options object.
The options object can contain an `items` property with each item having a `name` and `checked` properties.

### udfs_for_subject_type
Returns a list of UDFs that exist for a given subject type in Limfinity

Parameter | Description
--------- | -----------
:st | The subject type
:subj | Subject containing the UDF
:customizations | Object containing any additional properties for the UDFs such as "readOnly", or "anchor

```ruby
subj = UI.add_subject('Analyte', params, {name: name})
```
### add_subject
Creates a subject populated with the values from the params

Parameter | Description
--------- | -----------
:st | The subject type
:params | The params (usually from the custom form/view)
:options | The options object

```ruby
UI.save_subject(subj, params, name != subj.name ? {name: name} : {})
```
### save_subject
Saves a subject populated with the values from the params. This method performs access controlled read and write operations, and
writes to the audit trail.

Parameter | Description
--------- | -----------
:subject | The subject to save
:params | The params (usually from the custom form/view)
:options | The options object. If the options object contains "name" or "gen:name" properties, they will be used to create a new subject name

```ruby
 opts = count ? {count: count} : {}
 ScriptRunner::UI::search_tab_link(subj_type_name, query, "style=\"color:white; text-decoration:none;\" onmouseover=\"this.style.color='black'\"  onmouseout=\"this.style.color='white'\"", tab_name, opts)
```
### search_tab_link
Generates a link opening a search tab or showing '0' if nothing found

Parameter | Description
--------- | -----------
:subject_type_name | The subject type to search
:query | The query to run the search
:tab_opts | Object containing tab parameters
:link_attrs | Link attributes
:tab_anem | Name for the new tab

```ruby
#Example for Group Edit Component - display grid
# Displays a grid with 4 rows, each containing an editable collection kit record
params[:tool_message] = "Create up to 5 Collection Kits<br><br>"
extend ScriptRunner::UI
column_configs =  {
    'Quantity' => {minValue: 1}
  }

params[:custom_fields] = encode_fields([
   group_editor('Add_Collection_Kits', [
     {0=>subj.get_value('Facility'), 3=>0},
     {0=>subj.get_value('Facility'), 3=>0},
     {0=>subj.get_value('Facility'), 3=>0},
     {0=>subj.get_value('Facility'), 3=>0}
     ], name: 'custom[data]', flex: 1)
])
params[:custom_fields_place] = 'top'

```

```ruby
# script here to process the data from the Group Edit table
# This script reads all the data out of each row and creates one collection kit per row
# The editable fields are defined in the "Add Collection Kits" workflow tool
extend ScriptRunner::UI
names = []
count = 0
data = parse_group_edit_data(params['data'])
data.each do |row|
  quantity  = row.fetch(row.keys.find{|p|p[:display_name] == 'Quantity'})
  specimen_type = row.fetch(row.keys.find{|p|p[:display_name] == 'Specimen Type'})
  facility = row.fetch(row.keys.find{|p|p[:display_name] == 'Facility'})
  components = row.fetch(row.keys.find{|p|p[:display_name] == 'Kit Components'})
  
  if quantity > 0
    for i in 1..quantity
        kit = create_subject('Collection Kit') do |kit|
        kit.set_value('Facility', facility)
        kit.set_value('Specimen Type', specimen_type)
        kit.set_value('Kit Components', components)
        kit.advance_workflow('Collection Kit', 'Requested')
        count += 1
      end
    end
 end
  end
show_alert("Created #{count} Collection Kits") if count != 0
```
### group_editor
Embeds an editable grid within a dialog

Parameter | Description
--------- | -----------
:tool | The instance of the subject group edit tool defined in the workflow builder
:content | An array containing the configuration for each row that will be displayed in the embedded grid. 
:options | An array of options: payloads, column configs or anything else that needs to be passed ot the component

### select_template_field
?????????????

Parameter | Description
--------- | -----------
:subject_type | The name of the subject type
:options | An array of options

### select_printer_field
?????????????

Parameter | Description
--------- | -----------
:options | An array of options
