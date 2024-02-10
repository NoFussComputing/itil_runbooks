---
title: Ticket Triage
description: Runbook to detail Ticket Triage within GLPI
date: 2024-02-10
template: itil_runbook.html
# area: Automation
---

This runbook is aimed at a user who will be triaging tickets within GLPI.

# Workflow

1. Log into GLPI

1. navigate to `Assistance -> Tickets`

1. For each ticket status, follow the applicable sub-workflow. order of Precedence is:

    - New

    - Pending

    - Processing (Planned)

    - Processing (Assigned)


### New Ticket Sub-Workflow

1. for each ticket follow this sub-workflow:

    !!! info
        We have mandated that no ticket will be valid unless it has a category. (done by making category mandatory in default ticket template). As such when the user selected the category they will have been provided a ticket template to use.

    !!! tip
        If the ticket is missing information or the template is incomplete, contact the requester using `Answer` (grey follow-up) and set the ticket to pending, using follow-up template `End User Action Required` reminder `3-days`. After three follow-ups you can solve the ticket ensuring within the solution text the user is notified for the reason of the closure. 

    1. Open the Triage task, set your name as the task assignee and start the timer.

        !!! info
            Timer is only available if you have the GLPI plugin ActualTime. We do.

    1. Ensure that any item within the ITIL is add to the ticket as an `associated item`.

    1. Confirm the ticket type, and follow the playbook. the link should be part of the ticket description.

        - Type `Request`

            1. confirm the request viability in accordance with Playbook

                - `Viable`

                    1. Click `Changes` in the ticket left hand tab

                    1. using the `middle mouse button` or `right mouse click -> open in new window` click the yellow button `Create Changes from this Ticket`

                    1. Ensure you have not been assigned to the Change ticket

                    1. Complete as many fields as you can in the right hand menu

                    1. In the previous tab, go back to the ticket page and solve the ticket using solution template `Change/Created`

                    1. Workflow complete.

                - `Not Viable`

                    1. Solve the ticket using the solution type `Denied` or `Invalid` as appropriate

        - Type `Incident`

            !!! info
                Generally the incident ticket will either have been created by an automation or a technician. So it should be good to go. However if it's not, ensure contact is made with the technician to correct this so the incident can be resolved. Use follow-ups so this contact is recorded.

            1. Click `Problems` in the ticket left hand tab

            1. using the `middle mouse button` or `right mouse click -> open in new window` click the yellow button `Create Problems from this Ticket`

            1. Ensure you have not been assigned to the Change ticket

            1. Complete as many fields as you can in the right hand menu

            1. In the previous tab, go back to the ticket page and solve the ticket using solution template `Change/Created`

            1. Workflow complete.


### Non-New Ticket Sub-Workflow

1. review any note added to the ticket since the last time someone took action

    1. if can be actioned, continue workflow `New Ticket` 1.3

    1. if can't be actioned, request further information if required, or do your own research.

        !!! tip
            Don't forget to use follow-ups/tasks for any action taken and ensure that when setting the ticket to pending from a follow-up, that the follow-up reminder is set. This way you'll receive E-mail reminders.
