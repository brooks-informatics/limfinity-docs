# Sending Emails
```ruby
# In this example, an array of recipients is first create,d and then an email is setn to the recipients using an email template by the name of "Example Template", regarding the opened subject
# Create an array of recipients
recipients = []

# Add all members of the "User Group 1" group to the recipients array
recipients << find_user_group('User Group 1)
# Add all members of the "User Group 2" group to the recipients array
recipients << find_user_group('User Group 2)

# Send an email using the email template "Example Template" regarding the current subject
send_email( recipients , find_email_template('Example Template'), subj)
```
Limfinity supports the automatic sending of emails through scripts, if the installation is set up with proper emailing credentials. The `send_email()` method is used to send emails to users with registered email addresses to their user accounts.

<aside class="notice">
The send_email() method takes three parameters. The first parameter is the recipient or list of recipients. The second parameter is the email template, found by name in the system. The final parameter is the subject.
</aside>

<aside class="notice">
Note that an email template is always required to send an email. Email templates must be created in the system in order to successfully send an email.
</aside>

In this example, an array of recipients is first created. Recipients can be specific users, or entire user groups. User groups are added to the recipients array in this case.
After the recipients have been assigned, the `send_email()` method is called. The method takes the first parameter of the recipients, the second of the defined email template, and the last parameter of the opened subject.
*Note that an email template is always required to send an email. Email templates must be created in the system in order to successfully send an email.

## Sending an email with a file attachment
```ruby
# recipient email address
email_addr = 'test@brooks.com'

# This is the email template in the system
template = "Email Test"

# This is the 'File' UDF set in the 'Script Parameter'
if params['ATTACHED FILE'].present?
    file = params['ATTACHED FILE']
    subj.set_value('ATTACHED FILE', file)
end

# the attached file
rfile = subj.get_value('ATTACHED FILE')

send_email(email_addr, template, subj) do |e|
    e.add_attachment(rfile, rfile.to_s, rfile.to_json['mime_type'])
end

show_message('Email sent')
````
Files can be attached to the email using ‘File’ UDF.  When creating the email workflow tool, the field ‘Script Parameter’, which is the ‘File’ UDF, needs to be selected.  The After Script will contain the reference of the script parameters.

