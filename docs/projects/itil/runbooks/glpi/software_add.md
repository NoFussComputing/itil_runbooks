---
title: New Software
description: Runbook to detail what to do to add a new Software to GLPI
date: 2024-02-09
template: itil_runbook.html
# area: Automation
---

GLPI provides as part of it's library, software so it can be tracked and audited. This runbook provides the steps required to add software to GLPI.


## Workflow

1. Login to GLPI

1. Navigate to `Assets -> Software`

1. Click the blue `+ Add` at the top of the screen

1. As a minimum fill in the following fields

    - `Name` - The name for the Service

    - `Associable to a Ticket: Yes`

    - `Publisher`

    - `Technician in Charge of the Hardware` or `Group in Charge of the Hardware` - This is the person or group who will be responsible for the item

        !!! danger "Requirement"
            **ALL** Items within the ITIL must have a responsible person.

        !!! info
            Within NFC select the `Site administrators` group as the Technician group in charge

    - `Groups` - The user group that use the service

    - `Sub-Entities` Select `Yes` for prime Service

        !!! warning
            Not selecting `yes` for will prevent non-prime sites from associating the software to a ticket and using it as part of their machine inventory.

1. Save the software

1. Navigate to `Software -> Links`

    1. Add a Manual link for the following:

        - Home Page

        - Github Repo

        - Documentation link.

        !!! tip
            Ensure that when adding links, that you choose the appropriate icon and select `open in new window: yes`
