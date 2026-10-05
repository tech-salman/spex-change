# spex-change
A complete guide and reference manual for customizing Windows system properties, OEM details, CPU display names, and managing VRAM/memory tweaks safely via the Windows Registry. Step-by-step guides and instructions for modifying Windows system specs, OEM manufacturer info, processor names, and memory settings using the Registry Editor. This aso includes How to Fix "Settings Managed by Your Organization" Errors.


# Windows Registry Customization & System Tweaks Guide

A comprehensive, step-by-step guide and reference manual for safely customizing Windows system properties, OEM details, CPU display names, and managing VRAM/memory tweaks via the Windows Registry (`regedit`).

---

## ⚠️ Important: Back Up the Registry First

Always make a backup before editing the registry to prevent system issues: [3, 4] 

1. Press `Windows Key + R`, type `regedit`, and hit **Enter**.
2. Click **File > Export** in the Registry Editor.
3. Choose a save location, name the file, and set the export range to **All**. [3, 4] 

---

## 1. Change OEM Manufacturer and Model Info

You can customize custom PC manufacturer, model, and support details shown in your system properties via the Registry Editor (`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\OEMInformation`). [1, 2] 

### Steps:
1. Open `regedit`.
2. Navigate to: 
   `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion` [1, 2]
3. If `OEMInformation` does not exist under `CurrentVersion`, right-click `CurrentVersion`, select **New > Key**, and name it `OEMInformation`. [1, 2] 
4. Click on the `OEMInformation` key. [2, 5] 
5. In the right pane, right-click empty space and choose **New > String Value**. Create and name the values precisely as needed:
   * `Manufacturer`
   * `Model`
   * `SupportPhone`
   * `SupportHours`
   * `SupportURL` [2, 5] 
6. Double-click any of the new values to enter your custom text into the **Value data** box, then click **OK**. [2] 
7. Restart your PC to apply the changes. [1, 4] 

---

## 2. Change Processor (CPU) Display Name

To change the displayed CPU name in Windows System Properties or Task Manager (*note: this is a cosmetic label change only*): [6, 7, 8] 

1. Open `regedit`.
2. Navigate to: 
   `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\CentralProcessor\0` *(or check subkeys numbered for each core)*.
3. Locate `ProcessorNameString` on the right side and double-click it.
4. Type your desired text in the **Value data** field and click **OK**. [6, 7, 8, 9] 

---

## 3. Change Computer Name (Device Name)

If you want to change your actual network or device name rather than manual OEM text: [3] 

* **The Easiest Method:** Go to **Start > Settings > System > About** and select **Rename this PC**. [3] 
* **Another Way to Change Network and Device Identity:** Go to **Start > Settings > System > About** and select **Rename this PC**. This updates both your local device name and your network identification seamlessly.

---

## 4. RAM & Memory Modifications

> **Note:** Unlike cosmetic changes to your CPU or manufacturer info, you **cannot** change your physical RAM capacity (e.g., changing "8 GB" to "16 GB") via the Registry Editor. Windows reads your total physical RAM directly from the motherboard's BIOS at startup. [1] 

However, depending on your goal, here are two common registry modifications related to RAM:

### A. Increase Dedicated Video RAM (VRAM) Allocation
If integrated graphics say you don't have enough VRAM for a game or app, you can use a registry trick to force the system to report a higher VRAM cap: [2, 3] 

1. Open `regedit`.
2. Navigate to: 
   `HKEY_LOCAL_MACHINE\SOFTWARE\Intel` *(or `AMD` depending on your processor)*.
3. Right-click the folder, select **New > Key**, and name it `GMM`.
4. Click the new `GMM` folder. In the right pane, right-click empty space and choose **New > DWORD (32-bit) Value**.
5. Name it exactly `DedicatedSegmentSize`.
6. Double-click it, change the Base to **Decimal**, and enter your desired VRAM size in Megabytes (e.g., `512` for 512MB, `1024` for 1GB, or `2048` for 2GB). Click **OK**.
7. Restart your computer. [3, 4, 5] 

### B. Fix High RAM / Memory Leak Issues
If your RAM usage is hitting 100% due to common Windows memory leaks caused by the Network Data Usage service: [6, 7] 

1. Open `regedit`.
2. Navigate to: 
   `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Ndu`
3. In the right pane, double-click the `Start` value.
4. Change the **Value data** from `2` (Automatic) to `4` (Disabled).
5. Click **OK** and restart your PC. [6, 7] 

---

# How to Fix "Settings Managed by Your Organization" Errors

When you see messages like **"Some settings are managed by your organization"** on a personal PC, it usually means a **Group Policy** or a specific registry key has locked down that feature. This often happens after using "privacy fixer" tools, tweaking tools, or linking a school/work Microsoft account.

You can fix this and unlock your settings by removing these organizational policies through the registry or the Command Prompt.

---

## Method 1: Clear the Policies via Command Prompt (Fastest)
The quickest way to strip out all "organization" restrictions is to force-delete the policy registry keys using the Command Prompt.

1. Click the **Start Menu**, type `cmd`, right-click **Command Prompt**, and select **Run as administrator**.
2. Copy and paste the following commands one by one, pressing **Enter** after each one:
   ```cmd
   reg delete "HKLM\Software\Policies\Microsoft" /f
   ```
   ```cmd
   reg delete "HKCU\Software\Policies\Microsoft" /f
   ```
   ```cmd
   reg delete "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies" /f
   ```
   ```cmd
   reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies" /f
   ```
3. Type the following command to refresh your system policies immediately:
   ```cmd
   gpupdate /force
   ```
4. Restart your computer.

---

## Method 2: Manually Delete the Lockout Keys in Registry Editor
If you prefer to see exactly what is blocking your permissions, you can delete the specific restriction folders manually:

1. Press `Windows Key + R`, type `regedit`, and press **Enter**.
2. Navigate to this location in the left sidebar:
   `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\`
3. Look for subfolders under `Microsoft` related to the settings you can't access (for example: `WindowsUpdate`, `Windows Defender`, or `System`). 
4. Right-click the restricting subfolder and click **Delete**.
5. Next, navigate to this location:
   `HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\`
6. Look for and delete any similar restriction folders here as well.
7. Restart your PC to let Windows rebuild the default user permissions.

---

## Method 3: Disconnect Work or School Accounts
If your PC is linked to an educational or corporate Microsoft account, that organization's security profile will actively enforce these restrictions on your machine.

1. Open Windows **Settings** (`Windows Key + I`).
2. Go to **Accounts** > **Access work or school**.
3. Look for any email addresses listed there that belong to a school or job.
4. Click on the account and click **Disconnect**. *(Note: This might remove your access to company emails or school apps on this device until you log into them individually again).*
