## 🎯 Case Study: Riot Vanguard Anti-Cheat

> **Anti-Cheat System:** Riot Vanguard
> **Developer/Publisher:** Riot Games
> **Kernel-Level:** Yes (Ring 0)
> **Uninstall Behavior:** Installed separately from the game; persists after game removal
> **Transparency Rating:** Medium – Riot provides official documentation, but concerns remain due to constant background operation

### 🔍 Key Notes:

* Riot Vanguard is required to play **Valorant**, and it installs a **kernel-level driver (vgk.sys)** and a background service (`vgk`, `vgc`) that starts with Windows.
* **It runs continuously**, even when the game is not running, unless manually stopped or disabled.
* Vanguard’s persistent nature has led to widespread privacy and security concerns within the community.

### 🔧 How to Check If It's Installed:

```powershell
Get-Service -Name vgk, vgc -ErrorAction SilentlyContinue
```

If this returns a result, Vanguard is present and active on your system.

### 🛑 How to Stop and Disable It:

```powershell
Stop-Service -Name vgk, vgc -ErrorAction SilentlyContinue
Set-Service -Name vgk -StartupType Disabled -ErrorAction SilentlyContinue
Set-Service -Name vgc -StartupType Disabled -ErrorAction SilentlyContinue
```

> 📌 Run PowerShell as Administrator for these commands to work properly.

### 🧹 How to Remove It Manually:

```powershell
sc delete vgk
sc delete vgc
```

> ⚠️ This will permanently remove the Vanguard kernel service and client. Valorant will no longer launch until it’s reinstalled.

### 📝 Official Documentation:

* Riot Support (Vanguard Overview): [https://support-valorant.riotgames.com/hc/en-us/articles/360044648213](https://support-valorant.riotgames.com/hc/en-us/articles/360044648213)

---

Riot Vanguard remains one of the most prominent examples of kernel-level anti-cheat software in the industry today. While Riot maintains that it’s safe and secure, the fact that it runs at startup and continues operating in the background has sparked debates about transparency and user control. This case serves as a key example of why this guide was created.
