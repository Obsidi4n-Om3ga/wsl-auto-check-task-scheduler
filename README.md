# wsl-auto-check-task-scheduler
🔄 Automate WSL health checks and reboot after Windows updates using PowerShell and Task Scheduler. Runs at startup with SYSTEM privileges to prevent WSL timeout errors.
# 🛠️ WSL Health Check & Auto-Reboot via Task Scheduler

This project automates checking the status of Windows Subsystem for Linux (WSL) after a system update. If WSL fails to initialize properly, a PowerShell script triggers a system reboot. Task Scheduler ensures this script runs automatically at startup with elevated privileges.

---

## ✅ Features

- Checks WSL availability at system startup
- Automatically reboots if WSL is unresponsive
- Runs silently in the background with SYSTEM privileges
- Uses Task Scheduler for reliable startup execution
- Includes PowerShell-based fallback task registration

---

## 🧩 Components

### 📜 `Check-WSL-PostUpdate-AutoReboot.ps1`

- Located in `/scripts`
- Script logic to:
  - Check WSL status
  - Log events (optional enhancement)
  - Trigger reboot if needed

### ⚙️ `Register-WSLCheckTask.ps1`

- Located in `/task-scheduler`
- PowerShell script to register the task manually
- Useful if Task Scheduler UI fails or to include in deployment scripts

---

## 📌 Manual Setup Steps (UI-Based)

1. **Open Task Scheduler as Administrator**
2. **Create a New Task** (not Basic Task):
   - Name: `Check WSL After Update`
   - Run with highest privileges
   - User: `SYSTEM`
   - Configure for: `Windows 11`
3. **Add Trigger**:
   - At startup *(or on logon/on schedule)*
4. **Add Action**:
   - Program/script: `powershell.exe`
   - Arguments: `-ExecutionPolicy Bypass -File "C:\Scripts\Check-WSL-PostUpdate-AutoReboot.ps1"`
5. **Conditions/Settings**:
   - Disable "Start the task only if the computer is on AC power"
   - Enable "Restart if the task fails"

---

## 🧪 PowerShell Fallback (Script Registration)

```powershell
$Action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument '-ExecutionPolicy Bypass -File "C:\Scripts\Check-WSL-PostUpdate-AutoReboot.ps1"'
$Trigger = New-ScheduledTaskTrigger -AtStartup
Register-ScheduledTask -TaskName "Check WSL After Update" -Action $Action -Trigger $Trigger -RunLevel Highest -User "SYSTEM"
🎯 Final Result

    ✅ WSL launches reliably after updates

    ✅ Script auto-runs at startup

    ✅ No more startup timeout errors for WSL


🙌 Contributing

Pull requests and improvements welcome!
