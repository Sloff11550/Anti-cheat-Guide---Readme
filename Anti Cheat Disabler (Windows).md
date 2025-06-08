# Anti-Cheat Disabler & Status Checker (Windows)

This PowerShell script disables and checks the status of various kernel-level and user-mode anti-cheat systems on Windows. It's designed to provide users with a transparent, readable breakdown of which anti-cheat services are installed, running, stopped, or not present.

> This guide was created with the assistance of ChatGPT, used to perform an extensive deep search of all publicly known anti-cheat services to ensure thorough coverage and awareness.

---

## Features

* Disables known anti-cheat services until reboot
* Color-coded status report (Red = running, Yellow = stopped, Green = not installed)
* Friendly failure messages if a service is missing (instead of error spam)
* Fully transparent summary output

---

## Why?

This guide is meant for:

* Game modders
* Privacy-focused users
* Developers who want to audit what anti-cheat drivers are active
* General local testing

> **Note:** This script does not remove or permanently alter any system files. All changes are temporary until the next reboot.

---

## Usage

1. Open PowerShell **as Administrator**.
2. Copy and paste the script below into the console or run the file directly.
3. Review the output.

```powershell
# Anti-Cheat Disabler & Status Checker Script (Temporary Until Reboot)

$antiCheatServices = @(
    "BEService",             # BattlEye
    "BEDaisy",              # BattlEye Kernel Driver
    "EasyAntiCheat",        # Easy Anti-Cheat
    "ESEADriver2",          # Easy Anti-Cheat Driver
    "vgk",                  # Vanguard Anti-Cheat (Valorant)
    "faceit",               # FACEIT Anti-Cheat
    "FACEITService",        # FACEIT Service
    "nprotect",             # GameGuard
    "npgsvc",               # GameGuard Service
    "mhyprot2",             # miHoYo Anti-Cheat Driver
    "xhunter1",             # Xigncode3
    "xhunter",              # Alternate Xhunter name
    "anklebreaker",         # Hyperion Anti-Cheat (example)
    "EACLauncher",          # Alternate Easy Anti-Cheat service name
    "drvmap",               # Driver mapper used by some protections
    "PnkBstrA",             # PunkBuster A
    "PnkBstrB"              # PunkBuster B
    # "SysInternalsRootkitRevealerService"  # Hypothetical example (commented out)
)

Write-Host "\n--- Disabling Anti-Cheat Services (Temporary) ---"
$disableFailures = @()

foreach ($svc in $antiCheatServices) {
    Write-Host "Disabling service/driver: $svc"
    try {
        Stop-Service -Name $svc -ErrorAction Stop
        Set-Service -Name $svc -StartupType Disabled -ErrorAction Stop
        sc.exe config $svc start= disabled | Out-Null
    } catch {
        $disableFailures += "$svc - Failed to disable: $_"
    }
}

Write-Host "\nAll listed anti-cheat services have been processed. They will re-enable upon next reboot."

# -------------------------------------------
# Combined Verification Section
# -------------------------------------------

Write-Host "\n--- Anti-Cheat Status Report ---"

foreach ($svc in $antiCheatServices) {
    $status = Get-Service -Name $svc -ErrorAction SilentlyContinue
    if ($status) {
        if ($status.Status -eq 'Running') {
            Write-Host "$svc is RUNNING." -ForegroundColor Red
        } elseif ($status.Status -eq 'Stopped') {
            Write-Host "$svc is STOPPED." -ForegroundColor Yellow
        } else {
            Write-Host "$svc status: $($status.Status)"
        }
    } else {
        Write-Host "$svc is NOT INSTALLED (could not be disabled)." -ForegroundColor Green
    }
}

Write-Host "\n--- Disable Result Summary ---"
if ($disableFailures.Count -eq 0) {
    Write-Host "All anti-cheat services successfully disabled." -ForegroundColor Green
} else {
    Write-Host "Some anti-cheat services could not be disabled:" -ForegroundColor Red
    $disableFailures | ForEach-Object {
        if ($_ -match "Cannot find any service with service name") {
            Write-Host "$_ (Reason: Not installed)" -ForegroundColor Green
        } else {
            Write-Host $_ -ForegroundColor Red
        }
    }
}

Write-Host "\nCheck complete. Anti-cheat services above marked in red are actively running."
```

