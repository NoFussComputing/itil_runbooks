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

