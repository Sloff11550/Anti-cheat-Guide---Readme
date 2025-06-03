## 🛡️ Case Study: Easy Anti-Cheat (EAC)

> **Anti-Cheat System:** Easy Anti-Cheat (EAC)
> **Developer/Publisher:** Epic Games
> **Kernel-Level:** Sometimes (depends on the game)
> **Uninstall Behavior:** Varies by game; may leave behind a persistent service even after game removal
> **Transparency Rating:** Medium – some documentation provided, but inconsistent uninstall behavior

### Key Points:

* **EAC is commonly used** in games like Apex Legends, Fortnite, Elden Ring, and Dead by Daylight.
* While some games run EAC only during gameplay (user-mode only), others may install it as a persistent **Windows service**.
* Some users report that EAC remains installed and set to autostart **even after** uninstalling the game that used it.

### How to Check If EAC Is Installed:

```powershell
Get-Service -Name EasyAntiCheat -ErrorAction SilentlyContinue
```

If it returns a service, EAC is still present on your system.

### How to Stop or Disable It:

```powershell
# Stop the service if running
Stop-Service -Name EasyAntiCheat -ErrorAction SilentlyContinue

# Prevent it from restarting on boot
Set-Service -Name EasyAntiCheat -StartupType Disabled -ErrorAction SilentlyContinue
```

### How to Fully Uninstall It (If Applicable):

Some games include an uninstaller named `EasyAntiCheat_Setup.exe` in the game folder.

Example uninstall command:

```powershell
Start-Process "C:\Program Files (x86)\EasyAntiCheat\EasyAntiCheat_Setup.exe" -ArgumentList "uninstall" -Wait
```

*(Adjust path as needed depending on game install location.)*

### Official Documentation:

* [Official EAC Uninstall Instructions (easy.ac)](https://www.easy.ac/en-US/support/articles/eac-windows-service)

> 📝 Not all games include the uninstaller. Always check your game’s folder, and use the service check command above to verify removal.

This case shows that even widely used, well-supported anti-cheat systems can suffer from inconsistent uninstall practices—and why it’s important to verify what’s left behind.