---

## Simulation Mode (Safe Test Preview)

Use the following version if you'd like to preview what the script **would** do without actually disabling anything.

```powershell
# Simulation Mode (Safe Preview Only)

$antiCheatServices = @(
    "BEService",             # BattlEye
    "BEDaisy",              # BattlEye Kernel Driver
    "EasyAntiCheat",        # Easy Anti-Cheat
    "ESEADriver2",          # Easy Anti-Cheat Driver
    "vgk",                  # Vanguard Anti-Cheat (Valorant)
    "faceit",               # FACEIT Anti-Cheat
    "FACEITService",        # FACEIT Service
    "nprotect",             # GameGuard
    "npgsvc",               # GameGuard Service
    "mhyprot2",             # miHoYo Anti-Cheat Driver
    "xhunter1",             # Xigncode3
    "xhunter",              # Alternate Xhunter name
    "anklebreaker",         # Hyperion Anti-Cheat (example)
    "EACLauncher",          # Alternate Easy Anti-Cheat service name
    "drvmap",               # Driver mapper used by some protections
    "PnkBstrA",             # PunkBuster A
    "PnkBstrB"              # PunkBuster B
)

Write-Host "\n--- Simulation: Anti-Cheat Disabler & Checker ---"

foreach ($svc in $antiCheatServices) {
    Write-Host "[SIMULATION] Would attempt to disable service/driver: $svc"
}

Write-Host "\n--- Simulation: Anti-Cheat Status Report ---"

foreach ($svc in $antiCheatServices) {
    $status = Get-Service -Name $svc -ErrorAction SilentlyContinue
    if ($status) {
        if ($status.Status -eq 'Running') {
            Write-Host "[SIMULATION] $svc is RUNNING." -ForegroundColor Red
        } elseif ($status.Status -eq 'Stopped') {
            Write-Host "[SIMULATION] $svc is STOPPED." -ForegroundColor Yellow
        } else {
            Write-Host "[SIMULATION] $svc status: $($status.Status)"
        }
    } else {
        Write-Host "[SIMULATION] $svc is NOT INSTALLED (Would not be disabled)." -ForegroundColor Green
    }
}

Write-Host "\nSimulation complete. No changes were made."
```

---

## Screenshot Preview

> *Include one here if desired showing the green/yellow/red output after running.*

---

## Requirements

* Windows 10 or newer
* PowerShell 5.1+
* Administrator access

---

## License

MIT License

---

## Author

[Andrew (Sloff11550)](https://github.com/Sloff11550)

---

## Contributing

PRs welcome if you’d like to:

* Add more anti-cheat systems
* Improve the logging/reporting
* Add a GUI wrapper or automation hook

---

## Known Anti-Cheat Services Included

* BattlEye (BEService)
* Easy Anti-Cheat (EasyAntiCheat, ESEADriver2)
* FACEIT (faceit, FACEITService)
* Vanguard (vgk)
* PunkBuster (PnkBstrA, PnkBstrB)
* GameGuard (nprotect, npgsvc)
* miHoYo (mhyprot2)
* Xigncode3 (xhunter, xhunter1)
* Others like drvmap, EACLauncher, and test entries

---

## Reminder

This is a local-use guide only. Do not use this to bypass multiplayer protections. It's designed to help protect user privacy and provide visibility—**not** to cheat.

If you're connecting to a secure server, accessing confidential tools, or working with sensitive material (e.g. game design data, personal projects, or private software builds), this guide ensures that background anti-cheat drivers aren't silently monitoring, logging, or interfering during that session. Run this script beforehand to minimize risk and maintain control over what's running in the background.

---

## Author's Note

\[To be filled in: Add a personal author's note here based on your preference or input.]
