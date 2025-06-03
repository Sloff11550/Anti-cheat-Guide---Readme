## 🛑 Case Study: Delta Force (2025) – ACE Anti-Cheat Controversy

> **Anti-Cheat System:** Anti-Cheat Expert (ACE)
> **Developer/Publisher:** TiMi Studios / Tencent
> **Kernel-Level:** Yes
> **Uninstall Persistence:** Confirmed reports of the anti-cheat remaining active after game uninstall
> **Privacy Risk Level:** **High** due to development origin, kernel access, and lack of transparency

### Why This Matters:

* ACE runs with **Ring 0 privileges**, the highest level of access on your system.
* Many users have reported that **uninstalling the game does not remove ACE**, which may continue to operate in the background.
* This game has triggered **massive community backlash**, especially on Steam and Reddit.

### What Users Can Do:

Use the following **PowerShell or Command Prompt (as Administrator)** commands to remove ACE-related services from your system:

```powershell
sc delete ACE-GAME
sc delete ACE-BASE
sc delete "AntiCheatExpert Service"
sc delete "AntiCheatExpert Protection"
```

> ⚠️ **Warning:** These commands will **permanently remove** the listed services. Ensure no other installed games rely on them before proceeding.

To check for remaining ACE-related services, run:

```powershell
Get-Service | Where-Object { $_.Name -like "*ACE*" -or $_.DisplayName -like "*AntiCheatExpert*" }
```

### Additional Notes:

* Some users on the Steam forums have posted unofficial uninstall instructions or cleanup tools—always verify these before use.
* If an official uninstall method is discovered or documented by the developer, we will include it here in a future update.

This case exemplifies why transparency and user control are crucial when dealing with persistent kernel-level software.
