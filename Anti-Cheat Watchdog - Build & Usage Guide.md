# 🛡️ Anti-Cheat Watchdog - Build & Usage Guide

A lightweight Windows system tray app that monitors known anti-cheat services and drivers in real time.

🧠 **Runs entirely in Ring 3 (User Mode)** — it does not require or use kernel-level (Ring 0) access. All system monitoring is done via safe, read-only user-mode APIs.

---

## ⚠️ Disclaimer
> **You are strongly encouraged to compile this yourself.**

This tool is strictly **read-only**. It does **not block**, **interfere with**, or **modify** any system or third-party software. It is designed solely for **self-monitoring purposes**.

While a prebuilt `.exe` may be provided for convenience, use it **at your own risk** — and only if you trust the source.

---

## ✨ Features

✅ System tray app that runs quietly in the background  
🔍 Monitors over 20 known anti-cheat services and kernel-mode drivers  
🛑 Detects when anti-cheat services run too long without a game  
🧠 Uses WMI for hidden driver detection (admin only)  
👤 Works without admin for basic functionality  

---

## 🧰 Requirements
- 🪟 Windows 10 or 11
- 🧱 Visual Studio 2022 (Community Edition or higher)
- 📦 Workload: `.NET desktop development`

---

## 🧪 Step-by-Step: Build from Source

### 🔧 1. Install Visual Studio Components
- Open **Visual Studio Installer**
- Check ✅ **.NET desktop development**
- Click **Modify** to install the required tools

### 🛠️ 2. Create the Project
1. Open Visual Studio
2. Click **Create a new project**
3. Search: `Windows Forms App (.NET Framework)`
4. Click **Next**
5. Name it: `AntiCheatTrayWatcher`
6. Choose **.NET Framework 4.7.2** or higher

### 📄 3. Replace the Default Code
- Open `Program.cs`
- Replace all contents with the code in the `Anti Cheat Service List` canvas
- You may delete `Form1.cs` if not needed (the app runs tray-only)

### 🔐 4. Add the Admin Manifest (Optional but Recommended)
- Right-click the project → **Add → New Item → Application Manifest File**
- Paste the content from the `Anti Cheat Admin Manifest` canvas
- This enables admin mode support and deep scanning

### 🚀 5. Build and Run
- Press **Ctrl + F5** to run the app
- You’ll see a shield icon appear in your tray
- Double-click the icon to view active anti-cheat services
- Notifications will alert you when services start/stop or misbehave

---

## 🗂️ Optional: Precompiled `.exe`
While compiling it yourself is strongly recommended, a prebuilt `.exe` may be provided:

- 🔐 It’s **not required**, nor encouraged, unless you trust the source
- 🧾 Always refer to the source code — it’s transparent and readable

> "Build it, trust it. Run it, own it."

---

## 📝 License
Released under the **MIT License** — do what you want with it. Just don’t try to sue me if it makes your keyboard smell like Doritos.

---

### 💬 Questions?
Feel free to fork, open an issue, or contribute on GitHub.

---

### ✅ Code Transparency Notice
This code was **vetted and verified by ChatGPT** as of the most recent update. While every effort has been made to ensure it is clean, safe, and functional:

> 🔍 **Always verify any code before running it on your system.**

Don’t just take the author's word for it — check the code, understand what it does, and compile it yourself if possible.

Happy monitoring! 🖥️🐾

---

### ✍️ Author's Note
I believe in **transparency**, **security**, and **privacy** for everyone as a fundamental right. This project reflects that belief — offering clarity and control over what runs on your machine, without hidden agendas or invasive behavior.

---

### ❓ Frequently Asked Questions

**Q: Will this interfere with my games or anti-cheat software?**  
🅰️ No. This app is read-only and does not block, alter, or modify anything.

**Q: Why does it ask for admin access?**  
🅰️ Admin access allows deeper visibility into protected system services and drivers. It's optional, but recommended for full monitoring.

**Q: Can I trust the precompiled `.exe`?**  
🅰️ You're encouraged to compile it yourself from source, but a precompiled version may be provided for convenience. Always verify before using.

---

### 📘 Why This Tool Is Safe (README Extension)
This app is designed with **non-technical users in mind**, and here’s why you can trust it:

🔒 **Read-only:** It doesn’t write, modify, or inject into anything — it only reads system status.
🧠 **Ring 3 (User Mode):** No kernel-level access. Nothing runs in the background that could destabilize your system.
📡 **No Internet Access:** The program makes zero web requests — nothing gets uploaded, nothing gets tracked.
📁 **No Registry Edits:** It doesn’t touch your registry, startup entries, or critical paths unless you explicitly choose to add autostart manually.
👨‍👩‍👧‍👦 **Runs quietly in tray:** It doesn’t get in the way and won’t interrupt your workflow or gaming.
🧰 **DIY Friendly:** You’re encouraged to build it yourself and inspect every line.

> 💬 **Transparency is power.** If you're not sure about something, read the code — and ask questions.
