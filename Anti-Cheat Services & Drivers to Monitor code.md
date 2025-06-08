/*
 * ✅ Anti-Cheat Watchdog Core
 * ----------------------------
 * This code monitors known anti-cheat services and drivers from a Windows system tray app.
 * It runs entirely in Ring 3 (User Mode), and never modifies system state.
 *
 * 📝 DISCLAIMER:
 * This application can be run with or without administrator privileges.
 * Running without admin grants access to basic service detection only.
 * Running as administrator unlocks full system scanning capabilities, including:
 *   - Access to protected system services and drivers
 *   - Detection of hidden kernel-mode anti-cheats (e.g., vgk, BEDaisy, mhyprot2)
 *   - Full WMI driver and service visibility
 * Note: This app does not modify system state. It is read-only and safe to use.
 *
 * 🔓 LICENSE: MIT
 * You are free to use, modify, distribute, and share this code for personal or commercial use.
 * Just don't claim you wrote it all by yourself, and don't blame us if your toaster gains sentience.
 */


using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.ServiceProcess;
using System.Timers;
using System.Windows.Forms;
using System.Drawing;
using System.Management; // For WMI driver scanning

namespace AntiCheatTrayWatcher
{
    static class Program
    {
        static NotifyIcon trayIcon;
        static System.Timers.Timer checkTimer;
        static Dictionary<string, DateTime?> activeAntiCheats = new Dictionary<string, DateTime?>();
        static List<string> monitoredServices = new List<string>
        {
            "BEService",
            "BEDaisy",
            "vgk",
            "vgs",
            "EasyAntiCheat",
            "EAC",
            "FaceitAC",
            "FACEIT",
            "ESEADriver2",
            "ESEAClient",
            "Xhunter1",
            "nProtect GameGuard",
            "npggsvc",
            "mhyprot2",
            "AntiCheatExpert",
            "TseEngine",
            "ggpmdrv",
            "iqvw64e",
            "ScDrv",
            "xhunter",
            "CheatBuster",
            "RustProtect",
            "GameProtect"
        };

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);

            trayIcon = new NotifyIcon
            {
                Icon = SystemIcons.Shield,
                Visible = false,
                Text = "Anti-Cheat Monitor"
            };

            trayIcon.ContextMenu = new ContextMenu(new MenuItem[] {
                new MenuItem("Exit", (s, e) => Exit())
            });

            trayIcon.DoubleClick += (s, e) =>
            {
                var activeList = activeAntiCheats.Where(p => p.Value.HasValue).Select(p => p.Key).ToList();
                if (activeList.Count > 0)
                {
                    MessageBox.Show("Active Anti-Cheat Services:\n\n" + string.Join("\n", activeList), "Anti-Cheat Status", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
                else
                {
                    MessageBox.Show("No anti-cheat services are currently running.", "Anti-Cheat Status", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
            };

            checkTimer = new System.Timers.Timer(3000);
            checkTimer.Elapsed += CheckServices;
            checkTimer.Start();

            Application.Run();
        }

        static void CheckServices(object sender, ElapsedEventArgs e)
        {
            foreach (var svcName in monitoredServices)
            {
                try
                {
                    var sc = new ServiceController(svcName);
                    bool isRunning = sc.Status == ServiceControllerStatus.Running;

                    if (isRunning)
                    {
                        if (!activeAntiCheats.ContainsKey(svcName) || !activeAntiCheats[svcName].HasValue)
                        {
                            activeAntiCheats[svcName] = DateTime.Now;
                            ShowBalloon($"{svcName} started", "Anti-cheat service is now active.");
                            trayIcon.Visible = true;
                        }
                        else
                        {
                            TimeSpan elapsed = DateTime.Now - activeAntiCheats[svcName].Value;
                            if (elapsed.TotalMinutes >= 15 && !IsGameProcessRunning())
                            {
                                ShowBalloon($"Suspicious activity detected", $"{svcName} has been running for over 15 minutes with no associated game detected.");
                                activeAntiCheats[svcName] = null;
                            }
                        }
                    }
                    else if (activeAntiCheats.ContainsKey(svcName) && activeAntiCheats[svcName].HasValue)
                    {
                        activeAntiCheats[svcName] = null;
                        ShowBalloon($"{svcName} stopped", "Anti-cheat service has shut down.");
                        if (!activeAntiCheats.Any(kv => kv.Value.HasValue))
                            trayIcon.Visible = false;
                    }
                }
                catch
                {
                    // Ignore missing services
                }
            }

            // Additional WMI scan for hidden or driver-based services
            CheckForHiddenDrivers();
        }

        static void CheckForHiddenDrivers()
        {
            try
            {
                var searcher = new ManagementObjectSearcher("SELECT * FROM Win32_SystemDriver");
                foreach (ManagementObject obj in searcher.Get())
                {
                    string name = obj["Name"]?.ToString();
                    string state = obj["State"]?.ToString();

                    if (!string.IsNullOrEmpty(name) && state == "Running" && monitoredServices.Contains(name))
                    {
                        if (!activeAntiCheats.ContainsKey(name) || !activeAntiCheats[name].HasValue)
                        {
                            activeAntiCheats[name] = DateTime.Now;
                            ShowBalloon($"{name} (Driver) active", "Detected hidden or driver-based anti-cheat.");
                            trayIcon.Visible = true;
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Debug.WriteLine("WMI scan failed: " + ex.Message);
            }
        }

        static bool IsGameProcessRunning()
        {
            string[] knownGames = new string[] { "valorant", "fortnite", "pubg", "apex", "genshin", "rust", "fifa", "battlefield", "r6", "cod", "overwatch", "league" };
            var processes = Process.GetProcesses();
            return processes.Any(p => knownGames.Any(g => p.ProcessName.ToLower().Contains(g)));
        }

        static void ShowBalloon(string title, string text)
        {
            trayIcon.BalloonTipTitle = title;
            trayIcon.BalloonTipText = text;
            trayIcon.ShowBalloonTip(3000);
        }

        static void Exit()
        {
            checkTimer?.Stop();
            trayIcon.Visible = false;
            trayIcon.Dispose();
            Application.Exit();
        }
    }
}
