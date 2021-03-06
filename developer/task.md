# Breakdown of a Task

You can add tasks via the mobile application or website - but in all cases a task is defined best using our standard JSON format. This is how you would add a task via our API.

The building blocks of the Task JSON are:

- The main task details such as name, time outs, regional information, how it gets processed and by who
- The way in which a task will get processed using our component library
-- Within the "processing_components" property one or more components may be defined 

Here is an example:

```javascript
{
      "task": {
        "name": "This is the main title of the task",
        "show_name": true, // Show or hide the task name? Options are: true, false - default true
        "details": "Additional details of the task - inserts images, video's, audio plus limited html tags.",
        "send_to_timeline": true // defaults to true, option false - updates project timline with the completed task
        "region_based": false, //allocate the task to Taskers in a geo-location
        "lat": 0,
        "lng": 0,
        "radius": 300,
        "task_ttl": 0, // How many minutes before this task expires and is removed, 0 means never.
        "user_ttl": 0, // If the task has been allocated to a Tasker, how many seconds do they have to process it before it times out and can be allocated to another Tasker, 0 means never time it out.
        "process_count": 0, // Sets number of times a task can be processed, 0 means unlimited
        "user_multi_process": true, // If true a single Tasker can complete the same task multiple times
        "points": 1, // The amount of points to assign to successful completion of this task
        "repeat": after_completion, // Options are: after_completion, daily, weekly, monthly, yearly
        "repeat_schedule": after_completion:0, // Options are: after_completion:seconds, daily, weekly:day1;day2.., monthly:day-number1;day-number2.., yearly:mm-dd1;mm-dd2..
        "components_attributes": [
          {
            "component_type": "audio_file",
            "label": "Record an interview now",
            "name": "interview",
            "required": "yes",
            "extra": {
              "instructions": "",
              "max_length": 120 // Max audio file length in seconds
              }
          },
          {
            "component_type": "tags_collection",
            "label": "Select tags",
            "name": "tags",
            "required": "yes",
            "extra": {
              "instructions": "",
              "values": [
                {"value": 1, "text": "Wow"},
                {"value": 2, "text": "Glitch"},
                {"value": 3, "text": "Exec report"},
                {"value": 4, "text": "Compliance issue"},
                {"value": 5, "text": "Call back"}
              ]
            }
          },
          {
            "component_type": "text_area",
            "label": "Tell us how you are feeling?",
            "name": "my_feelings",
            "required": "no",
            "extra": {
              "instructions": "",
              "character_limit": 140
            }
           }
          ]
        },
        "meta_data": [ // Optional - leave out if you do not need it
          {
            "label": "Any label you want to give",
            "value": "Any value"
          },
          {
            "label": "Any second label you want to give",
            "value": "Any second value"
          }
           }
          ]
}
```

This examples has three components defined in the processing_components property, namely:
- audio_file - task worker can record audio
- tags_collection - task worker can choose from a set list of tags
- text_area - task worker can type written text in a text field

This is just a simple example of how a Task is constructed in Taskio. 

You can read about [Task Processing Components here](/developer/components.md).

## Task processing logic

Whether a task is available for processing will depend on these factors, in this order:

1. If task_ttl has passed, then the task is no longer available
2. Is process_count still active? If process count was defined as integer X, then have we completed (number this task has been processed - X = 0)? Once we complete this number, the task is no longer available (remember setting process_count=0 means it never runs out)
3. Does user_multi_process = true? If not, a single Tasker can only process a task once


