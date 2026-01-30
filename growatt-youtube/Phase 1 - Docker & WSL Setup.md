# Home Assistant on Windows using WSL2 and Docker

Stable. Persistent. Zero config loss.

This setup runs Home Assistant in Docker while storing all data on your local drive so Docker, WSL, or Windows updates cannot wipe your system.

Your Home Assistant data lives permanently here:

C:\HomeAssistant\config

If that folder exists, you are safe.

---

## What this gives you

- Native Windows install
- Fast Docker performance with WSL2
- Persistent storage
- Safe reboots and updates
- Easy backups
- No Docker disk corruption issues

---

# Full Setup Guide

## Step 1. Install WSL2

Open PowerShell as Administrator:

```powershell
wsl --install
```

Reboot.

Verify:

```powershell
wsl -l -v
```

You should see Version 2.

If not:

```powershell
wsl --set-default-version 2
wsl --set-version Ubuntu 2
```

---

## Step 2. Install Docker Desktop

Install Docker Desktop from the official site.

During install:

- Use WSL 2 backend

After install:

Docker → Settings → Resources → WSL Integration  
Enable Ubuntu  
Apply and Restart

---

## Step 3. Create persistent folders

```powershell
mkdir C:\HomeAssistant -Force
mkdir C:\HomeAssistant\config -Force
mkdir C:\HomeAssistant\backups -Force
```

This is where everything lives permanently.

---

## Step 4. Run Home Assistant

```powershell
docker run -d `
  --name homeassistant `
  --restart unless-stopped `
  --privileged `
  -e TZ="Australia/Perth" `
  -p 8123:8123 `
  --log-opt max-size=10m `
  --log-opt max-file=3 `
  -v C:\HomeAssistant\config:/config `
  homeassistant/home-assistant:stable
```

Important line:

```
-v C:\HomeAssistant\config:/config
```

Without this, you will lose everything.

---

## Step 5. Confirm persistence

Wait 1 minute, then:

```powershell
dir C:\HomeAssistant\config
```

You should see:

- configuration.yaml
- .storage
- home-assistant.log

If yes, storage is working.

---

## (Optional) Step 6. Test safety

```powershell
docker stop homeassistant
docker rm homeassistant
```

Run the container again.

If your setup is still there, you're protected.

---

# Backups

Inside Home Assistant:

Settings → System → Backups → Create Backup

Save backups to:

C:\HomeAssistant\backups

Optional later:

- Cloud backup
- Scheduled copy job
- External drive

---

# Maintenance

## Limit log growth (already included)

```
--log-opt max-size=10m
--log-opt max-file=3
```

Prevents Docker logs filling the disk.

---

## Monthly cleanup (optional)

```powershell
docker system df
docker system prune -a
```

Only run prune if you understand it removes unused images.

---

# Folder Structure

```
C:\HomeAssistant
 ├─ config      ← Home Assistant data
 ├─ backups     ← backups
```

Everything important is inside config.

---

# Common Issues

## Home Assistant reset itself

You forgot the bind mount.

Recreate with:

```
-v C:\HomeAssistant\config:/config
```

## No space left on device

Increase Docker disk size or run:

```powershell
docker system prune -a
```

## Cannot access localhost

Check:

- Docker running
- Port 8123 free
- Firewall not blocking

---

# Golden Rule

If /config is mapped to C:\HomeAssistant\config, you are safe.

If not, you can lose everything.

Never skip this.
