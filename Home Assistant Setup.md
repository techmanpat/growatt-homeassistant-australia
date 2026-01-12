# Home Assistant on Windows 10 using Docker + WSL 2
Growatt focused setup with permanent config storage

This guide walks through setting up Home Assistant on Windows 10 using Docker Desktop with WSL 2, ensuring your configuration is permanently stored on `C:\`, then connecting a Growatt inverter after Home Assistant is running.

This setup uses **Home Assistant Container**, not Home Assistant OS.

---

## Requirements

- Windows 10 (version 2004 or newer recommended)
- Admin access on the PC
- Internet connection
- Access to Growatt Web portal server-au.growatt.com
- Growatt MIN / TLX Inverter Type

---

## Part 1: Enable WSL 2 on Windows 10

### 1. Check your Windows version
Press `Win + R`, type `winver`, press Enter.

---

### 2. Enable required Windows features
Open **PowerShell as Administrator** and run:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Restart your PC.

---

### 3. Install WSL 2 and Ubuntu

```powershell
wsl --install -d Ubuntu
```

Restart if prompted, then open **Ubuntu** from the Start menu and complete setup.

---

### 4. Set WSL 2 as default

```powershell
wsl --set-default-version 2
```

---

## Part 2: Install Docker Desktop

1. Install **Docker Desktop for Windows**
2. Open Docker Desktop → **Settings**
3. Enable **Use the WSL 2 based engine**
4. Enable **Ubuntu** under *Resources → WSL Integration*
5. Restart Docker Desktop

---

## Part 3: Create a permanent Home Assistant config folder

```powershell
mkdir C:\HomeAssistant\config
```

---

## Part 4: Run Home Assistant using Docker (Replace Your City, with well your city. 

```powershell
docker run -d `
  --name homeassistant `
  --restart unless-stopped `
  -p 8123:8123 `
  -e TZ="Australia/<YOUR CITY>" `
  -v C:\HomeAssistant\config:/config `
  homeassistant/home-assistant:stable
```

---

### Open Home Assistant

```
http://localhost:8123
```

---

## Part 5: Docker maintenance

Restart:
```powershell
docker restart homeassistant
```

Logs:
```powershell
docker logs -f homeassistant
```

---

## Part 6: Connect Growatt using the official API (Cloud integration)

This method connects Home Assistant to Growatt using an **API key generated from the Growatt web portal**.  
It is easier to set up than Modbus, but relies on Growatt’s cloud and internet connectivity.

---

### When to use this method

Use the Growatt API integration if:
- You do not have local LAN or Modbus access
- You want a quick setup with minimal networking work
- You are happy relying on Growatt’s cloud availability

Be aware:
- Data updates may be delayed
- If Growatt’s API changes or is offline, data may stop temporarily

---

### Step 1: Log in to the Growatt web portal

1. Open a browser and go to the Growatt web portal  
2. Log in with your Growatt account credentials  
3. Confirm your inverter and plant are visible and reporting data  

---

### Step 2: Generate an API key in the Growatt portal

1. In the Growatt portal, go to **Account settings**
2. Locate **API**, **Open API**, or **Developer access**
3. Generate a new API key
4. Copy and store the API key securely

You will need:
- Growatt username  
- Growatt plant ID  
- Generated API key  

---

### Step 3: Add the Growatt integration in Home Assistant

In Home Assistant:

1. Go to **Settings → Devices & Services**
2. Click **Add Integration**
3. Search for **Growatt**
4. Select the Growatt integration
5. Select either Username & Password or if you have a MIN/TLX Inverter select API Token (My Path)

6. Submit and wait for the integration to initialise

If successful, Home Assistant will automatically create Growatt sensor entities.

---

### Step 4: Verify Growatt entities

After setup, confirm entities such as:
- Solar production power
- Grid import or export power
- Load or consumption power
- Battery power (if applicable)
- Battery state of charge (SOC)

You can find these under:  
**Settings → Devices & Services → Growatt → Entities**

---

### Step 5: Use Growatt entities in dashboards and energy tracking

Once entities are available, you can:
- Add them to Lovelace dashboards
- Use them in automations
- Feed them into the Home Assistant Energy dashboard
- Create utility meters and long-term statistics

If using a shared configuration template:
- Replace placeholder entity IDs with your own Growatt entity names
- Confirm units are correct (W vs kW)

---

### Notes and limitations

- This integration depends on Growatt’s cloud API
- Data may lag compared to local Modbus
- API rate limits are controlled by Growatt
- Internet outages will affect data availability

For maximum reliability and real-time data, a local Modbus connection has generaly been recommended, however I found Growatts sensor data is unreliable via MODBUS, and this has approach has been rock solid. 

---

## Notes

- Always map config to `C:\HomeAssistant\config`
- Never commit `.storage` or `secrets.yaml`
- Treat this as a template, not a clone

---

## Summary

- Docker + WSL 2 = stable Windows setup
- Config stored permanently on C:\
- Growatt via API setup

