# Power Flow Card Plus Setup

This section configures **Power Flow Card Plus** using the helpers you created earlier.

This gives you a clean, real time energy flow view showing:

- Grid import and export
- Solar production
- Battery charge and discharge
- Home usage
- Remaining battery runtime

---

# Where to configure

Home Assistant:

Dashboard → Edit → Add Card → Power Flow Card Plus

---

# Grid Configuration

Enable:

Separated Entities

Set:

Import power → your grid import sensor  
Export power → your grid export sensor

Example:

```
sensor.grid_import_power
sensor.grid_export_power
```

Use your actual entity names.

---

# Solar Configuration

Entity:

Solar Power Total (W)

This is the helper you created earlier.

Example:

```
sensor.solar_power_total_w
```

---

# Battery Configuration

Enable:

Separated Entities

Set:

Discharge power → Battery Discharge Power (W)  
Charge power → Battery Charging Power (W)

Example:

```
sensor.battery_discharge_power_w
sensor.battery_charging_power_w
```

---

# Home (Load) Configuration

Entity:

Local load power

Example:

```
sensor.local_load_power
```

This represents total house consumption.

---

# Battery Remaining Hours Tile

Add a second card to show runtime.

Dashboard → Add Card → Tile

Select:

Battery Hours Remaining

Example:

```
sensor.battery_hours_remaining
```

This displays:

Estimated hours left based on current discharge rate.

---

# Final Layout Recommendation

Power Flow Card Plus  
Battery Hours Remaining Tile

Place the runtime tile directly below the flow card for quick visibility.

---

# Result

You now have:

- Real time grid flow
- Total solar production
- Battery charge and discharge
- Home consumption
- Remaining battery hours

This gives a complete picture of your system at a glance.
