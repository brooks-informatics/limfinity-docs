# API

**List of Objects Supported by Limfinity API**
<aside class="notice">
Following is a list of Objects and their identifying values that are called by supported methods. Scroll down for a list of corresponding methods.
</aside>

##  AuditRec
A record of any change within the system.

Parameter | Description
--------- | -----------
:id | Unique ID of an audit record.
:obj_name | Name of the changed object.
:user | The name of the user that made the change.
:obj_type | The type of the object changed.
:created_at | The date the audit record was created.
:message | The action performed that created the audit.
:comments | Any user-generated comment added when the audited action was performed.

## Auth_token
An authorization token used in place of a user’s password for API calls. Using the auth_token will omit the “API Session Created” and “API Session Removed” entries in the audit log.

## Users
Uniquely identifies a user in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a user.
:username | User’s Login name.
:fullname | User’s full name.
:email | User’s email.
:created_at | Date when the user was created in the system.
:roles | Roles assigned to the user.
:disabled | Defines whether the user is currently disabled.
:locked =|Defines whether the user is currently locked.
:active | Defines whether the user is currently active.
:groups | List of User Groups this user belongs to.

## Group
Uniquely identifies a user group in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a user group.
:name | User group’s name.
:description | Description of the group.
:created_at | Date when the group was created in the system.
:created_by | User name who created this group.
:updated_at | Date when the group was last updated.
:users-count | Number of users in the group.
:baseline | Placeholder text.

## Role
A role that defines the access permissions in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a role.
:name | Role’s name.
:rights | A list of rights assigned to the role.
:baseline | Placeholder text.
:created_at | Date when the role was created in the system.
:system_role | Defines whether the role is internal (built-into the software) or user-defined.

## UserField
A user-defined field in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a user-defined field.
:name | Internal name of the user-defined field.
:display_name | Display (human readable) name of user-defined field.
:type | User-defined field type (“Date”, “Text Field”, “Text Area”, “Checkbox”, etc.) :searchable = Defines whether the user-defined field is searchable.
:values | User-defined field values.
:created_at | Date when the user-defined field was created in the system.
:updated_at | Date when the user-defined field was last updated.
:created_by | User name who created this user-defined field.
:used_by | Which objects use the user-defined field.
:permission | Permissions applicable to the user calling a method which returns this object.

## SubjectType
A user-defined subject type in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a subject type.
:name | Internal name of the subject type.
:descr | Description of the subject type.
:color | Color of the icon representing the subject type.
:searchable_quick | Defines whether the subject type may be found in a quick search. :searchable_advanced = Defines whether the subject type may be found in an advanced search. :searchable_batch = Defines whether the subject type may be found in a batch search.
:fields | List of user-defined properties.
:searchable | The subject type is searchable within the system.
:syslock | Indicates if the object is “locked” in the configuration, and can’t be changed regardless of the site. :baseline = Indicates if the object is part of the baseline configuration.
:created_at | Date when the subject type was created in the system.
:updated_at | Date when the subject type was last updated.
:created_by | User name who created this subject type.
:enabled | Enabled or disabled flag for this subject type.
:permission | The permissions available to this subject type.
:configuration | The subject type is part of the system configuration.
:fields_count | Number of user-defined fields in this Subject Type.

## SubjectTypeGroup
A user-defined group of subject types in the system:

Parameter | Description
--------- | -----------
:id | Unique ID of a subject type group.
:subject_types | The subject types associated with this group.
created_at | Date when the subject type group was created.
baseline | Indicates if the object is part of the baseline configuration.
updated_at | Date when the subject type group was last updated.
created_by: | User name who created this subject type group.

## Subject
Any user object in the system. Subjects are instances of a particular Subject Type:

Parameter | Description
--------- | -----------
:id | Unique ID of a subject.
:name | Subject name.
:barcode_tag | The unique barcode number assigned to the subject. :rid_tag = The unique RFID number assigned to the subject. :subject_type = Subject type name (Example: Bacteria or Sample).
:created_at | Date when the subject was created in the system.
:updated_at | Date when the subject was last updated.
:terminated | Flag if subject is terminated and no longer part of process/workflow. :user = Username of a subject owner.
:permission | The permissions available to this subject.
:flow_states | Workflow states which the object has been in.
:created_by | The user that created the subject.
:updated_by | The user that last updated the subject.
:udfs | The number and names of all user-defined fields associated with this subject.

**List of API Functions**
<aside class="notice">
Following is a list of Methods along with the returned objects they call along with any required, query, or control parameters to use with them. An explanation of each is as followed:
</aside>

**Returned Objects**

LIMFINITY objects returned by API function.

