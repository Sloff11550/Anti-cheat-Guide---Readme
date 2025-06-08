<#
Anti-Cheat Disabler & Status Checker (Windows)
Author: Andrew or Sloff11550
License: MIT

Description:
This script attempts to disable all known kernel/user-mode anti-cheat
services temporarily (until reboot), and reports service status clearly.
Ideal for testing, security auditing, or local privacy analysis.
#>

```powershell
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

Write-Host "\n=== Disabling Anti-Cheat Services (Temporary Until Reboot) ==="
$disableFailures = @()
$notInstalledInfo = @()

foreach ($svc in $antiCheatServices) {
    Write-Host "Disabling service/driver: $svc"
    $status = Get-Service -Name $svc -ErrorAction SilentlyContinue
    if ($status) {
        try {
            Stop-Service -Name $svc -ErrorAction Stop
            Set-Service -Name $svc -StartupType Disabled -ErrorAction Stop
            sc.exe config $svc start= disabled | Out-Null
        } catch {
            $disableFailures += "$svc - Failed to disable: $_"
        }
    } else {
        $notInstalledInfo += "$svc - Skipped: Not installed"
    }
}

Write-Host "\nAll listed anti-cheat services have been processed. They will re-enable upon next reboot."

# -------------------------------
# Anti-Cheat Service Status Report
# -------------------------------

Write-Host "\n=== Anti-Cheat Status Report ==="
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
        Write-Host "$svc is NOT INSTALLED." -ForegroundColor Green
    }
}

# -------------------------------
# Summary Output
# -------------------------------

Write-Host "\n=== Disable Result Summary ==="
if ($disableFailures.Count -eq 0 -and $notInstalledInfo.Count -eq 0) {
    Write-Host "All anti-cheat services successfully disabled." -ForegroundColor Green
} else {
    if ($disableFailures.Count -gt 0) {
        Write-Host "Some anti-cheat services could not be disabled:" -ForegroundColor Red
        $disableFailures | ForEach-Object { Write-Host $_ -ForegroundColor Red }
    }
    if ($notInstalledInfo.Count -gt 0) {
        Write-Host "The following services were not installed and were skipped:" -ForegroundColor Green
        $notInstalledInfo | ForEach-Object { Write-Host $_ -ForegroundColor Green }
    }
}

Write-Host "\nCheck complete. Anti-cheat services above marked in red are actively running."
```
