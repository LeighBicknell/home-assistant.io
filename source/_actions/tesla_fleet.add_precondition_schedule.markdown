---
title: "Add precondition schedule"
action: tesla_fleet.add_precondition_schedule
domain: tesla_fleet
description: "Adds or modifies a precondition schedule on a vehicle."
related_actions:
  - tesla_fleet.get_schedules
  - tesla_fleet.remove_precondition_schedule
  - tesla_fleet.add_charge_schedule
---

The **Add precondition schedule** action adds a location-based schedule to your Tesla vehicle that warms or cools the cabin so it's ready at a set time, or modifies an existing one.

Use it to have the car ready for your commute, for example at 07:45 on weekdays.

{% include actions/ui_header.md %}

To add a precondition schedule from an automation or a script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're setting up a new automation, add a trigger in the **When** section. Scripts don't need a trigger. They run when something else calls them.
4. In the **Then do** section, select **Add action**.
5. From the search box, search for and select **Tesla Fleet: Add precondition schedule**.
6. Select the **Vehicle** to add the schedule to.
7. Select the **Days of week** the schedule runs on.
8. Set the **Precondition time**.
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
Precondition time:
  description: The time the cabin should be ready.
One time:
  description: Run the schedule only once, then disable it.
Schedule ID:
  description: The ID of an existing schedule to modify. Leave empty to add a new schedule.
Name:
  description: The name of the schedule.
{% endoptions_ui %}

{% include actions/yaml_header.md %}

In YAML, refer to this action as `tesla_fleet.add_precondition_schedule`. A basic example looks like this:

{% example %}
action: |
  action: tesla_fleet.add_precondition_schedule
  data:
    device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
    days_of_week:
      - monday
      - tuesday
      - wednesday
      - thursday
      - friday
    enable: true
    precondition_time: "07:45:00"
    name: Commute
{% endexample %}

This has the cabin ready at 07:45 on weekdays.

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
precondition_time:
  description: >
    The time the cabin should be ready, in `HH:MM` format.
  required: true
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

- The vehicle is woken up if needed, and the **Vehicle Commands** scope is required.
- Use [Get schedules](/actions/tesla_fleet.get_schedules/) to find the ID of an existing schedule.

{% include actions/try_it.md %}

{% include actions/stuck.md %}

{% include actions/related.md %}
