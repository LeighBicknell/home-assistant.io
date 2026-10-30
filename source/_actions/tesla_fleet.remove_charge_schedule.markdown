---
title: "Remove charge schedule"
action: tesla_fleet.remove_charge_schedule
domain: tesla_fleet
description: "Removes a charge schedule from a vehicle."
related_actions:
  - tesla_fleet.get_schedules
  - tesla_fleet.add_charge_schedule
  - tesla_fleet.remove_precondition_schedule
---

The **Remove charge schedule** action deletes a charge schedule from your Tesla vehicle. You identify the schedule to remove by its schedule ID, which you can find with [Get schedules](/actions/tesla_fleet.get_schedules/).

Use it to clean up schedules you no longer need, for example a schedule that a smart charging service left behind.

{% include actions/ui_header.md %}

To remove a charge schedule from an automation or a script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're setting up a new automation, add a trigger in the **When** section. Scripts don't need a trigger. They run when something else calls them.
4. In the **Then do** section, select **Add action**.
5. From the search box, search for and select **Tesla Fleet: Remove charge schedule**.
6. Select the **Vehicle**.
7. Enter the **Schedule ID** to remove.
8. Select **Save**.

### Options in the UI

{% options_ui %}
Vehicle:
  description: The vehicle to remove the schedule from.
Schedule ID:
  description: The ID of the schedule to remove, as returned by the Get schedules action.
{% endoptions_ui %}

{% include actions/yaml_header.md %}

In YAML, refer to this action as `tesla_fleet.remove_charge_schedule`. A basic example looks like this:

{% example %}
action: |
  action: tesla_fleet.remove_charge_schedule
  data:
    device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
    id: 1
{% endexample %}

This removes the charge schedule with ID `1`.

### Options in YAML

{% options_yaml %}
device_id:
  description: >
    The ID of the vehicle to remove the schedule from.
  required: true
  type: string
id:
  description: >
    The ID of the schedule to remove.
  required: true
  type: integer
{% endoptions_yaml %}

## Good to know

- The vehicle is woken up if needed. The **Vehicle Commands** or **Vehicle Charging Management** scope is required.

{% include actions/try_it.md %}

{% include actions/stuck.md %}

{% include actions/related.md %}
