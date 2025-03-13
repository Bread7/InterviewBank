# IFEO (lmage File Execution Options) Hijacking

**IFEO (Image File Execution Options)** is a Windows feature originally intended for debugging but has since been weaponized by attackers.

By modifying the registry entries under this feature, adversaries can intercept the execution of legitimate applications and redirect them to malicious code, thereby establishing persistence or hijacking program behaviour.

### **How it works**

The IFEO registry keys (found under `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`) are designed for debugging purposes. An attacker can set a “Debugger” value for a specific executable. When that program is launched, Windows starts the debugger which is instead potentially malicious executable.

**Why it’s effective:** This method doesn’t replace the original executable outright; instead, it intercepts its launch, which can make detection more difficult.

## Example

#### How Attackers Exploit IFEO

* **Hijacking Execution:** An attacker may target a common executable (e.g., `notepad.exe`) by setting its IFEO debugger key to a malicious payload. Every time the user or system calls that executable, the malicious code is run instead.
* **Stealth Persistence:** Since IFEO entries reside in a system registry location that isn’t as frequently monitored as standard autorun keys (like Run/RunOnce), modifications may remain unnoticed for longer periods.
* **Bypassing Protections:** Because the registry key is part of Windows’ debugging infrastructure, many automated security tools might overlook these changes.

#### Example Attack Code

The following code snippets illustrate how an attacker might create an IFEO entry for a target executable (in this case, `notepad.exe`):

**Using Command Prompt**

```bat
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v Debugger /d "C:\malicious\malware.exe" /f
```

**Using PowerShell**

```powershell
New-Item -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" -Force
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" -Name "Debugger" -Value "C:\malicious\malware.exe" -PropertyType String -Force
```

### Defensive Measures

Defenders can take several steps to detect and mitigate IFEO abuse, namely active monitoring and detection, as well as applying mitigation techniques.

#### Monitoring and Detection

* **Registry Monitoring:**
  * Tools like [**Sysmon**](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon), [Windows Event Logs](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.5), or third-party solutions to watch for new or modified keys under\
    `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`.
* **Baseline Comparison:** Maintain a known-good baseline of IFEO entries. Any deviation (especially non-standard debugger values) should trigger an alert.
* [**Elastic Search Rule**](https://www.elastic.co/guide/en/security/current/image-file-execution-options-injection.html#_rule_query_475): Using SIEMs such as Elastic Search can be deployed for larger organizations that have many machines to check. Elastic search rule

```elastic
registry where host.os.type == "windows" and event.type == "change" and
  registry.value : ("Debugger", "MonitorProcess") and length(registry.data.strings) > 0 and
  registry.path : (
    "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*.exe\\Debugger",
    "HKLM\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*\\Debugger",
    "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess",
    "HKLM\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess",
    "\\REGISTRY\\MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*.exe\\Debugger",
    "\\REGISTRY\\MACHINE\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*\\Debugger",
    "\\REGISTRY\\MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess",
    "\\REGISTRY\\MACHINE\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess",
    "MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*.exe\\Debugger",
    "MACHINE\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Image File Execution Options\\*\\Debugger",
    "MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess",
    "MACHINE\\SOFTWARE\\WOW6432Node\\Microsoft\\Windows NT\\CurrentVersion\\SilentProcessExit\\*\\MonitorProcess"
  ) and
    /* add FPs here */
  not registry.data.strings regex~ ("""C:\\Program Files( \(x86\))?\\ThinKiosk\\thinkiosk\.exe""", """.*\\PSAppDeployToolkit\\.*""")
```

#### Mitigation Techniques

* **Access Control:** Restrict permissions to the IFEO registry keys to limit who can create or modify these entries. Sample script to achieve this

```powershell
# Define the registry key path for IFEO
$registryPath = "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options"

# Retrieve the current ACL of the key
$acl = Get-Acl -Path $registryPath

# Display the current ACL for review (optional)
$acl | Format-List

# Define identities (modify these as needed)
$adminIdentity = "BUILTIN\Administrators"
$usersIdentity = "BUILTIN\Users"

# Create an access rule that gives Administrators full control
$adminRule = New-Object System.Security.AccessControl.RegistryAccessRule($adminIdentity, "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")

# Create an access rule that denies write permissions (e.g., SetValue, CreateSubKey, Delete) for Users
$userDenyRule = New-Object System.Security.AccessControl.RegistryAccessRule($usersIdentity, "SetValue, CreateSubKey, Delete", "ContainerInherit,ObjectInherit", "None", "Deny")

# Set or add the rules. In this example, we first ensure administrators have full control...
$acl.SetAccessRule($adminRule)
# ...and then add a deny rule for standard users.
$acl.AddAccessRule($userDenyRule)

# Apply the modified ACL back to the registry key
Set-Acl -Path $registryPath -AclObject $acl

Write-Output "Registry permissions updated for IFEO key."

```

* **Whitelisting:** Implement application whitelisting policies to ensure that only approved executables or debuggers can be launched. This requires other privileged applications to perform whitelisting via policies set, such as [**applocker**](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/app-control-for-business/applocker/applocker-overview)
* **Forensic Auditing:** Regularly audit systems for unexpected IFEO entries using PowerShell scripts. For example, a script to enumerate all IFEO entries might look like:

Sample Powershell Script for Enumeration:

```powershell
Get-ChildItem "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options" | ForEach-Object {
    $exe = $_.PSChildName
    $properties = Get-ItemProperty $_.PSPath
    if ($properties.Debugger) {
        Write-Output "Executable: $exe - Debugger set to: $($properties.Debugger)"
    }
}
```

### Interview Questions

1. What is IFEO, and how is it intended to be used in Windows?
2. How can an attacker abuse IFEO for persistence or code injection?
3. What are some methods or tools you could use to detect suspicious IFEO modifications on a Windows system?
4. How would you defend against the misuse of IFEO by an attacker?
5. Can you discuss a real-world incident or case study where IFEO abuse was observed in an attack?

Author: [`Ninjarku`](https://github.com/Ninjarku)🐱‍👤

### References

1. https://learn.microsoft.com/en-us/previous-versions/windows/desktop/xperf/image-file-execution-options
2. https://www.elastic.co/guide/en/security/current/image-file-execution-options-injection.html#\_investigation\_guide\_440
3. https://securityblueteam.medium.com/utilizing-image-file-execution-options-ifeo-for-stealthy-persistence-331bc972554e
