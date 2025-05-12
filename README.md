# 🛡️ Anti-Cheat Removal & Awareness Toolkit

> 🤖 **Powered with help from ChatGPT** — This guide was built and reviewed using ChatGPT to ensure accuracy, clear formatting, and that all PowerShell scripts are clean, functional, and beginner-friendly. AI was used for proofreading, formatting, command validation, and optimizing the guide’s structure for clarity.

> ⚠️ **Disclaimer from the creator:** Don’t take my word for it. Always do your own research. Always fact check everything you read online—especially when it comes to system-level software. Knowledge is power, but only when it’s verified.
>
> 🔄 I will try to update this project regularly as new information or anti-cheat systems are discovered.

> A GitHub-hosted resource for disabling, removing, and understanding kernel-level anti-cheat software installed by modern games.

---

## 🎯 What This Is

Many popular games today install anti-cheat systems that run at **kernel level** (aka Ring 0)—the deepest layer of your operating system. These services:

* Often **do not uninstall** with the game
* May run **constantly in the background**
* Have **admin-level access** to your system

This project provides:

* 🔧 PowerShell scripts to detect, stop, and disable known anti-cheat systems
* 📖 Educational content explaining what kernel-level anti-cheat is and why it matters
* 💡 Guidance for everyday users and privacy-conscious gamers alike

---

## 🧰 What's Included

### `/scripts/`

* `stop_all_anti_cheats.ps1` — Stops all known anti-cheat services
* `disable_on_boot.ps1` — Prevents anti-cheats from restarting automatically
* `check_status.ps1` — Shows what’s installed, running, or inactive

### `/docs/`

* `Anti-Cheat_Removal_Guide.txt` — Full manual removal walkthrough (drivers, services, registry, etc.)
* `Anti-Cheat_Guide_README.txt` — Summary for casual users
* `What_Is_Kernel_Anti_Cheat.txt` — Plain-language deep dive on how these tools work

---

## ⚙️ Supported Anti-Cheat Systems

> 🧠 **Note on Easy Anti-Cheat (EAC):** Not all games using EAC install a persistent Windows service. Some only run EAC in user mode during gameplay. In these cases, the service `EasyAntiCheat` may not appear as installed when scanned, even though the anti-cheat is active during gameplay. This is actually preferred behavior, as the anti-cheat is not running when the game is closed.
>
> Similarly, **some other anti-cheat systems** (like BattlEye or GameGuard) can also be configured to only activate when the game launches. However, many are known to run persistently or load at boot, regardless of whether the game is open. This scanner is designed to detect only resident system-level services—not temporary user-mode instances.

This toolkit supports detection/removal for:

| Service Name     | Anti-Cheat        | Games (examples)           |
| ---------------- | ----------------- | -------------------------- |
| `vgk`, `vgc`     | Riot Vanguard     | Valorant                   |
| `EasyAntiCheat`  | Easy Anti-Cheat   | Apex Legends, Fortnite     |
| `BEService`      | BattlEye          | Rainbow Six, DayZ, PUBG    |
| `atvi-brynhildr` | RICOCHET          | Call of Duty (Warzone, MW) |
| `FACEIT`         | FACEIT Anti-Cheat | FACEIT CS\:GO client       |
| `xhunter1`       | XIGNCODE3         | Black Desert Online        |
| `npggsvc`        | GameGuard         | Various Korean MMOs        |

---

## 🚀 How to Use the Scripts

> 📜 **Important Transparency Note:**
> This project does not include any pre-made or downloadable PowerShell scripts. All script blocks provided here are "copy and paste ready" for manual use only.
>
> This is intentional. Users should always:
>
> * Review scripts before running them
> * Understand what they do
> * Modify them to suit their own system needs
>
> ⚠️ I strongly recommend verifying any script you run, even from this project. Blind trust in automation—even from helpful projects—is not good digital hygiene. This project is here to help you take control, not give it away.

