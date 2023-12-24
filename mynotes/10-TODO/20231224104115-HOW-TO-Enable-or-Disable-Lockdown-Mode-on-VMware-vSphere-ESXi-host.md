---
title: HOW TO: Enable or Disable Lockdown Mode on VMware vSphere ESXi host
url: http://vstrong.info/2012/09/13/how-to-enable-or-disable-lockdown-mode-on-vmware-vsphere-esxi-host/
clipped_at: 2023-12-24 10:41:15
category: default
tags: 
 - EXSI
 - lockdown
---

# HOW TO: Enable or Disable Lockdown Mode on VMware vSphere ESXi host

**Q:** What is Lockdown Mode?  
**A:** Lockdown Mode prevents users from logging directly to the host. The host will only be accessible through local console or vCenter Server. None of remote management options e.g. vCLI, PowerCLI script, SSH will work. When it is enabled, only _vpxuser_ () has authentication permissions and can connect to the host remotely.

**How can you enable/disable Lockdown Mode**:

1.  [From the Direct Console User Interface (DCUI)](#1);
2.  [From vSphere Client](#2);
3.  [Using ESXi Shell](#3);
4.  [Using PowerCLI script](#4).

  
**1\. Enable/Disable Lockdown Mode from DCUI**:

1.  Open server console;
2.  Press **F2** to **Customize System/View Logs**;
3.  Open **Configure Lockdown Mode**;
4.  Press SPACE to enable or disable lockdown mode;  
    [![](assets/1703385675-c8c8e6f9170d8e96c0f2fadf09e5db34.jpg "Enable Lockdown Mode - 1")](http://www.vstrong.info/wp-content/uploads/2012/09/Enable-Lockdown-Mode-1.jpg)
5.  Press ENTER to save the changes.

  
**2\. Enable/Disable Lockdown Mode in vSphere Client**:

1.  Open vSphere Client and connect to the host / vCenter Server;
2.  Select the host and click on **Configuration** tab;
3.  Under **Software** select **Security Profile**;
4.  Under **Lockdown Mode** click Edit;
5.  Tick / Untick **Enable Lockdown Mode**;  
    [![](assets/1703385675-d607d4e8381d1400354ae8dfdf180ea3.jpg "Enable Lockdown Mode - 2")](http://www.vstrong.info/wp-content/uploads/2012/09/Enable-Lockdown-Mode-21.jpg)
6.  Click **OK**.

  
**3\. Enable/Disable Lockdown Mode in ESXi Shell:**

Check if Lockdown mode is enabled: **vim-cmd -U dcui vimsvc/auth/lockdown\_is\_enabled**  
Enable Lockdown mode: **vim-cmd -U dcui vimsvc/auth/lockdown\_mode\_enter**  
Disable Lockdown mode: **vim-cmd -U dcui vimsvc/auth/lockdown\_mode\_exit**

Example:

```plain
~ # vim-cmd -U dcui vimsvc/auth/lockdown_is_enabled
false
~ # vim-cmd -U dcui vimsvc/auth/lockdown_mode_enter
~ # vim-cmd -U dcui vimsvc/auth/lockdown_is_enabled
true
~ # vim-cmd -U dcui vimsvc/auth/lockdown_mode_exit
~ # vim-cmd -U dcui vimsvc/auth/lockdown_is_enabled
false
```

  
**4\. Enable/Disable Lockdown Mode using PowerCLI script**:

```plain
$vCenter = 'vCenterServer_Name_or_IP_address'
Connect-VIServer $vCenter
 $Scope = Get-VMHost #This will change the Lockdown Mode on all hosts managed by vCenter, amend this if you need to
 foreach ($ESXhost in $Scope) {
 (get-vmhost $ESXhost | get-view).ExitLockdownMode() # To DISABLE Lockdown Mode
 # (get-vmhost $ESXhost | get-view).EnterLockdownMode() # To ENABLE Lockdown Mode
 }
Disconnect-VIServer -Server $vCenter -Confirm:$false
```

By Mark Strong, on September 13th, 2012 | Tags: [PowerCLI](http://vstrong.info/tag/powercli/), [Powershell](http://vstrong.info/tag/powershell/), [Script](http://vstrong.info/tag/script/), [VMware](http://vstrong.info/tag/vmware/) | Category: [Script,](http://vstrong.info/category/script/ "Script") [VMware](http://vstrong.info/category/vmware/ "VMware")