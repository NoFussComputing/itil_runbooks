---
title: Add Exectution or Hop node to AWX
description: Runbook to detail what to do to add exectution or hop node to AWX
date: 2024-03-14
template: itil_runbook.html
---

This runbook details what to do to add exectution or hop node to AWX.


## Assumptions

- You have Access within AWX to add instances.


## Workflow

1. Navigate to `Administration -> Instances`

1. Click Add

1. Complete the following fields

    - `Name` _preferable to use the site name the node is to be installed in_

    - `Description` _Optional. give it a good description_

    - `Instance Type`

        - `execution` _Select this type if you wish to run jobs on the node._

        - `hop` _Select this type if its expected that other hop or execution nodes will be connecting to it_

    - `Peers from control nodes` _Only Select this if the control node has network connectivity to the node to be created_

    - `Managed by Policy` _Only Applicable to execution node_

        - `Checked` AWX can auto-magically assign jobs to the execution node

        - `Un-Checked` AWX can't auto-magically assign jobs to this node

    !!! info
        For a node to be installed on a remote site where AWX does not have network connectivity too, `Peers from control nodes` and Managed by Policy must be `false`/`un-checked`

1. click `save`.

1. Click on the `Peers` tab

1. click `associate`

1. click the check-box next to the peer that is accessible to the node to be installed. _This will generally be a DNS name_

1. click `save`.