**Required Parameters**

Necessary parameter or parameters for the method to run correctly. Without the required parameters an error message from the server should be expected. These will be written in the format required for the script along with placeholder text within the ‘ ’ marks.

**Optional Query parameters**

Optional parameters to control the results. These will be written in the format required for the script along with placeholder text within the ‘ ’ marks.

**Optional Control parameters**

Optional parameters to control the number and order of records in the output, and to implement paging of the results. These will be written in the format required for the script along with placeholder text within the ‘ ’ marks.

## audit
```ruby
#Example #1 - This API will print the total number of Audit Records and list all related information.
#add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :date_flag=>'all/today/yesterday/week/month': Allows user to search for a specific date #, :date_range=>'date from,date to': Allows user to search a specific date range
#, :subj_ids=>'value,value': comma-separated Subject IDs to get audit records
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'text': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'audit'#Copy and paste optional query and/or control parameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "AuditRecs"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s)
      end
    end
  end
end
```
Retrieves a list of audit records of every change made within the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects:** | AuditRecs |
**Required Parameters:** | None |
**Optional Query Parameters** | :date_flag=>’all/today/yesterday/week/month’ | Displays audit records made today, yesterday, this week, or this month.
                             | :date_range=>’date_from,date_to’ | Displays audit records made within a specific date range(format: mm/dd/yyyy).
                             | :subj_ids=>’value,value’ | Displays audits records of a specific range of subjects.
**Optional Control Parameters**  | :start=>’value’ | The item specific ID to start the list with.
                              | :limit=>'value' | The total number of items to retrieve.
                             | :sort=>'text' | Sort the displayed results by a specific field such as subject_type, subject_name, etc.
                             | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.

## users
```ruby
#Example #2 - This API will print the total number of Users and list all related information.
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks
req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'users'

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "Users"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s)
      end
    end
  end
end
```

Retrieves a list of users within the system.

Property | Description
--------- | -----------
**Returned Objects** | Users
**Required Parameters** | None
**Optional Query Parameters** | None
**Optional Control Parameters** | None


## users_groups
```ruby
#Example #3 - This API will print the total number of User Groups and list all related information.
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'user_groups'

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "Groups"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s)
      end
    end
  end
end
```
Retrieves a list of user groups within the system.

Property | Description
--------- | -----------
**Returned Objects** | Groups
**Required Parameters** | None
**Optional Query Parameters** | None
**Optional Control Parameters** | None

## roles
```ruby
#Example #4 - This API will print the total number of Roles and list all related information.
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks
req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'roles'

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "Roles"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s) end
    end
  end
end
```
Retrieves a list of roles within the system.

Property | Description
--------- | -----------
**Returned Objects** | Roles
**Required Parameters** | None
**Optional Query Parameters** | None
**Optional Control Parameters** | None

## userfields
```ruby
#Example #5 - This API will print the total number of User Fields and list all related information. #add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :query=>'text': optional search string to filter the results.
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'value': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'userfields'#Copy and paste optional query and/or co ntrol parameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "UserFields"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s) end
    end
  end
end
```
Retrieves a list of user-defined fields within the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | UserFields |
**Required Parameters** | None |
**Optional Query Parameters** | :query=>’text’  | Displays a list of user-defined fields containing a specific text string.
**Optional Control Parameters** | :start=>’value’ | The item specific ID to start the list with.
                         | :limit=>'value' | The total number of results to retrieve.
                         | :sort=>'text' | Sort the displayed results by a specific field such as subject_type, subject_name, etc.
                         | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.

## subject_types
```ruby
#Example #6 - This API will print the total number of Subject Types and any relevant information
#add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :query=>'text': optional search string to filter the results.
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'value': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'subject_types'#Copy and paste optional query and/or control parameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "SubjectTypes"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s) end
    end
  end
end
#puts data
```
Retrieves a list of subject types within the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | SubjectTypes |
**Required Parameters** | None |
**Optional Query Parameters** | :query=>’text’  | Displays a list of subject types containing a specific text string.
**Optional Control Parameters** | :start=>’value’ | The item specific ID to start the list with.
                         | :limit=>'value' | The total number of results to retrieve.
                         | :sort=>'text' | Sort the displayed results by a specific field such as subject_type, subject_name, etc.
                         | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.
