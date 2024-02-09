---
title: New Appliance
description: Runbook to detail what to do to add a new Appliance to GLPI
date: 2024-02-09
template: itil_runbook.html
# area: Automation
---

An Appliance is generally a self-contained device that does one purpose. GLPI provides in its library these items so they are trackable. Within No Fuss Computing we use appliances to track services we have deployed. This runbook provides the steps required to add an appliance to GLPI.


## Workflow

1. Login to GLPI

1. Ensure you are within the same entity the appliance will be created in

1. Navigate to `Management -> Appliances`

1. Click the blue `+ Add` at the top of the screen

1. As a minimum fill in the following fields

    - `Name` - The name for the Service

    - `Associable to a Ticket: Yes`

    - `Appliance types`

        - `Web Application` - An application that is accessed via a web browser

    - `Technician in Charge of the Hardware` or `Group in Charge of the Hardware` - This is the person or group who will be responsible for the item

        !!! danger "Requirement"
            **ALL** Items within the ITIL must have a responsible person.

        !!! info
            We generally select the Site administrators group for prime services

    - `Groups` - The users group that use the service

    - `Sub-Entities` Select `Yes` for prime Service

        !!! warning
            Not selecting `yes` for a prime service will prevent non-prime sites from associating the appliance to a ticket

1. Save the appliance

1. Navigate to `Appliance -> Items`

    1. Add the software to the appliance

    1. If deployed to a cluster, add the Cluster.
