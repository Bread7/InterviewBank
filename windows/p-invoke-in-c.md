# P Invoke in c\#

## Why Red teamer like to use C# (To exploit windows)

1\) C# is created by microsoft, using C# is to create tool can easily manage windows API, modify registers, file handling, compare to other language that might need extra libraries or build from scratch.

2\) C# is built on .Net Core framework, it provides strong libraries and functionalities such as network communication, encryption, reflective. These functions are important for malware, pentest tool and backdoors, it reduce the extrawork from working from scratch.

3\) C# is compiled into Intermediate Language (IL) and executed by the Common Language Runtime (CLR) at runtime. Hackers can take advantage of this by using obfuscation tools (such as ConfuserEx) to encrypt or transform the code, making it harder for antivirus software to perform static analysis

Some of famous c# tool:

* Cobalt strike beacon Loader
* SharpSploit
* Seabelt
* Rubeus
* SharpShooter

## What is P/Invoke to do with C#?

P/Invoke (Platform Invocation Services) is a .NET runtime machism to allow managed code like c# to directly access unmanage code like Windows DLL that are written in C. It's main idea is to call Windows API and other native libraries.

### What is managed and unmanaged code?

Maybe just some table to see if you're able to spot the difference.

| **Aspect**              | **Managed Code**                                                | **Unmanaged Code**                                        |
| ----------------------- | --------------------------------------------------------------- | --------------------------------------------------------- |
| **Runtime Environment** | Runs under **.NET CLR**                                         | Runs directly on OS (no CLR)                              |
| **Memory Management**   | **Automatic** (Garbage Collection)                              | **Manual** (`malloc/free`, `new/delete`)                  |
| **Language Support**    | C#, VB.NET, F#                                                  | C, C++, Assembly                                          |
| **Security**            | **Safer** (Buffer overflow protection, managed execution)       | **Less secure** (Prone to buffer overflows, memory leaks) |
| **Interoperability**    | Requires **P/Invoke** or **COM Interop** to call unmanaged code | Can call unmanaged code directly                          |
| **Performance**         | Slight overhead due to runtime management                       | Faster (direct execution without CLR overhead)            |
| **Exception Handling**  | Handled by **CLR**                                              | Handled manually by the developer                         |

#### Side topic: Common language runtime (CLR)

CLR is something like JVM in java, C# codes are compiled into the "middle layer language" (Intermediate language), when it runs, the CLR JIT compiler will translate to machine code.

CLR handles garbage collections, developer does not need to allocate or free the memory usage.

Like JVM, it helps to handle exceptions and ensure security.

#### **Main Difference: Managed vs. Unmanaged Code**

* The **main difference** is indeed **memory management**.
* **Managed code** is controlled by the **.NET Runtime (CLR)**, which acts as a **middleman** to handle low-level tasks like **garbage collection**, memory management, and security.
* Like other high-level languages, **managed code is protected** from memory corruption issues (e.g., buffer overflows).
* **Unmanaged code**, on the other hand, **gives full control** over memory allocation, meaning developers must manually allocate and free memory (which can lead to leaks or corruption).

## Deeper Thinking&#x20;

Is there the only reason why we need to use P/Invoke?

The .Net runtijme already utllise P/invoke under the hood, and provided us with abstractions that runs on top, for example, if you want to start a process in .NET. We can utilise the `Start`method in the `System.Diagnostics.Process`Class. If we trace this method in the run time, it actually uses P/Invoke to call the createProcess API.&#x20;

Howevent seens it's in a high level, it does not actually allow us to customise the data being passed into the STARTUPINFO struct and prevent us from being able to do things like start the process in suspended state.

## Example of P/Invoke&#x20;

Assume we want to create a messageBox using the Windows API

{% embed url="https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messagebox" %}

```
int MessageBox(
  [in, optional] HWND    hWnd,
  [in, optional] LPCTSTR lpText,
  [in, optional] LPCTSTR lpCaption,
  [in]           UINT    uType
);
```

First we need to import the DLL (user32.dll for MessageBox)

```csharp
[DllImport("user32.dll", CharSet = CharSet.Unicode)]
```

According to the microsoft documentations, it requires 4 parameters.

{% code overflow="wrap" %}
```csharp
Static extern int MessageBoxw(IntPtr hWnd, string lpText, String lpCaption, uint uType);
```
{% endcode %}

Now Let's call it in complete code

```csharp
using System;
using System.Runtime.InteropServices;

namespace ConsoleApp1
{
    internal class Program
    {
        [DllImport("user32.dll", CharSet = CharSet.Unicode)]
        static extern int MessageBoxW(IntPtr hWnd, string lpText, string lpCaption, uint uType);

        static void Main(string[] args)
        {
            MessageBoxW(IntPtr.Zero, "Hello World!", "P/Invoke", 0);
        }
    }
}
```

## Interview Question:

What is managed and unmanaged code?

What is Common Language Runtime

Since .Net have some function that build on top of P/Invoke, as a hacker why do we need to use P/Invoke?
