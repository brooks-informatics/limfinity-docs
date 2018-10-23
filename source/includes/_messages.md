# Messages
There are two different types of messages, which can be displayed via scripts in Limfinity: messages and alerts. Messages are displayed in confirmation dialogs, which the user must interact with, and alerts are displayed in a dropdown message for a short amount of time.

## Showing Messages
```ruby
#  Example: Showing Messages
#Here we use the show_message() method to provide the user with a simple output message
show_message("All samples have been selected for analyses. Please click 'OK' to proceed.")
```
```ruby
#  Example: Showing Alerts
# Here we provide the user with a simple alert message, which they do not need to acknowledge
show_alert("You are now in the next workflow state")
```
Messages with confirmation dialogs are shown using the show_message() in After Script tab. The user will be required to click a confirmation “OK” button to acknowledge that they have seen the message. show_message() dialogs are shown after all other code has been executed.

<aside class="notice">
Messages are different from alerts in one way, in that messages require the acknowledgement of the message via a confirmation selection. Alerts are shown in a dropdown textbox for only a short period of time.
</aside>

<aside class="notice">
There is only a single parameter for the show_message() method, and that is the message itself. The message must be typed in quotes, as such: “Example”.
</aside>

## Text Messages from Input Forms
```ruby
params[:tab_name] = 'Samples'
params[:subject_type] = 'Sample'
params[:query] = search_query do |qb|
    qb.and(
      qb.state('Results Entry')
    )
end
params[:hint_msg] = '<p style="color:red">List of Samples.</p>'
params[:tb_items] = [
   {xtype: 'box', html: ''<span style =\'color:darkgreen\'>Result Entry State</span>'}
]
```
```ruby
# Here we use a tool message to inform the user. THis message will be displayed in the tool itself
params[:tool _message] = "Prior to performing this action, ensure that all other actions are complete"
```
Text messages can be added to input forms by using the `:tool_message` parameter.
<aside class="notice">
The :tool_message parameter must be added to the “Before Script” of a tool or transition. With this tool, text is added to a form which will be displayed.
</aside>

<aside class="notice">
The :hint_msg and :tb_items parameters can be added to “Before Script” which will be displayed in the tool bar of any subject list.
</aside>

![Subject List with toolbar message](/images/guides/toolbar_message.png)

<aside class="notice">
There is a single difference between a tool message and a show message, and that is that tool messages are shown in combination with the confirmation dialog of a tool, rather than after the confirmation dialog has been closed.
</aside>
