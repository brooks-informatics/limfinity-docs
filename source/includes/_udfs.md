# User Defined Fields (UDFs)
User-Defined Fields (UDFs) make up the subjects in Limfinity. There are many types of UDFs, each with its own corresponding value types. For example, a “Number” type UDF only supports the entering of numerical values, whereas a “Date” type UDF only supports date format values. UDFs are often set using scripts.

## Changing the way UDFs are displayed in grids
```javascript
if (value == null) {
return  '<span style="color:blue;font-weight:bold;"> No </span>'
} else return '<span style="color:red;font-weight:bold;"> '+val+' </span'
```
![Grid Renderer (JavaScript)](/images/guides/grid_renderer.png)

![Explorer Subject List View using Grid Renderer](/images/guides/grid_renderer_in_explorer.png)

## Making UDFs Required
```ruby
#Using the params[:required] script will ensure that each UDF with a 'true' parameter must have a value before the form can be submitted
params[:required] = {
    'Chem Analyses' => true,
    'Chemistry Package' => true
}
```
It is often necessary to make sure a UDF has a corresponding value when a form is being completed. Furthermore, it may be necessary to ensure that a value has been entered prior to the form submission completion. Using the `params[:required]` in **Before Script** can accomplish this.

## Disabling UDFs in Forms
```ruby
# Here we disable the 'Submitter Name' UDF, as it should be a read-only UDF
params[:disabled] = {
    'Submitter Name'=>true
}
```
UDFs can be disabled in forms by using the `params[:disabled]` parameter in **Before Script.** This parameter is used when specific UDFs in a form should be disabled, in order to enforce read-only access to the user.

## Skipping/Hiding UDFs in Forms
```ruby
# Here we skip the 'Submitter Name' UDF if it already has an entry
if subj.get_value('Submitter Name').present?
    params[:skip_UDF] = {
      'Submitter Name' => true
    }
end
```
It is possible to skip or hide a UDF when editing a subject form, by using the `params[:skip_UDF]` parameter. This is a very useful tool when logic is involved when editing a subject.

<aside class="notice">
As with most other parameter tools, the params[:skip_UDF] parameter must be entered in the “Before Script” of a tool or transition.
</aside>



