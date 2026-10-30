---
title: "Get schedules"
action: tesla_fleet.get_schedules
domain: tesla_fleet
description: "Returns the charge and precondition schedules configured on a vehicle."
related_actions:
  - tesla_fleet.remove_charge_schedule
  - tesla_fleet.remove_precondition_schedule
---

The **Get schedules** action returns the charge schedules and precondition schedules that are configured on your Tesla vehicle, including their schedule IDs. Use the IDs with [Remove charge schedule](/actions/tesla_fleet.remove_charge_schedule/) or [Remove precondition schedule](/actions/tesla_fleet.remove_precondition_schedule/), or to modify a schedule with the add actions.

{% include actions/ui_header.md %}

To get the schedules from an automation or a script:

1. Go to {% my automations title="**Settings** > **Automations & scenes**" %}.
2. Open an existing automation or script, or select **Create automation** > **Create new automation**.
3. If you're setting up a new automation, add a trigger in the **When** section. Scripts don't need a trigger. They run when something else calls them.
4. In the **Then do** section, select **Add action**.
5. From the search box, search for and select **Tesla Fleet: Get schedules**.
6. Select the **Vehicle** to get the schedules from.
7. Select **Save**.

### Options in the UI

{% options_ui %}
Vehicle:
  description: The vehicle to get the schedules from.
{% endoptions_ui %}

{% include actions/yaml_header.md %}

In YAML, refer to this action as `tesla_fleet.get_schedules`. A basic example looks like this:

{% example %}
action: |
  action: tesla_fleet.get_schedules
  data:
    device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
  response_variable: schedules
{% endexample %}

This stores the vehicle's schedules in the `schedules` response variable.

### Options in YAML

{% options_yaml %}
device_id:
  description: >
    The ID of the vehicle to get the schedules from.
  required: true
  type: string
{% endoptions_yaml %}

## Response data

The response data is a mapping with the following fields:

- `charge_schedules`: A list of charge schedules. Each schedule includes its `id`, `name`, `enabled`, `days_of_week`, `start_enabled`, `start_time`, `end_enabled`, `end_time`, `one_time`, `latitude`, and `longitude`.
- `precondition_schedules`: A list of precondition schedules. Each schedule includes its `id`, `name`, `enabled`, `days_of_week`, `precondition_time`, `one_time`, `latitude`, and `longitude`.

Times are in minutes after midnight, and `days_of_week` is a bitmask where Sunday is `1`, Monday is `2`, and so on up to Saturday, which is `64`.

## Good to know

- The vehicle is woken up if needed. No command scope is required.

{% include actions/try_it.md %}

{% include actions/more_examples.md %}

### Automation: remove all charge schedules when smart charging is turned off

Smart charging services, such as an energy supplier's app, can leave charge schedules on the vehicle after they stop managing it. A leftover schedule stops the vehicle from charging outside the scheduled times. This automation removes every charge schedule when a smart charging switch is turned off.

- **Trigger**: State: the smart charging switch turns off
- **Actions**: Get schedules, then Remove charge schedule for each returned schedule ID

{% details "YAML example for removing all charge schedules" %}

{% example %}
automation: |
  alias: "Remove charge schedules when smart charging is off"
  triggers:
    - trigger: state
      entity_id: switch.smart_charging
      to: "off"
  actions:
    - action: tesla_fleet.get_schedules
      data:
        device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
      response_variable: schedules
    - repeat:
        for_each: "{{ schedules.charge_schedules | map(attribute='id') | list }}"
        sequence:
          - action: tesla_fleet.remove_charge_schedule
            data:
              device_id: 0d462c0c4c0b064b1a91cdbd1ffcbd31
              id: "{{ repeat.item }}"
{% endexample %}

{% enddetails %}

{% include actions/stuck.md %}

{% include actions/related.md %}