## subject_groups
```ruby
#Example #7 - This API will print the total number of Subject Type Groups and any relevant informatio n
#add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :query=>'text': optional search string to filter the results.
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'value': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'subject_groups'#Copy and paste optional query and/o r control parameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "SubjectTypeGroups"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s) end
    end
  end
end
#puts data
```
Retrieves a list of subject type groups within the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | SubjectTypes |
**Required Parameters** | None |
**Optional Query Parameters** | :query=>’text’  | Displays a list of subject type groups containing a specific text string.
**Optional Control Parameters** | :start=>’value’ | The item specific ID to start the list with.
                        | :limit=>'value' | The total number of results to retrieve.
                        | :sort=>'text' | Sort the displayed results by a specific field such as subject_type, subject_name, etc.
                        | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.
## subjects
```ruby
#Example #8 - This API will print the total number of Subjects and any relevant information
#add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :subject_ids=>'value': limit subjects to a list of IDs
#, :subject_names=>'text': limit subjects to a list of names
#, :user_fields=>'text': comma-delimited list of User Defined fields to retrieve for subjects #, :query=>'text': optional search string to filter the results.
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'value': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'subjects', :subject_type=>'Patient'#Copy and paste optional query and/or control parameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
puts data
data.each do |key,value|
  if key == "Subjects"
    value.each do |types|
      puts "--------------" types.each do |k,v|
        puts (k.to_s + ": " + v.to_s)
      end
    end
  end
end
#puts data
```
Retrieves a list of subjects within the system.


Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | Subjects |
**Required Parameters** | :subject_type=>’value/text’  | Limits the search to all subjects of a specific type(can use subject type name or specific ID).
**Optional Query Parameters** | :subject_ids=>’value,value’ | Limits subjects to a list of item specific IDs.
                              | :subject_names=>’text,text’ |  Limits subjects to a list of item specific names.
                              | :query=>’text’ | Displays a list of subjects containing a specific text string.
**Optional Control Parameters** | :start=>’value’ | The item specific ID to start the list with.
                                | :limit=>'value' | The total number of results to retrieve.
                                | :sort=>'text' | Sort the displayed results by a specific field such as subject_type, subject_name, etc.
                                | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.
## delete_subject
```ruby
#Example #9 - This API will delete a Subject as specified by ID or a combination of subject_type/subj ect_name
#add any of the Optional query parameters by copy and pasting <, :function=>'value'> after <:method=> ''>
#, :subject_type=>'value': subject type ID
#, :subject_name=>'text': subject name
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url  = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks
req  = Net::HTTP::Post::Multipart.new url.path,
                                      :username=>'admin', :password=>'admin', :method=>'delete_subject', :id=>'13'#replace <:id=>'value '> with <:subject_type=>'value', :subject_name=>'text'> for optional query parameters

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
puts "-------------------"
puts data.to_json
puts "-------------------"
```
Deletes a subject as specified by ID.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | None |
**Required Parameters** | :id=>’value’ | The specific ID of the subject to be deleted, OR
                        |  :subject_type=>’value/text’ | Limits the search to all subjects of a specific type(can use subject type name or specific ID).
                        | :subject_name=>’text’ | Limits the search to all subjects matching the specified name
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

## gen_token
```ruby
#Example #10 - This API will generate and print an "auth_token" to be used instead of a user's pass word. Using the auth_token will omit the API Session Created
# and API Session Removed  entries in the audit log
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks = Net::HTTP::Post::Multipart.new url.path,

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'gen_token'

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

str = res.body
puts str #Output the generated authentication token
```
Creates an “auth_token” to be used instead of a user’s password. Using the auth_token will omit the “API Session Created” and “API Session Removed” entries in the audit log.

<aside class="notice">
<b>Note:</b> Generated Authorization Tokens are only valid for 10 minutes after they are either generated or last used. If an expired or invalid token is used, the following error message should be displayed: “Invalid Token”.
</aside>

Property | Description
--------- | -----------
**Returned Objects** | Auth_token
**Required Parameters** | None
**Optional Query Parameters** | None
**Optional Control Parameters** | None

## subject_details
```ruby
#Example #11 - This API will Print all details of a Subject as specified by ID, Barcode, and RFID
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks 8.
req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'subject_details', :subject_id=>'9',
                                     :barcode_tag=>'L04000009', :rfid_tag=>'355AB1CBC000004000000009'

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total

data.each do |key,value|
  puts (key.to_s + ": " + value.to_s)
end
```
Deletes a subject as specified by ID.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | A list of values corresponding to a specific subject |
**Required Parameters** | :id=>’value’ | The specific ID of the subject to be displayed.
                        | :barcode_tag=>’value’ | The specific barcode tag of the subject to be displayed.
                        | :rfid_tag =>value | The specific RFID tag of the subject to be displayed.
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

