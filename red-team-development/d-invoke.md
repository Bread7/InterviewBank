# D Invoke

#### What is D/Invoke?

**D/Invoke** (Dynamic Invocation) is a technique used in software development, particularly in the context of Windows API calls, to dynamically resolve and invoke functions at runtime. It is often used in scenarios where developers want to avoid static linking or direct references to Windows API functions, which can be flagged by security software or make the code more detectable.

D/Invoke is commonly used in offensive security tools, malware, and red teaming frameworks to evade detection by antivirus (AV) and endpoint detection and response (EDR) solutions. It allows developers to call Windows API functions without explicitly importing them, making the code more stealthy.

***

#### Benefits of Using D/Invoke

1. **Improved Stealth** (Red-teaming):
   * D/Invoke allows developers to create more stealthy applications or payloads that are harder to detect by traditional security tools.
2. **Dynamic Functionality** (Development):
   * It enables dynamic resolution of APIs, making the code more flexible and adaptable to different environments.
3. **Useful for Research** (Blue-teaming):
   * Security researchers can use D/Invoke to study evasion techniques and improve defensive mechanisms.
4. **Reduced Attack Surface** (Blue-teaming):
   * By avoiding direct API imports, the application may have a smaller attack surface, as fewer dependencies are exposed.

***

#### Real-World Usages of D/Invoke

1. **Red Teaming and Penetration Testing**:
   * Red teamers and penetration testers use D/Invoke to execute malicious actions (e.g., process injection, credential dumping) while evading detection by security tools.
   * It is often used in tools like Cobalt Strike, Meterpreter, and custom payloads.
2. **Malware Development**:
   * Malware authors use D/Invoke to hide their use of Windows API calls, making it harder for security solutions to detect and analyse their code.
   * It is particularly useful for advanced persistent threats (APTs) and fileless malware.
3. **Bypassing Security Solutions**:
   * D/Invoke can be used to bypass static analysis and signature-based detection mechanisms, as it avoids direct imports of suspicious APIs.
4. **Legitimate Software Development**:
   * In some cases, legitimate software developers may use D/Invoke to dynamically load libraries or APIs to improve compatibility or reduce the attack surface of their applications.

***

#### Advantages of D/Invoke

1. **Evasion of Detection**:
   * D/Invoke helps avoid static analysis by security tools, as it does not rely on direct imports of Windows API functions.
2. **Flexibility**:
   * Developers can dynamically resolve and call APIs at runtime, making the code more adaptable to different environments.
3. **Reduced Footprint**:
   * By avoiding direct API imports, the binary size and metadata can be reduced, making the application less suspicious.
4. **Useful for Testing**:
   * Red teamers and security researchers can use D/Invoke to test the effectiveness of security solutions without triggering alerts.

***

#### Disadvantages of D/Invoke

1. **Complexity**:
   * Implementing D/Invoke requires a deeper understanding of Windows internals, memory management, and API calling conventions, making it more complex than traditional API calls.
2. **Potential for Errors**:
   * Dynamically resolving and invoking APIs can lead to runtime errors if not implemented correctly, such as incorrect function signatures or memory corruption.
3. **Increased Scrutiny**:
   * While D/Invoke can evade some detection mechanisms, advanced EDR solutions may still detect its usage through behavioral analysis or heuristics.
4. **Ethical Concerns**:
   * The primary use case for D/Invoke is often malicious, which can lead to ethical and legal concerns for developers.

***

#### Vulnerabilities and Risks

1. **Detection by Advanced EDR**:
   * While D/Invoke can evade static analysis, advanced EDR solutions may detect its usage through runtime monitoring, behavioral analysis, or heuristic detection.
2. **Exploitation by Attackers**:
   * Malware authors can misuse D/Invoke to create more stealthy and evasive payloads, making it harder for defenders to detect and respond to threats.
3. **Potential for Abuse**:
   * The technique can be abused to bypass security controls, making it a double-edged sword for both attackers and defenders.

***

#### Real-World Red Team Adaptation

In red team tools, this technique is often used for **stealthier API calls**, such as:

* **Process Injection** (e.g., `VirtualAllocEx`, `CreateRemoteThread`).
* **Credential Dumping** (e.g., `Lsass` memory access via `MiniDumpWriteDump`).
* **Defense Evasion** (e.g., unhooking EDRs by manually mapping DLLs). For example, to call `VirtualAllocEx` dynamically:

```csharp
using System;
using System.Runtime.InteropServices;
using DInvoke.DynamicInvoke; // Requires DInvoke (TheWover/DInvoke) NuGet package

public class Program
{
    // Define a delegate matching the signature of MessageBoxW
    [UnmanagedFunctionPointer(CallingConvention.StdCall)]
    private delegate int MessageBoxW(
        IntPtr hWnd,
        string lpText,
        string lpCaption,
        uint uType);

    public static void Main()
    {
        // Dynamically resolve user32.dll and MessageBoxW without P/Invoke imports
        IntPtr user32 = Generic.GetLoadedModuleAddress("user32.dll");
        IntPtr messageBoxAddr = Generic.GetExportAddress(user32, "MessageBoxW");

        // Map the delegate to the resolved function address
        MessageBoxW messageBox = Marshal.GetDelegateForFunctionPointer<MessageBoxW>(messageBoxAddr);

        // Call MessageBoxW dynamically
        messageBox(IntPtr.Zero, "D/Invoke Demo", "Hello World", 0);
    }
}

// Example delegate for VirtualAllocEx
[UnmanagedFunctionPointer(CallingConvention.StdCall)]
private delegate IntPtr VirtualAllocEx(
    IntPtr hProcess,
    IntPtr lpAddress,
    uint dwSize,
    AllocationType flAllocationType,
    MemoryProtection flProtect);

// Dynamically resolve and invoke it
IntPtr kernel32 = Generic.GetLoadedModuleAddress("kernel32.dll");
IntPtr virtualAllocExAddr = Generic.GetExportAddress(kernel32, "VirtualAllocEx");
var virtualAllocEx = Marshal.GetDelegateForFunctionPointer<VirtualAllocEx>(virtualAllocExAddr);

// Allocate memory in a remote process (hypothetical use case)
IntPtr allocatedMemory = virtualAllocEx(
    targetProcessHandle,
    IntPtr.Zero,
    0x1000,
    AllocationType.Commit | AllocationType.Reserve,
    MemoryProtection.ExecuteReadWrite);
```

***

#### Conclusion

D/Invoke is a powerful technique with legitimate uses in software development and security research, but it is often associated with malicious activities due to its ability to evade detection. While it offers advantages like improved stealth and flexibility, it also introduces complexity and potential risks. Defenders should be aware of D/Invoke and similar techniques to better detect and respond to advanced threats.

### Interview questions

1. **What is D/Invoke, and how does it differ from traditional P/Invoke?**
2. **Why is D/Invoke commonly used in red teaming or malware development?**
3. **Name two Windows APIs that attackers might invoke dynamically using D/Invoke.**
4. **What are the limitations of D/Invoke? How can EDRs still detect it?**

Author: [Ninjarku](https://github.com/Ninjarku)🐱‍👤

### References

1. https://github.com/TheWover/DInvoke
2. https://thewover.github.io/Dynamic-Invoke/
3. https://www.tevora.com/threat-blog/dynamic-invocation-in-csharp/
