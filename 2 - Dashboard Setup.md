# HACS + Power Flow Card Plus + Mushroom Tiles
Home Assistant Container on Windows 10 (Docker + WSL 2)

This guide covers:
- Installing HACS on **Home Assistant Container**
- Installing **Power Flow Card Plus** (your power reporting dashboard card) for Growatt
- Installing **Mushroom** cards and using Mushroom tiles

These steps assume your Home Assistant config is mapped to:
`C:\HomeAssistant\config`

---

## Part 1: Install HACS (Home Assistant Community Store)

### 1. Confirm your Home Assistant config folder exists on C:\
You should already have:

`C:\HomeAssistant\config`

Inside it, you should see at least:
- `configuration.yaml`
- `.storage` folder

```powershell
mkdir C:\HomeAssistant\config\custom_components -Force
```

---

### 2. Create the HACS folder path
Create this folder:

`C:\HomeAssistant\config\custom_components\hacs`

If `custom_components` does not exist, create it.

```powershell
mkdir C:\HomeAssistant\config\custom_components\hacs -Force
```

---

### 3. Download the HACS release zip
1. Open the HACS download page and follow the download link for the **latest release**
2. Download the file called something like `hacs.zip`
3. Extract the contents of that zip

You want the extracted folder named:

`hacs`

---

### 4. Copy HACS into your Home Assistant config
Copy the extracted `hacs` folder into:

`C:\HomeAssistant\config\custom_components\hacs`

When you are done, you should have files that look like:

`C:\HomeAssistant\config\custom_components\hacs\manifest.json`

If you see `custom_components\hacs\hacs\manifest.json` you have one folder too deep. Fix it.

---

### 5. Restart Home Assistant (Docker container)
Open PowerShell and run:

```powershell
docker restart homeassistant
```

---

### 6. Add the HACS integration inside Home Assistant
1. In Home Assistant, go to:
   - **Settings → Devices & services**
2. Click:
   - **Add integration**
3. Search for:
   - **HACS**
4. Select HACS, tick the boxes, and submit
5. Follow the prompts, including GitHub device login/authorisation if asked

Important:
- If HACS does not appear in the Add Integration list, do a hard refresh:
  - Windows: `Ctrl + F5`
  - Mac: `Cmd + Shift + R`

---

### 7. Confirm HACS is installed
You should now see **HACS** in the left sidebar.

If you do not:
- Restart Home Assistant again
- Hard refresh the browser again

---

## Part 2: Install Mushroom (tiles and UI components)

### 1. Open HACS
1. Go to **HACS**
2. Go to **Frontend**

---

### 2. Search and install Mushroom
1. Search for:
   - `Mushroom`
2. Open it
3. Click **Download**
4. When prompted, refresh the browser

---

### 3. Confirm Mushroom is available in your dashboard
1. Go to a dashboard
2. Click the 3 dots (top right)
3. **Edit dashboard**
4. **Add card**
5. Search for `Mushroom`

You should now see Mushroom card types like:
- Mushroom Tile Card
- Mushroom Template Card
- Mushroom Entity Card
- Mushroom Chips Card

---

## Part 3: Install Power Flow Card Plus (power reporting card)

This is the common “power flow” style reporting card used for solar, battery, and grid flow dashboards.

### 1. Install via HACS
1. Go to **HACS → Frontend**
2. Search for:
   - `Power Flow Card Plus`
3. Open it
4. Click **Download**
5. Refresh the browser when prompted

---

### 2. Add the card to a dashboard
1. Go to your dashboard
2. 3 dots → **Edit dashboard**
3. Click **Add card**
4. Search for:
   - `Power Flow Card Plus`
5. Select it

If you want to use YAML mode, use **Manual card** and paste a config like this.

---

## Part 4: Example configs (copy/paste)

### Power Flow Card Plus example (placeholders)
Replace the `sensor.YOUR_*` placeholders with your actual Growatt entities.

```yaml
type: custom:power-flow-card-plus
title: Power Flow
entities:
  grid:
    entity: sensor.YOUR_GRID_POWER_W
  solar:
    entity: sensor.YOUR_SOLAR_POWER_W
  home:
    entity: sensor.YOUR_HOME_LOAD_POWER_W
  battery:
    entity: sensor.YOUR_BATTERY_POWER_W
    state_of_charge: sensor.YOUR_BATTERY_SOC_PERCENT
```

Notes:
- Some Growatt integrations expose **import/export** separately, others expose a single signed value.
- If your grid sensor is “import only”, the card will still work, but it may not show export correctly.
- If your battery power uses negative/positive direction, you may need to flip it depending on how your sensors report.

---

### Mushroom Tile examples

#### A simple entity tile
```yaml
type: custom:mushroom-entity-card
entity: sensor.YOUR_SOLAR_POWER_W
name: Solar Right Now
```

#### A “tile style” button for a switch
```yaml
type: custom:mushroom-entity-card
entity: switch.YOUR_DEVICE_SWITCH
name: Toggle Device
tap_action:
  action: toggle
```

#### A compact header row using Mushroom Chips
```yaml
type: custom:mushroom-chips-card
chips:
  - type: entity
    entity: sensor.YOUR_BATTERY_SOC_PERCENT
  - type: entity
    entity: sensor.YOUR_GRID_POWER_W
  - type: entity
    entity: sensor.YOUR_SOLAR_POWER_W
```

---

## Part 5: Common problems and fixes

### HACS does not show up
- Confirm folder path is correct:
  - `C:\HomeAssistant\config\custom_components\hacs\manifest.json`
- Restart Home Assistant:
  ```powershell
  docker restart homeassistant
  ```
- Hard refresh your browser:
  - Windows: `Ctrl + F5`
  - Mac: `Cmd + Shift + R`

### Cards installed but not showing
- Refresh the browser
- Restart Home Assistant
- Check **Settings → Dashboards → Resources**
  - HACS usually adds resources automatically, but this is where you confirm they exist

---

You can now head over to Phase 3, where you can use my custom configuration to get extra reporting, such as tariff calculations, savings, import costs etc. 



