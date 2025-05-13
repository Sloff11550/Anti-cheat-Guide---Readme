## 🎯 Case Study: BattlEye Anti-Cheat

> **Anti-Cheat System:** BattlEye (BEService)
> **Developer/Publisher:** BattlEye Innovations
> **Kernel-Level:** Yes (for many supported games)
> \*\*Uninstall Behavior: Varies by game; some include uninstallers, others require manual removal  \*\* Usually leaves behind services even after game removal
> \*\*Transparency Rating: Medium – official uninstallation methods exist, but clarity varies across games  \*\* Low – no universal uninstaller and little documentation for end users

### Key Points:

* BattlEye is used in major titles like **Rainbow Six Siege**, **PUBG**, **DayZ**, **ARK: Survival Evolved**, and **Battlefield**.
* It typically installs a Windows service called `BEService`, which **often remains even after uninstalling the game**.
* Some games do include an **uninstaller batch file** or EXE inside their install folders—such as `Uninstall_BattlEye.bat` or `Uninstall_BattlEye.exe`.

### How to Check If BattlEye Is Still Installed:

```powershell
Get-Service -Name BEService -ErrorAction SilentlyContinue
```

If this returns a service, BattlEye is still installed on your system.

### How to Stop and Disable It:

```powershell
# Stop BattlEye service if running
Stop-Service -Name BEService -ErrorAction SilentlyContinue

# Disable it from running at startup
Set-Service -Name BEService -StartupType Disabled -ErrorAction SilentlyContinue
```

### How to Remove It Manually:

```powershell
sc delete BEService
```

> ⚠️ This will permanently remove the BattlEye service. Ensure no installed games are still using it.

### Check for Uninstaller (Game Folder Specific):

Look for one of the following inside your game's install directory:

* `Uninstall_BattlEye.bat`
* `Uninstall_BattlEye.exe`
* Or navigate to a `BattlEye` subfolder within the game’s directory

> ✅ **If the uninstaller is present, it’s generally safe to run.** Be sure to check afterward using `Get-Service` to confirm full removal.

### 📄 Official Documentation:

* [BattlEye FAQ](https://www.battleye.com/support/faq/)
* [Microsoft: BattlEye Compatibility Info](https://support.microsoft.com/en-us/windows/older-versions-of-battleye-software-may-not-be-compatible-with-windows-10-version-1903-26ad3ec8-e5af-c0e2-ac70-df7c4a0aca4c)
  BattlEye has minimal public-facing documentation. Most uninstall methods are game-specific. If you are unsure, consult your game’s support portal.

---

This case highlights why even popular, widely-used anti-cheat systems can lack clear uninstall paths—making it vital for users to know how to check and remove services manually if desired.
