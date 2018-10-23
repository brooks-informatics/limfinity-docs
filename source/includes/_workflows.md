# Workflows and States
Workflows and states are the core of the Limfinity platform. Workflows dictate what types of objects can be manipulated, when they can be manipulated, and how. Workflows are the main engine in Limfinity and explain the individual processes which each object goes through.

## Starting Workflows
```ruby
# The start_workflow() method is used to start the workflow for a newly created subject
# param1 - Name of the workflow as defined in the system
# param2 - Subject for which the workflow should be started

patient = create_subject('Patient') do |p|
    p.set_value('Family', params['Family'])
end

start_workflow('Patient Workflow', patient)
```
When a subject is created, it needs to be placed into a workflow. The `start_workflow()` method is called to perform this. The `start_workflow()` method takes two parameters: the first parameter is the name of the workflow, and the second parameter is the subject for which the workflow should be started.

There are two parameters required for the `start_workflow()` method:

Property | Description
--------- | -----------
**Workflow** | Name of the workflow as defined in the system
**Subject** | Subject for which the workflow should be started

## Advancing Workflows
```ruby
# Iterate over a set of subjects and create a new subject with the same properties
# Place the subject into a 'Batched' state in the `Result` workflow
spikes.each do |spike|
    new_result = create_subject('Result') do |rz|
      rz.copy_properties_from(spike)
      rz.set_value('Result Type', 'SAMPLE SPIKE')
    end
    advance_workflow('Result', 'Batched', new_result)
end

```
When a workflow has been started, or when it is in progress, it may be necessary to set a subject or list of subjects to a specific state. This can be done in any after script or run script, and so it can be performed in tools, workflow transitions, and even quick links.

<aside class="warning">
Advancing workflows requires the correct subject type and workflow combination. A subject of a type not supported for a workflow can not be advanced to a state within that workflow.
</aside>

There are three parameters for the `advance_workflow()` method:


Property | Description
--------- | -----------
**Workflow** | The name of the Workflow in Limfinity
**Workflow State** | The name of the state to which the subject should be advanced, within the workflow
**Subject** | The subject object/loop index which is to be advanced.

## Acquiring All Subject States
```ruby
# Here we check to see if the subject has been in more than 5 states
# If the subject has been in more than 5 states, an error is raised

states_array = subj.states

if states_array.count > 5
    raise "This subject has been in too many states"
end
```
If it is ever necessary to acquire a list of the states that a subject has been in during its lifetime, the states method may be used. A simple `subj.states` call may be used on the current subject, and an array of the subject’s states will be returned.
This method is useful for quality control, but may also be used for checking if a subject has been in a specific state by using additional scripting.

## Determining the Current State of a Subject
```ruby
# Here we use the current_states method to check if the subject is in a specific state
if subject.current_states = "Pending Approval"
    raise "This subject is currently in the 'Pending Approval' state"
end
```
```ruby
# Here we advance the subject through a workflow, if its array of states includes 'HOLD'
note = params['Employee Notes']
subj.set_value('Employee Notes', note) if note.present?
samples = subj.get_value('Samples')
samples.each do |sample|
    advance_workflow('Sample', 'Received', sample)
end
advance_workflow('Sample Prep', 'MICRO', subj) if subj.current_states.map(&:name).include?('HOLD')
```
It is often necessary to determine the current state of a subject. In order to successfully obtain the current state of an object, the `current_states` method is used. This method may be used in a check or in conjunction with an array index.
