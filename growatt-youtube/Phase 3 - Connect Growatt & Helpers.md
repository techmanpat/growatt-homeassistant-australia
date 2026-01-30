# Home Assistant Helper Setup (Grwoatt Battery)
MIN Type Inverter

These template helpers combine multiple Growatt sensors into clean, usable totals for dashboards and Energy views.

Use this when you have:

- Multiple solar strings
- Multiple batteries
- Separate charge and discharge sensors

---

# Where to create helpers

Home Assistant:

Settings → Devices & Services → Helpers → Create Helper → Template → Sensor

Repeat for each helper below.

---

# Helper 1. Solar Power Total (W)

Combines all solar strings into one total wattage.

Name:

Solar Power Total (W)

State:

```
{{
  states('sensor.YOUR-DEVICE-ID_input_1_wattage') | float(0) +
  states('sensor.YOUR-DEVICE-ID_input_2_wattage') | float(0) +
  states('sensor.YOUR-DEVICE-ID_input_3_wattage') | float(0) +
  states('sensor.YOUR-DEVICE-ID_input_4_wattage') | float(0)
}}
```

Unit of measurement:

W

Leave everything else blank.

---

# Helper 2. Battery Charging Power (W)

Shows total charging watts across all batteries.

Name:

Battery Charging Power (W)

State:

```
{{
  states('sensor.YOUR-DEVICE-ID_battery_1_charging_w') | float(0) +
  states('sensor.YOUR-DEVICE-ID_battery_2_charging_w') | float(0)
}}
```

Unit of measurement:

W

Device class:

Power

State class:

Measurement

---

# Helper 3. Battery Discharge Power (W)

Shows total discharge watts across all batteries.

Name:

Battery Discharge Power (W)

State:

```
{{
  states('sensor.YOUR-DEVICE-ID_battery_1_discharging_w') | float(0) +
  states('sensor.YOUR-DEVICE-ID_battery_2_discharging_w') | float(0)
}}
```

Unit of measurement:

W

Device class:

Power

State class:

Measurement

---

# Helper 4. Battery Remaining (kWh)

Calculates usable energy remaining based on State of Charge.

Assumes:

- 25 kWh battery
- 90% usable capacity

Name:

Battery Remaining (kWh)

State:

```
{% set soc = states('sensor.YOUR-DEVICE-ID_state_of_charge_soc') | float(0) %}
{% set usable_kwh = 25 * 0.9 %}
{{ (soc/100 * usable_kwh) | round(1) }}
```

Unit of measurement:

kWh

Device class:

Energy

State class:

Total

---

# Helper 5. Battery Hours Remaining

Estimates runtime based on current discharge rate.

If discharge is very low, returns "unknown" to avoid nonsense values.

Name:

Battery Hours Remaining

State:

```
{% set discharge_w = states('sensor.battery_discharge_power_w') | float(0) %}
{% set remaining_kwh = states('sensor.battery_remaining_kwh') | float(0) %}

{% if discharge_w < 100 %}
  {{ 'unknown' }}
{% else %}
  {{ (remaining_kwh / (discharge_w / 1000)) | round(1) }}
{% endif %}
```

Unit of measurement:

h

Leave everything else blank.

---

# Final Notes

- Replace YOUR-DEVICE-ID with your actual Growatt SN / ID
- All helpers update live
- Safe for dashboards and Energy panel

---

# Result

You will now have:

- Total solar production
- Total charge power
- Total discharge power
- Remaining battery energy
- Estimated runtime

Much cleaner for dashboards and automations.
