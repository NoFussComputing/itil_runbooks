---
title: New Service Catalog form
description: Playbook to detail the flow for adding a new Service Catalog form
date: 2024-02-05
template: project.html
software:
  name: glpi
itil_tags:
  - form
  - new
  - service_catalog
---

This runbook details how to create a new form using the GLPI plugin Form Creator. This setup of the form has been done in a way that will allow easily automating for the forms lifecycle.

On Creation of the form it will be usable as a service catalog form that any user with access can complete and submit. On submission A ticket is created from the form that is ready to be automated.


## Workflow

1. Within GLPI, navigate to `Administration -> Forms`

1. Click `+ Add` ad the top of the page

1. complete the following fields

    - `Name` - Name of the Form.

    - `Description` - This is the description for the form that will be seen in the service catalog.

    - `Icon` - The icon that will be displayed for the form within the service catalog.

    - `Icon Color` - The icon colour that will be displayed within the service catalog.

    - `Background Color` - The background colour of the item within the service catalog.

    - `Header` - This is the description field that is displayed when an end user is completing the form.

    - Click the yellow `+ Add` button in the bottom right hand corner to save the form.

1. Add the form questions 

    !!! tip
        Form questions can be placed in sections and just like the questions themselves, can be setup to be only displayed under certain circumstances.

    !!! info
        You can view the form by clicking on `Preview` in the left menu. **Don't forget to save the form first**

1. Navigate to `Target` in the left hand menu

1. Click the blue `+ Add Target` in the middle window

1. fill in the form

    - `Name: Form Ticket` - The name of the target

    - `Target: Target Ticket` - What type of ticket the form will create

1. Click on the new ticket target you just created

1. Complete the following fields as a minimum within each tab:

    !!! warning "Reminder"
        Don't forget to save before changing tabs so you don't loose the changes made.

    - `Target Ticket` Tab

        - `Ticket Title: [Form] <form name>, <item>`

        - `Description`

            > There are special requirements for setting up the ticket description. Please see below.

    - `Properties` Tab

        - `Destination Entity` - The entity where the ticket will be created

        - `Request Sources: formcreator` - Don't change this field.
        
        - `Request type` - What type of ticket. Generally this will be set to `request`

        - `ITIL Categories` - The ITIL category for the ticket.

            !!! info
                If the ITIL category has been setup with a ticket template, the fields from that ticket template will be added to the ticket on creation

    - `Actors` Tab

        Each of the fields in this tab are for assigning a user or group as either, `Requester`, `Watcher` or `Assigned To`. unless required leave as is and rely on ticket assignment rules.

        !!! info
            By Default we have setup the ticket to be assigned based off of the ITIL category then the assigned Item.

1. Test the form.

    !!! info
        We have an entity called `Development` that can be used by all `Entity Owners` as a playground. Temporally adjust the target entity to `Development` to test the forms creation.
    
        Don't forget to close the test ticket in the `development` entity and to reset the Ticket Target entity within the form.


1. confirm that the ticket created is setup correctly and that the JSON is valid

!!! warning "Reminder"
    Don't forget that when you have completed the form that it is set to `Active` So that end users can complete the form


## Ticket Description

Setup of the ticket description is important as currently the Form Creator plugin does not have the ability to post the form to a webhook. To work around this, we have devised a way of setting up the ticket description in a particular way that will enable the ticket to not only be a record of the form but the first step in the automation flow for that form.

Basic layout of the ticket description

``` text
##FULLFORM##

---

[
  JSON Document
]

```

As GLPI uses html within it's rich text form fields, It's very important when creating a new line from `---` and below that you use `Shift+Enter`. This method will not insert html paragraph tags `<p>`, only the new line character `\n`.

This layout has been chosen as JSON is easily parsable for automations. To parse the description above fist split the description by `---` and choose item `1` of the resulting array. You can safely use any whitespace trim function if required.

If you find html within the resulting json document that is not part of a json element value, go back and edit the form and ensure that the new lines done as described above.


### JSON Document Format

To ensure that the JSON is parsable, make sure that it is valid. The basic layout of the JSON document is as follows.

