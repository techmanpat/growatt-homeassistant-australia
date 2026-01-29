Home Assistant on Windows with WSL2 + Docker (Persistent Setup Guide)

This setup keeps your Home Assistant config safe on C:\HomeAssistant so Docker or WSL resets cannot wipe it.

Part A, Install WSL2 properly
Step 1, Enable WSL2

Open PowerShell as Administrator:

wsl --install


Reboot when prompted.

Step 2, Confirm WSL2 is running
wsl -l -v


You should see your distro and Version 2.

If it shows Version 1:

wsl --set-default-version 2
wsl --set-version Ubuntu 2

Part B, Install Docker Desktop
Step 3, Install Docker Desktop

Download and install Docker Desktop.

During install:

Select Use WSL 2 instead of Hyper-V

Open Docker Desktop once after install.

Step 4, Enable WSL integration

Docker Desktop
Settings → Resources → WSL Integration

Enable:

Ubuntu

Click Apply and Restart.

Step 5, Set Docker disk location (optional but recommended)

Docker Desktop
Settings → Resources → Advanced

Recommended:

Disk image size: 200GB or more

Disk location: C: or D:

Part C, Create permanent Home Assistant folders
Step 6, Create folders
mkdir C:\HomeAssistant -Force
mkdir C:\HomeAssistant\config -Force
mkdir C:\HomeAssistant\backups -Force


This is where everything lives permanently.

Part D, Run Home Assistant with persistent storage
Step 7, Start container
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


Important line:

-v C:\HomeAssistant\config:/config


This prevents data loss.

Step 8, Confirm files are created
dir C:\HomeAssistant\config


You should see:

configuration.yaml

.storage

home-assistant.log

If yes, persistence is working.

Step 9, Test persistence
docker stop homeassistant
docker rm homeassistant


Run Step 7 again.

Open:

http://localhost:8123

You should return to your existing setup.

Part E, Backups
Step 10, Create backups

Inside Home Assistant:

Settings → System → Backups → Create Backup

Save backups to:

C:\HomeAssistant\backups

Part F, Prevent Docker storage issues
Step 11, Limit log growth

Already handled with:

--log-opt max-size=10m
--log-opt max-file=3

Step 12, Monthly cleanup (optional)
docker system df
docker system prune -a


Only run prune if you understand it removes unused images.

The rule you must not break

If /config is NOT mapped to:

C:\HomeAssistant\config

You can lose everything.

With the bind mount:

Reboots are safe

Restarts are safe

Docker updates are safe
