# Home Assistant Energy, Tariff & Financial Control YAML

This repository contains my **Home Assistant configuration and automation YAML files** used to manage **electricity tariffs, energy tracking, and financial controls** for a solar + battery setup (Growatt-based).

I am sharing these files so others can **reuse, adapt, and improve** them for their own setups, rather than starting from scratch.

---

## What’s in this repository

### 1. `configuration.yaml`
This file contains:
- Template sensors
- Utility meters
- Helpers
- Energy-related entities

These are used to:
- Track grid import and export
- Calculate costs based on tariff periods
- Aggregate solar and battery data
- Support dashboards and reporting

### 2. `automations.yaml`
This file contains:
- Time-based tariff switching automations
- Logic that sets the active tariff depending on time of day
- Automations that drive cost calculations and financial tracking

These automations allow Home Assistant to:
- Automatically apply the correct electricity rate
- Keep energy costs accurate without manual changes
- Support dashboards that show real-world cost impact

---

## Why I’ve shared this

Electricity pricing, especially with:
- Time-of-use tariffs
- EV charging windows
- Solar and battery systems

is hard to model correctly in Home Assistant.

By sharing my setup, you can:
- Use my tariff logic as a starting point
- Reuse my financial tracking structure
- Save time building your own system

This is **not plug-and-play**. It is intentionally shared as a **reference and foundation**.

---

## Important: Adapt this for your setup

### Use an AI to customise it
I strongly recommend you:
- Upload these YAML files into an AI tool like **ChatGPT**
- Ask it to:
  - Adapt the tariff times to your electricity plan
  - Rename helpers and sensors
  - Simplify or extend logic for your use case

This will save you hours and reduce mistakes.

---

## Critical changes you MUST make

### 1. Update Growatt device IDs
All Growatt sensors are device-specific.

You **must**:
- Replace every Growatt sensor entity ID
- Match them to the IDs shown in **your Home Assistant installation**

If you don’t do this, the sensors will not work.

---

## Where to install these files (Windows users)

If you are running Home Assistant on **Windows** (for example via Docker or supervised install):

1. Locate the Home Assistant folder on your **C drive**
2. Navigate to the directory where Home Assistant is installed
3. Copy and paste:
   - `configuration.yaml`
   - `automations.yaml`
4. Restart Home Assistant

Example location (will vary by setup):
```
C:\homeassistant\
```

If you are using Docker or WSL, place these files inside the Home Assistant config directory mapped to the container.

---

## Final notes

- This setup reflects **my tariff structure and financial goals**
- Treat it as a **template**, not a final product
- Always test changes carefully
- Back up your existing YAML before replacing anything

If this helps you, feel free to fork it, improve it, or share it forward.

Happy automating.