``` json
[
  {
    "<automation name>": {}
  }
]
```

This layout is a JSON List of dictionaries with only one item in the dictionary. A Dictionary with the name of the automation `<automation name>`. Adding the form fields is done by adding sub-dictionaries to the automation dictionary. for example:


#### Form Fields

To add the questions and answers to the document The following template contains all options.

``` json
{
  "<automation name>": {
    "<your_form_field_name>": {      // the question name in lower case and no spaces. use `_` for spaces.
      "answer": "##answer_24##",     // The form variables can be viewed from within the Ticket Target tab underneath the description.
      "optional": true,              // This field should only be used if the question is not mandatory.
      "type": "string",              // What type the answer is
      "values": "one,two,three"      // only use when type is choice
    }
  }
}

```

The mapping of JSON types to GLPI question types is as follows: 

- `choice` - GLPI type `Radios` when used, `values: "<value 1>,<value 2>,<etc>"` must be set. must be lower case

- `int` - GLPI Type `Integer` or `GLPI Object`

- `multi` - GLPI type multi-select field

- `string` - GLPI type text

- `text` - GLPI type text area


#### Form Approval

If the form requires approval, add an `approval` dictionary to the automation dictionary. If the `approval` dictionary is not included the automation should treat the form as not requiring approval.

``` json
{
  "<automation name>": {
    "approval": {
      "type": "entity", // entity,group,user
      "id": 0
    }
  }
}

```

Approval type are as follows:

- `entity` The `id` field must contain the profile ID of a profile within the entity that will approve the form

- `group` The `id` field must contain the ID of the group that will approve the form

- `user` The `id` field must contain the ID of the user who will approve the form.

!!! info
    We have a profile within all entities that contain only the entity owner. If the form requires approval from the entity owner use id `3` and type `entity` to select the entity owner.


#### Multi Automations

For tasks that require automations to be ran against multiple systems, this is possible by using the `systems` dictionary.

``` json
{
  "<automation name>": {
    "systems": {
      "<system name>": {
        "itil_category": 2,
        "task_category": 6
      }
    }
  }
}
```

`<system name>` can be any value that you wish as it is designed to be used by your automations. The only reserved keys in this dictionary are `itil_category`, `solution_text` and `task_category` which are used for the ticket creation process.

!!! info
    We use `glpi`, `ipam` and `notification` as the system names. `notification` is a "special" that is used to create a ticket for the purposes of notifying a user.


#### Reserved field names

- `solution_text`. used as a means to notify a user of information on closing a ticket. Use a `hidden` field question for this when creating the from with a default value of `-description-` and fill out the description field. This will tell the automation to look within the description field of form `question: <question ID>`

    ``` json
    "solution_text": {
      "answer": "-description-",
      "category": 0,              // The solution type ID
      "id": "<question id>"
    }
    ```

    Solution text can also be added to the `<system name>` dictionary


#### Full JSON Document Example

Below is an example of everything as explained above to demonstrate a JSON document with all options included.

``` JSON

[
  {
    "new_user": {
      "approval": {
        "type": "entity",
        "id": 0
      },
      "first_name": {
        "answer": "##answer_24##",
        "type": "text"
      },
      "last_name": {
        "answer": "##answer_25##",
        "type": "text"
      },
      "email": {
        "answer": "##answer_26##",
        "type": "text"
      },
      "account_type": {
        "answer": "##answer_27##",
        "type": "choice",
        "values": "public,internal"
      },
      "public_user": {
        "answer": "##answer_29##",
        "type": "boolean"
      },
      "solution_text": {
        "answer": "-description-",
        "category": 0,             // The solution type ID
        "id": "<question id>"
      },
      "systems": {
        "glpi": {
          "itil_category": 2,
          "task_category": 6
        },
        "ipam": {
          "itil_category": 3,
          "task_category": 1
        },
        "notification": { // Special case we use
          "itil_category": 3,
          "task_category": 1,
          "solution_text": {
            "answer": "-description-",
            "category": 0,              // The solution type ID
            "id": "<question id>"
          }
        }
      }
    }
  }
]


```
