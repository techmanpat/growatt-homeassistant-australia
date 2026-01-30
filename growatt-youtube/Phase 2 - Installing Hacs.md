# Installing HACS (Home Assistant Community Store) for Docker on Windows

This guide installs HACS safely for a Docker setup where your Home Assistant config lives at:

C:\HomeAssistant\config

This method:

- Works with Docker
- Survives reboots
- Survives container recreation
- Keeps everything persistent

---

## Step 1. Stop Home Assistant

Open PowerShell:

```powershell
docker stop homeassistant
```

---

## Step 2. Create required folders

```powershell
mkdir C:\HomeAssistant\config\custom_components -Force
mkdir C:\HomeAssistant\config\custom_components\hacs -Force
```

---

## Step 3. Download the latest HACS release

```powershell
Invoke-WebRequest `
  -Uri https://github.com/hacs/integration/releases/latest/download/hacs.zip `
  -OutFile C:\HomeAssistant\config\custom_components\hacs.zip
```

---

## Step 4. Extract HACS

```powershell
Expand-Archive `
  -Path C:\HomeAssistant\config\custom_components\hacs.zip `
  -DestinationPath C:\HomeAssistant\config\custom_components\hacs `
  -Force
```

---

## Step 5. Delete the ZIP

```powershell
Remove-Item C:\HomeAssistant\config\custom_components\hacs.zip
```

---

## Step 6. Verify the install

```powershell
dir C:\HomeAssistant\config\custom_components\hacs
```

You should see files like:

- manifest.json  
- __init__.py  
- other HACS files  

If you see these, install succeeded.

---

## Step 7. Start Home Assistant

```powershell
docker start homeassistant
```

Wait 1 to 2 minutes for it to fully boot.

---

## Step 8. Add HACS in the Home Assistant UI

Open:

http://localhost:8123

Navigate to:

Settings → Devices & Services → Add Integration

Search for:

HACS

Tick all boxes and continue.

Home Assistant will show a GitHub device code.

---

## Step 9. Authorise with GitHub

Open:

https://github.com/login/device

Enter the device code shown in Home Assistant.

Click Authorise.

---

## Step 10. Restart Home Assistant

Option A. Inside Home Assistant:

Settings → System → Restart → Restart Home Assistant

Option B. PowerShell:

```powershell
docker restart homeassistant
```

---

## Step 11. Confirm it worked

After restart:

- HACS should appear in the sidebar
- Integrations should load normally

If HACS does NOT appear:

```powershell
docker logs --tail 200 homeassistant
```

Check for errors and troubleshoot.

---

# Folder Structure After Install

```
C:\HomeAssistant
 └─ config
     └─ custom_components
         └─ hacs
```

If the hacs folder exists here, your install is persistent and safe.