## run_script
```ruby
#Example 12 - This API will execute a run_script / helper script by specifying the name of the script in the system and the data which should be passted to it.
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://demo.limfinity.com/api') 8.
    req = Net::HTTP::Post::Multipart.new url.path, :username=> "admin",
                                         :password=> "admin",
                                         :name=> "demo_helper_script",
                                         :data=> { "Name": "Sample 1", "BARCODE": "123456", "Specimen Name": "Specimen 1", "Created": "10/28/2014", "Created By": "User 1", "Current Amount": 10.2 }

res = Net::HTTP.star(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
puts data.to_json
```
Allows the running of a helper script by name and the passing of arbitrary JSON-formatted parameters to that script.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | Helper script-returned serialized JSON |
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |
**Required Parameters** | :name=>’helper_script_name’ | The name of the helper script to be run.
                        | :data=>’’ | Arbitrary data to be passed to the script represented as a JSON object(s) of a string wit JSON-encoded object(s). This data will be accessible in the script as data.
                        The script will also have access to all HTTP request parameters specified in the request using `params[:parameter_name]` syntax

                        This function differs from all other API functions by how it should be called. Instead of passing `run_script` as a value for the “method” parameter, is requires a special URL in the following form:  `http://{LIMS_ADDR}:{LIMS_PORT}/api/run_script`

                        [Example](http://demo.lims247.com/api/run_Script)

## search_subjects
```ruby
#Example #13 - This API will search for a Subject by variables such as Subject Type, Search Mode, Use r Fields, and texts strings.
#add any of the optional control parameters by copy and pasting <, :function=>'value'> after <:method =>''>:
#, :start=>'value': specifies what record to start listing from
#, :limit=>'value': limit number of records to retrieve
#, :sort=>'value': sort the records by a specific value
#, :dir=>'ASC/DESC': sort the records in ascending or descending order
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks)

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin',
                                     :method=>'search_subjects', :subject_type=>'Client', :search_mode=>'REGULAR',
                                     :user_fields=>'Address, Phone Number',
                                     :fields=>'Subject Name', :conditions=>'contains', :values=>'brooks'#Copy and paste optional control par ameters here

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
data.each do |key,value|
  if key == "Subjects"
    value[0].each do
      puts (k.to_s|k,v| +":"+v.to_s)
    end
  end
end
```
Retrieves a subject or list of specified subjects.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | Subjects |
**Required Parameters** | :search_mode=>’REGULAR/DEEP’ | Specifies the type of search to be performed.
                       |  :subject_type=>’value/text’ | Limits the search to all subjects of a specific type(can use subject type name or specific ID, Example: ‘Client’).
                       |  :user_fields=>’text,text’ | Comma delimited list of User-defined fields to retrieve for the searched subject(s)(Example: ‘Address, Phone Number’).
                       |  :fields=>’text’ | fields to search within the subject(Example: ‘Subject Name’)
                       | :conditions=>’text’ | The conditions that will be applied to the search subject. :values=>’value/text’ = Specific strings within the subject to be searched for.
**Optional Query Parameters** | None |
**Optional Control Parameters** | :start=>’value’ | The item specific ID to start the list with.
                                |  :limit=>'value' | The total number of results to retrieve.
                                |  :sort=>'text' | Sort the displayed results by a specific identifier such as subject_type, subject_name, etc.
                                | :dir=>'ASC/DESC' | Sort the displayed results in ascending or descending order.

## import_subjects via CSV
```ruby
#Example #14 - This API will import Subjects based on a CSV filerequire 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks) 8.

File.open("./clients.csv") do |the_csv|

  req = Net::HTTP::Post::Multipart.new url.path,
                                       :file=> UploadIO.new(the_csv, "text", "clients.csv"), :username=>'admin', :password=>'admin',
                                       :method=>'import_subjects', :subject_type=>'Client'

  res = Net::HTTP.start(url.host, url.port) do |http|
    http.request(req)
  end

  data = JSON.load(res.body)
  puts "Total Success" if data['success']
  puts data['message'] #success or error message will be printed here
end

```
Imports a subject or subjects along with specified fields from a CSV file.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | Subjects |
**Required Parameters** | :file=>’.CSV file’ | File to import(Must be CSV format).
                         |  :subject_type=>’value/text” | The specific type of the subject or subjects to be imported.
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

## import_subjects via JSON
Imports a subject or subjects along with specified fields specified by subjects in JSON format

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | None |
**Required Parameters** | :json=>’’ | String of LIMFINITY subjects in JSON format.
                        | :subject_type=>’value/text” | The specific type of the subject or subjects to be imported.
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

## upload_file_udf
```ruby
#Example #15 - This API will upload a file to a Subject as specified by ID
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://your-limfinity-url/api')#Copy and paste actual URL within the '' marks)

fname = 'C:\Users\Louis\Desktop\New Limfinity API Guide Examples\clients.csv'

File.open("#{fname}") do |f|

  req = Net::HTTP::Post::Multipart.new url.path,
                                       :username=>'admin', :password=>'admin', :method=>'upload_file_udf',
                                       :file=> UploadIO.new(f, "text", fname), :id=>'2', :udf_name=>'File'

  res = Net::HTTP.start(url.host, url.port) do |http|
    http.request(req)
  end

  str = res.body
  puts str
end
```
Uploads a file containing user-defined fields to the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | None |
**Required Parameters** | :udf_name=>’ text’ | Name of the file to be uploaded.
                        |:file=>’text’ | The content of the file to be uploaded.
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

## get_pedigree_data
```ruby
#Example #16 - This API will print the pedigree data of a Subject as specifed by ID
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'
​
url = URI.parse('http://demo.limfinity.com/api')#Copy and paste actual URL within the '' marks)
​
​
req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :password=>'admin', :method=>'get_pedigree_data',
                                     :subject_type=>'Patient', :subject_id=>'12345'
​
res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end
​
data = JSON.load(res.body)
​
data.each_pair do |key, value|
  if key.is_a?(Numeric) #has assigned patient
    patient = value[:Individual]
    father = value[:Father]
    if father
      father_id = father.is_a?(Subject) ? father.id : father
      father_data = data[father_id]
      puts father_data #output father data
    end
    mother = value[:Mother]
    if mother
      mother_id = mother.is_a?(Subject) ? mother.id : mother
      mother_data = data[mother_id]
      puts mother_data #output mother data
    end
  else #No assigned patient
    puts "No assigned patient"
  end
end
```
Retrieves the pedigree data of one family within the system.

Property | Parameter| Description
--------- | ----------- | -----------
**Returned Objects** | Key/value pair pf pedigree node subject and its information. |
**Required Parameters** |  :family_subject=>’value/text’ | Family subject type(can use subject type name or specific ID).
                        | :pedigree_prop=>’text’ | Pedigree type user-defined field or its name(not required if there is only one user-defined field for this type
**Optional Query Parameters** | None |
**Optional Control Parameters** | None |

### Returned Hash Values
he key is an individual ID of a pedigree node. The key is the numeric subject ID for the pedigree nodes with the assigned subject, or a string otherwise. The value is a hash with the following content(each pair is optional).

Property | Description
--------- | -----------
:Individual | Assigned subject.
:Mother | Subject if the mother node has an assigned subject; String ID otherwise.
:Father | Subject if the father node has an assigned subject; String ID otherwise.
:Gender | ‘Male’ or ‘Female’.
:Sampled, :MZTwin, :DZTwin, :Proband, :Deceased, :Consultand, :Carrier, :Affected | Same as in the standard PED format.
:terminated_pregnancy | “true” if the pregnancy was terminated.
:adopted_in | “true” if the individual was adopted in
:adopted_out | “true” if the individual was adopted out.

## Using auth Token
```ruby
#Example #17 - The purpose of this API is to test if an auth token generated by "Example 10 Gen Token.rb" works as intended if used instead of a password.
require 'rubygems'
require 'json'
require 'net/http'
require 'net/http/post/multipart'

url = URI.parse('http://demo.limfinity.com/api')#Copy and paste actual URL within the '' marks

req = Net::HTTP::Post::Multipart.new url.path,
                                     :username=>'admin', :auth_token=>'9bdc7e45-e541-4867-9077-6ff181876118', :method=>'users'#copy and paste generated auth token in <:auth_token=>''>

res = Net::HTTP.start(url.host, url.port) do |http|
  http.request(req)
end

data = JSON.load(res.body)
total = data['Total']
puts total
puts data
data.each do |key,value|
  if key == "Users"
    value.each do |types|
      puts "--------------"
      types.each do |k,v|
        puts (k.to_s + ": " + v.to_s)
      end
    end
  end
end
```
This example is not linked to a corresponding method. The purpose of this example is to demonstrate how to use an auth token returned by “Example 10 Gen Token.rb”.
<aside class="notice">
<b>Note:</b> Auth tokens are only valid for ten minutes after they have either been created or last used. If an expired or invalid token is used, the following error message should be displayed: “Invalid Token”.
</aside>


```ruby
suffix = subj.get_value('Program Identifier')
url= 'https://clinicaltrialsapi.cancer.*9'
```
## Calling External APIs with Limfinity
User can use call_external_service in After Script with optional :get, :post(default), and :put to specify the HTTP methods
