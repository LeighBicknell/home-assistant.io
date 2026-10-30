---
title: "Add charge schedule"
action: tesla_fleet.add_charge_schedule
domain: tesla_fleet
description: "Adds or modifies a charge schedule on a vehicle."
related_actions:
  - tesla_fleet.get_schedules
  - tesla_fleet.remove_charge_schedule
  - tesla_fleet.add_precondition_schedule
---

The **Add charge schedule** action adds a location-based charge schedule to your Tesla vehicle, or modifies an existing one. The schedule applies when the vehicle is plugged in at the schedule location.

Use it to charge during cheaper overnight rates, for example from 23:30 until 05:30 on weeknights.

{% include actions/ui_header.md %}

To add a charge schedule from an automation or a script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're setting up a new automation, add a trigger in the **When** section. Scripts don't need a trigger. They run when something else calls them.
4. In the **Then do** section, select **Add action**.
5. From the search box, search for and select **Tesla Fleet: Add charge schedule**.
6. Select the **Vehicle** to add the schedule to.
7. Select the **Days of week** the schedule runs on.
8. Set a **Start time**, an **End time**, or both.
9. Select **Save**.

### Options in the UI

{% options_ui %}
Vehicle:
  description: The vehicle to add the schedule to.
Days of week:
  description: The days the schedule runs on.
Enable:
  description: Whether the schedule is active.
Location:
  description: The location where the schedule applies. Defaults to your Home Assistant location.
Start time:
  description: The time to start charging. At least one of start time or end time is required.
End time:
  description: The time to stop charging. At least one of start time or end time is required.
One time:
  description: Run the schedule only once, then disable it.
Schedule ID:
  description: The ID of an existing schedule to modify. Leave empty to add a new schedule.
Name:
  description: The name of the schedule.
{% endoptions_ui %}

{% include actions/yaml_header.md %}

In YAML, refer to this action as `tesla_fleet.add_charge_schedule`. A basic example looks like this:

{% example %}
action: |
  action: tesla_fleet.add_charge_schedule
  data:
    device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
    days_of_week:
      - monday
      - tuesday
      - wednesday
      - thursday
      - friday
    enable: true
    start_time: "23:30:00"
    end_time: "05:30:00"
    name: Off-peak
{% endexample %}

This charges between 23:30 and 05:30 on weeknights when the vehicle is plugged in at home.

### Options in YAML

{% options_yaml %}
device_id:
  description: >
    The ID of the vehicle to add the schedule to.
  required: true
  type: string
days_of_week:
  description: >
    The days the schedule runs on. Use the lowercase day names, such as `monday` or `saturday`.
  required: true
  type: list
enable:
  description: >
    Whether the schedule is active.
  required: true
  type: boolean
location:
  description: >
    The location where the schedule applies, as a mapping with `latitude` and `longitude`. Defaults to your Home Assistant location.
  required: false
  type: map
start_time:
  description: >
    The time to start charging, in `HH:MM` format. At least one of `start_time` or `end_time` is required.
  required: false
  type: string
end_time:
  description: >
    The time to stop charging, in `HH:MM` format. At least one of `start_time` or `end_time` is required.
  required: false
  type: string
one_time:
  description: >
    Run the schedule only once, then disable it.
  required: false
  type: boolean
id:
  description: >
    The ID of an existing schedule to modify. Leave it out to add a new schedule.
  required: false
  type: integer
name:
  description: >
    The name of the schedule.
  required: false
  type: string
{% endoptions_yaml %}

## Good to know

- The vehicle is woken up if needed. The **Vehicle Commands** or **Vehicle Charging Management** scope is required.
- Use [Get schedules](/actions/tesla_fleet.get_schedules/) to find the ID of an existing schedule.

{% include actions/try_it.md %}

{% include actions/stuck.md %}

{% include actions/related.md %}
