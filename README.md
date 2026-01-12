# Home Assistant Template for Growatt Solar & Battery system in Australia
Made by TechManPat 

# Growatt Home Assistant Template (Docker)

This repo is a TEMPLATE version of a Home Assistant config focused on Growatt energy tracking and bill estimation.

It is not a full clone of my Home Assistant instance. You must connect your own Growatt integration and update a few entity IDs.

## What you get
- Grid import energy (kWh) from your Growatt import power (W)
- House energy (kWh) from your Growatt local load power (W)
- Utility meters for daily + monthly import, split by tariff (Western Australia EV Tariffs form Synergy*)
- Template sensors for:
  - Grid cost today
  - House value today
  - Bill to date
  - Projected bill end of cycle

*If you want a different state & supplier tariffs you will need to edit the YAML yourself, use this as a guide, infact you can ZIP this up and upload into ChatGPT and ask it to update it to your own specific tariffs.   

## Before you copy anything
Do NOT copy `.storage/` from another system. It contains device registry and auth state.

## Step 1: Put files in your HA config
If you're running Home Assistant in Docker with a mapped config folder:

1. Download this repo (Code -> Download ZIP) and unzip it.
2. Copy these files into your HA config folder:
   - configuration.yaml
   - automations.yaml
   - scripts.yaml
   - scenes.yaml
3. Copy `secrets.example.yaml` to `secrets.yaml` (optional).

## Step 2: Update the 2 critical entity IDs
Open `configuration.yaml` and search for:

- `sensor.YOUR_GROWATT_GRID_IMPORT_POWER_W`
- `sensor.YOUR_GROWATT_LOCAL_LOAD_POWER_W`

Replace them with your actual Growatt entities.

Common places to find them:
- Settings -> Devices & Services -> your Growatt integration -> Entities
- Look for entities that end in `_power` and are measured in W.

## Step 3: Update your electricity rates
In `configuration.yaml` search for:
- `# CHANGE: Update these kWh rates`

Update the $/kWh values for each tariff to match your plan.

## Step 4: Create the required helpers
Create these helpers in Home Assistant:

1. Input number: `bill_days_elapsed`
   - min 0, max 60, step 1
2. Input number: `grid_cost_bill_to_date`
   - min 0, max 9999, step 0.01
3. Input number: `supply_charge_bill_to_date`
   - min 0, max 9999, step 0.01
4. Input number: `set_value`
   - min 0, max 9999, step 0.01

## Step 5: Restart Home Assistant
Restart Home Assistant after copying the files.

## Troubleshooting
- If entities show as `unknown`, confirm your 2 power sensor entity IDs are correct.
- If utility meters are missing, confirm `sensor.grid_import_energy` is created (it is derived from your import power).

