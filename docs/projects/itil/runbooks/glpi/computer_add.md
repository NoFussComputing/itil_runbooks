---
title: New Computer
description: Runbook to detail what to do to add a new new computer to GLPI
date: 2023-11-20
template: itil_runbook.html
area: Automation
---


## New host workflow

1. Login to GLPI

1. Navigate to Assets -> Computers

1. Click the blue `+ Add` at the top of the screen

1. As a minimum fill in the following fields

    - `Name` - The computers name

    - `Technician in Charge of the Hardware` or `Group in Charge of the Hardware` - This is the person or group who will be responsible for the item

        !!! danger "Requirement"
            **ALL** Items within the ITIL must have a responsible person.

    - `Serial Number` or `UUID` - This information allows the inventory agent to link your computer to an incoming inventory
