# Anti-cheat-Guide---Readme
Understanding and how to uninstall kernel drivers.
🔒 Anti-Cheat Removal Toolkit — README (Quick Summary)

This guide is a summarized version of the full anti-cheat removal toolkit, designed for users who want to identify, stop, and disable known kernel-level anti-cheat systems.

🎯 What This Covers

Disabling/removing kernel-level anti-cheats like:

Riot Vanguard (vgk, vgc)

Easy Anti-Cheat (EasyAntiCheat)

BattlEye (BEService)

RICOCHET (atvi-brynhildr)

FACEIT Anti-Cheat (FACEIT)

XIGNCODE3 (xhunter1)

nProtect GameGuard (npggsvc)

⚡ Scripts Included

✅ Universal Kill Switch (Temporarily Stop All)

Stops all known anti-cheat services:

$services = @("vgk","vgc","EasyAntiCheat","BEService","atvi-brynhildr","FACEIT","xhunter1","npggsvc")
foreach ($svc in $services) { sc.exe stop $svc 2>$null }

🔍 Status Scanner (Check What’s Running)

Shows which services are running, installed, or not present:

foreach ($svc in $services) {
  $status = Get-Service -Name $svc -ErrorAction SilentlyContinue
  if ($status -and $status.Status -eq 'Running') {
    Write-Host "$svc is RUNNING" -ForegroundColor Red
  } elseif ($status) {
    Write-Host "$svc is installed but NOT running" -ForegroundColor Yellow
  } else {
    Write-Host "$svc is NOT installed" -ForegroundColor Green
  }
}

🔒 Disable on Boot

Prevents all known anti-cheat services from starting automatically:

foreach ($svc in $services) {
  Set-Service -Name $svc -StartupType Disabled -ErrorAction SilentlyContinue
}

✅ Additional Notes

These scripts must be run as Administrator

Games may re-enable services automatically on launch

Use the full guide for manual removal (registry, file deletion, etc.) if needed

📂 Where to Get the Full Guide

Ask the person who shared this README with you for the full “Anti Cheat Removal Guide” if you want full uninstall instructions, paths, registry keys, and GUI tool recommendations.

Ask the person who shared this README with you for the full “Anti Cheat Removal Guide” if you want full uninstall instructions, paths, registry keys, and GUI tool recommendations.