> 🧠 **New to PowerShell? Here's how to run these scripts:**
>
> 1. Open **Notepad**.
> 2. Copy and paste one of the PowerShell script blocks below.
> 3. Save the file with a `.ps1` extension (for example: `stop_anti_cheats.ps1`).
> 4. Right-click the file and choose **Run with PowerShell**.
> 5. Or open PowerShell as Administrator, navigate to the script's folder, and type: `./stop_anti_cheats.ps1`
>
> ⚠️ If PowerShell gives a security warning or blocks the script, run this command first:
>
> ```powershell
> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
>
> This allows local scripts to run while still protecting against remote unsigned ones.

> **Run all scripts as Administrator** or they may fail silently.

### Stop All Known Anti-Cheat Services:

```powershell
$services = @("vgk","vgc","EasyAntiCheat","BEService","atvi-brynhildr","FACEIT","xhunter1","npggsvc")
foreach ($svc in $services) { sc.exe stop $svc 2>$null }
```

### Disable Them from Starting on Boot:

```powershell
foreach ($svc in $services) {
  Set-Service -Name $svc -StartupType Disabled -ErrorAction SilentlyContinue
}
```

### Check What’s Running Right Now:

```powershell
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
```

---

## 🧠 What Is Kernel-Level Anti-Cheat?

These tools run at the deepest level of your OS. They can:

* Monitor your entire system
* Access or scan memory
* Intercept inputs (keyboard, mouse, devices)
* Run at startup—even if the game is uninstalled

They are **more powerful than most antivirus software**, and often lack transparency or user control.

> Want a full breakdown? See [`docs/What_Is_Kernel_Anti_Cheat.txt`](./docs/What_Is_Kernel_Anti_Cheat.txt)

---

## 📉 Why This Project Exists

Most users are unaware that uninstalling a game doesn’t always uninstall the anti-cheat. These tools:

* Can remain on your system without notice
* May pose a security risk if vulnerable
* Often run even when not needed
* Are rarely explained to the end-user

This repo provides both awareness **and tools to take control.**

---

## 🧠 TL;DR (Non-Techie Version)

* These tools run deeper than antivirus and can see everything.
* They often don’t uninstall with the game.
* If they break or get hacked, they can mess up your entire PC.
* This toolkit helps you check for them, stop them, and clean them up.

> If a game installed a system service and didn’t tell you, would you be okay with that?

**Neither are we.**

---

## 📢 Want to Help?

* ⭐ Star the repo
* 🔄 Share it with your friends
* 🛠 Submit updates for new anti-cheat systems
* 🧾 Start a petition or discussion about uninstall transparency on Steam/Epic/etc.

---

## 👤 About the Creator

Hi, I’m Andrew — a longtime PC gamer who loves shooters, MMOs, and pretty much everything in between (except for a few genres I won’t name here 😄). I’ve spent years enjoying the best this platform has to offer.

I absolutely support the need for anti-cheat, especially in multiplayer. I can’t stand cheaters. If you want to cheat in single-player, fine. But if you’re ruining the game for everyone else in the room? That’s where I draw the line.

But what I don’t support is how far anti-cheat has gone. Kernel-level anti-cheat is invasive, risky, and often lacks transparency. It runs with higher permissions than most antivirus software, can remain after you uninstall the game, and puts your entire system at risk if it’s exploited—or simply mishandled. If these systems only monitored the game and shut down when it closed, I probably wouldn’t care. But they don’t.

I decided to create this project because somebody had to. This isn't necessarily about removing the software completely (unless you want to)—it’s about giving you control over what’s running on your machine. You should have the right to decide whether something stays active after you stop playing.

I can’t guarantee these methods won’t lead to a ban in some games—tampering always carries that risk. But I believe strongly that **no anti-cheat should be running 24/7** just because you installed a game. This guide gives you the option to control that.

> 💡 If you're not sure whether the service has stopped or disabled correctly, you can always restart your computer. If you're about to play a game that uses anti-cheat, a fresh reboot will typically allow the service to start up normally—no manual reactivation needed.

Please feel free to share this link with everyone you know, everywhere you can. The more people who understand this issue, the better.

---

## 🔗 External Resources

* [Easy Anti-Cheat FAQ](https://www.easy.ac/en-us/support/game/issues/anticheat/)
* [Riot Vanguard Technical Info](https://support-valorant.riotgames.com/hc/en-us/articles/360044648213)
* [Steam Uninstall Behavior](https://help.steampowered.com/)

---

**Game fair. But play safe.**

> Your FPS isn’t worth giving up your kernel. Play hard. Play to win. Just don’t cheat. And above all, have fun.
