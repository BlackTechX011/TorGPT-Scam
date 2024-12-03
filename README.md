# Exposing the Scam Behind TorGPT: Uncovering the Hidden Threat

### The Story Behind the Investigation

A few months ago, a friend handed me a file named **TorGPT.exe**, claiming it was a cutting-edge AI tool that wasn’t functioning on their system. The demo video looked promising, and out of curiosity, I decided to test it on my own machine. However, due to an issue with .NET dependencies, it failed to execute, and I put it aside, forgetting about it.

Recently, while working on a forensic analysis algorithm, the file caught my attention again. Running it through my tools revealed shocking findings: **TorGPT.exe** wasn’t just malfunctioning—it was a sophisticated **malware dropper**. It deployed **SpyNote malware**, a dangerous spyware capable of compromising systems. This report documents my analysis, evidence, and findings to expose the malicious intent behind this scam.

---

> [!CAUTION]
> This report is for **educational and ethical purposes only**. The information contained herein aims to expose malicious campaigns and aid in defending against them. Under no circumstances should this analysis be used for unauthorized activities or malicious intent.
---
## Summary of Findings

- **TorGPT.exe** is a dropper malware disguised as an AI-based application.
- It exploits victims' systems by delivering **SpyNote malware** and other malicious payloads.
- Some contacted domains and IPs are known to mislead investigators by:
  - Using legitimate-looking endpoints.
  - Returning errors, such as `{"BadRequest":"An endpoint for the request '' is not valid for this service"}`, to evade detection.
- It is part of a larger scam targeting unsuspecting users with fake AI tools.
- If you are looking for more technical details, see the sections below for a detailed breakdown.

---

## File Details

### Main File
- **Name:** `TorGPT.exe`
- **Type:** Win32 Executable
- **Detected:** 43/75 antivirus engines flagged this as malicious.

### Dropped Files
1. **cfb22ef7-547c-4043-b2cc-30ae6b292def.dll**
   - **Type:** Win32 DLL
   - **Size:** 462.00 KB
   - **Purpose:** Likely used for malicious injection or persistence.
   - **Detection Rate:** Associated with multiple malicious executables like `TJprojMain` and `SpyNote X.exe`.

2. Bundled files within the dropper:
   - `54198208c5df802eca64c371bcc5cffafb1a4303fc24b065c6b6ca08ee84fbc4`
   - `eab2000b93112b85257650d597275c571cdedc13eee01c0b9568250fb83e82d1`
   - Additional hashes provided in the artifacts section.

---

## Execution Chain Analysis

The following diagram illustrates the **execution chain** of **TorGPT.exe**:

```plaintext
TorGPT.exe
   ├── Drops: cfb22ef7-547c-4043-b2cc-30ae6b292def.dll
   │       ├── Executes: SpyNote X.exe (Multiple Variants)
   │       └── Executes: TJprojMain.exe
   └── Bundled Payloads:
           ├── Obfuscated Payload 1 (54198208c5d...)
           ├── Obfuscated Payload 2 (eab2000b93...)
           └── Other malicious files
```

### Parent-Child Relationships

1. **TorGPT.exe** initiates execution.
2. Drops **cfb22ef7-547c-4043-b2cc-30ae6b292def.dll**, which acts as a loader for:
   - **SpyNote X.exe** (multiple malicious binaries detected).
   - **TJprojMain.exe**, associated with spyware activity.

---

## Network Indicators

### Contacted Domains
- **query.prod.cms.rt.microsoft.com**
  - **Domain Created:** February 2, 1991
  - **Registrar:** MarkMonitor Inc.
  - url is legitimate but used to misleads investigators by making fake requests.
  - url return:
    ```json
    {"BadRequest":"An endpoint for the request '' is not valid for this service"}
    ```
  - This tactic is used to deter automated analysis and manual investigation.

### Contacted IPs
- **184.25.191.235** (United States, ASN: 16625)
- **192.229.211.108** (United States, ASN: 15133)
- **20.99.133.109** (United States, ASN: 8075)
- **20.99.186.246** (United States, ASN: 8075)
- **23.216.147.76** (United States, ASN: 20940)

> [!NOTE]
>  Some IPs appear inactive or return 404 errors when queried. However, historical data links them to command-and-control (C2) operations and other malicious campaigns.

---

## Recommendations

- **Do not execute unknown files:** Always verify the source and integrity of files before running them.
- **Use up-to-date antivirus software:** Modern security tools can detect and quarantine such threats.
- **Analyze suspicious files in a sandboxed environment:** Avoid running them on your primary system.
- **Block malicious domains and IPs:** Add the listed domains and IPs to your firewall or security appliance.
- **Report incidents to authorities:** Share findings with cybersecurity organizations for wider awareness.
- **Be cautious of misleading indicators:** Legitimate-looking domains or IPs returning errors may still be part of a malware delivery chain.


---

> [!NOTE]
> All the findings and artifacts, including hashes and related files, are stored for further analysis. Contributions to this repository are welcome to expand on indicators of compromise (IOCs) and additional research.


---


# If You’re Here, Let’s Get Technical

<center>

If you've made it this far, you likely want to dive deeper into the technical details.  
This section is where the real forensic analysis comes to life.  
Get ready for a comprehensive breakdown of the evidence and the inner workings of the malicious software.

</center>

---

# [+] File Analysis Report


## Basic Properties

| Property          | Value                                                                 |
|-------------------|-----------------------------------------------------------------------|
| **Name**           | `TorGPT.exe`                                    |
| **MD5**           | `0510475cbbfd2001438a2cef052328ab`                                    |
| **SHA-1**         | `ca031654255f58f29d2c1d99075ca00edaf52255`                             |
| **SHA-256**       | `c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd`     |
| **Vhash**         | `21703675151550c32292660828`                                           |
| **Authentihash**  | `0cef7c356eae1b52225daa33bd197072952be622b39e057e3822d0fe2365a6e4`     |
| **Imphash**       | `f34d5f2d4577ed6d9ceec516c1f5a744`                                     |
| **SSDEEP**        | `196608:Y9cWyqfiAPEmTU9VWRc8Unf8zFpeUc37T1AGFX6rhDzVxfj2PFN9sWf:LWpfdE2KnfapeV316rhDz/fj2PFZf` |
| **TLSH**          | `T11AD622023A504D66D076A7F99893EA3CB3722EF81920C64B16F2EE5BFD523D41D3D681` |
| **File Type**     | Win32 EXE, executable, windows, win32, pe, peexe                      |
| **Magic**         | PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows    |
| **TrID**          | Generic CIL Executable (.NET, Mono, etc.) (44.1%)<br>Windows Control Panel Item (generic) (34.8%)<br>Win64 Executable (generic) (6.3%)<br>Win32 Dynamic Link Library (generic) (3.9%)<br>Win16 NE executable (generic) (3%) |
| **DetectItEasy**   | PE32<br>Library: Costura.Fody<br>Library: .NET (v4.0.30319)<br>Linker: Microsoft Linker |
| **Magika**        | PEBIN |
| **File Size**     | 12.18 MB (12774400 bytes) |
| **PEiD Packer**   | .NET executable |

## History

| Property                | Value                                      |
|-------------------------|--------------------------------------------|
| **Creation Time**       | 2079-11-17 05:53:41 UTC                    |




## Signature Info

| Property            | Value                        |
|---------------------|------------------------------|
| **Signature Verification** | File is not signed        |
| **File Version Information** | Copyright © 2024          |
| **Product**          | TorGPT                       |
| **Description**      | TorGPT                       |
| **Original Name**    | TorGPT.exe                   |
| **Internal Name**    | TorGPT.exe                   |
| **File Version**     | 1.0.0.0                      |
| **Comments**         | We Learn We Did              |

## Portable Executable Info

### .NET Details

| Property            | Value                                                                 |
|---------------------|-----------------------------------------------------------------------|
| **Module Version Id** | `83e9492f-ea46-405a-a293-5797d18df38c`                              |
| **TypeLib Id**       | `b5221054-69ed-43e7-91d8-19422d294f5b`                                |
| **Target Machine**  | Intel 386 or later processors and compatible processors                   |
| **Compilation Timestamp** | 2079-11-17 05:53:41 UTC                                            |
| **Entry Point**      | 12550430                                                              |
| **Contained Sections** | 3                                                                    |
| **Sections**         | **Name** | **Virtual Address** | **Virtual Size** | **Raw Size** | **Entropy** | **MD5** | **Chi2** |
|                     | .text | 8192 | 12542244 | 12542464 | 7.77 | `ce256773073ec722ca2cbc7169f4b027` | 9831373 |
|                     | .rsrc | 12558336 | 230834 | 230912 | 3.81 | `80a17a9356c5b5c891f940f761be5274` | 15241297 |
|                     | .reloc | 12795904 | 12 | 512 | 0.1 | `e5b54919665137dc639a60b41c0bf351` | 128015 |

### Imports

- `mscoree.dll`

### Contained Resources by Type

- **RT_GROUP_ICON**: 1
- **RT_VERSION**: 1
- **RT_MANIFEST**: 1
- **RT_ICON**: 1

### Contained Resources by Language

- **NEUTRAL**: 4

### Contained Resources

| Property          | Value                                                                 |
|-------------------|-----------------------------------------------------------------------|
| **SHA-256**       | `356ee6b3db9ac3b6ee43a638795c1d41177d3d70ac3e9f2bfd70e3bd90d6f3ae`    |
| **File Type**     | unknown                                                               |
| **Type**          | RT_ICON                                                               |
| **Language**      | NEUTRAL                                                               |
| **Entropy**       | 3.8                                                                   |
| **Chi2**          | 15171917                                                              |
| **SHA-256**       | `fa10977a1c455d978f9a0d67211d109adfde5718848df251379eadda8ed12485`    |
| **File Type**     | ICO                                                                   |
| **Type**          | RT_GROUP_ICON                                                         |
| **Language**      | NEUTRAL                                                               |
| **Entropy**       | 2.02                                                                  |
| **Chi2**          | 1797.6                                                                |
| **SHA-256**       | `941289decf43635430ec2750965d87f47dcec71c431f2c46204fb`              |
| **File Type**     | unknown                                                               |
| **Type**          | RT_VERSION                                                            |
| **Language**      | NEUTRAL                                                               |
| **Entropy**       | 3.31                                                                  |
| **Chi2**          | 69319.71                                                              |
| **SHA-256**       | `539dc26a14b6277e87348594ab7d6e932d16aabb18612d77f29fe421a9f1d46a`  |
| **File Type**     | unknown                                                               |
| **Type**          | RT_MANIFEST                                                           |
| **Language**      | NEUTRAL                                                               |
| **Entropy**       | 5                                                                     |
| **Chi2**          | 4719.86                                                               |

### Dot Net Assembly

| Property                  | Value                                                                 |
|---------------------------|-----------------------------------------------------------------------|
| **Common Language Runtime metadata version** | 1.1                                      |
| **CLR version**              | v4.0.30319                            |
| **Assembly name**            | TorGPT.exe                             |
| **Metadata header Relative Virtual Address** | 12516664                                |
| **Assembly flags**           | COMIMAGE_FLAGS_ILONLY, COMIMAGE_FLAGS_32BITREQUIRED                   |
| **Entry point token**        | 100663378                                                            |
| **RVA entry point**          | 1494348                                                              |
| **Resources va**             | 11022315                                                             |
| **Streams**                  | **Size** | **Entropy** | **Chi2** | **MD5**                    |
|                           | #GUID | 16 | 4 | 240 | `9c8ea394d38fe88141ff6622e572b498`    |
|                           | #Blob | 15112 | 2.41 | 2036449.13 | `dbc52fca5a342aef5faf2e5b350f036b` |
|                           | #US | 76 | 3.20 | 5502.11 | `ddae54df64c0a343b41a0b295c9f7b68`  |
|                           | #~ | 8872 | 5.77 | 191215.69 | `0416a79e50fd56c6cbaf95ab3b352de86317e669793`  |
|                           | #Strings | 9492 | 5.12 | 91709.13 | `288ca07642afb3b352de86317e669793`  |

### Manifest Resource

- `TorGPT.Properties.Resources.resources`
- `YourEvilChatbotApp.Form1.resources`
- `YourEvilChatbotApp.ImageGenForm.resources`
- `YourEvilChatbotApp.intro.resources`
- `costura.costura.dll.compressed`
- `costura.costura.pdb.compressed`
- `costura.metadata`
- `costura.microsoft.extensions.configuration.abstractions.dll.compressed`
- `costura.microsoft.extensions.configuration.dll.compressed`
- `costura.microsoft.extensions.configuration.fileextensions.dll.compressed`
- `costura.microsoft.extensions.configuration.newtonsoftjson.dll.compressed`
- `costura.microsoft.extensions.fileproviders.abstractions.dll.compressed`
- `costura.microsoft.extensions.fileproviders.physical.dll.compressed`
- `costura.microsoft.extensions.filesystemglobbing.dll.compressed`
- `costura.microsoft.extensions.primitives.dll.compressed`
- `costura.newtonsoft.json.dll.compressed`
- `costura.system.buffers.dll.compressed`
- `costura.system.diagnostics.diagnosticsource.dll.compressed`
- `costura.system.memory.dll.compressed`
- `costura.system.numerics.vectors.dll.compressed`
- `costura.system.runtime.compilerservices.unsafe.dll.compressed`
- `costura.system.valuetuple.dll.compressed`

### External Assemblies

- `Newtonsoft.Json v11.0.0.0`
- `System.Drawing v4.0.0.0`
- `System.Net.Http v4.2.0.0`
- `System v4.0.0.0`
- `mscorlib v4.0.0.0`
- `System.Windows.Forms v4.0.0.0`
- `System.Speech v4.0.0.0`
- `System.Core v4.0.0.0`

### Assembly Data

| Property            | Value                                                                 |
|---------------------|-----------------------------------------------------------------------|
| **majorversion**  | 1                                                                     |
| **hashalgid**     | 32772                                                                 |
| **flags_text**    | afPA_None                                                             |
| **name**          | TorGPT                                                                |


### What it is doing
```

Main File: TorGPT.exe
  |
  +-- Dropped Files
  |    |
  |    +-- cfb22ef7-547c-4043-b2cc-30ae6b292def.dll (Win32 DLL, 462.00 KB)
  |          |
  |          +-- Execution Parents
  |          |    |
  |          |    +-- TJprojMain (Win32 EXE, 70/74 detections)
  |          |    +-- TorGPT.exe (Win32 EXE, 43/75 detections)
  |          |    +-- SpyNote X.exe (Win32 EXE, 45/72 detections)
  |          |    +-- SpyNote X.exe (Win32 EXE, 43/72 detections)
  |          |    +-- TJprojMain (Win32 EXE, 69/74 detections)
  |          |
  |          +-- Bundled Files
  |                |
  |                +-- 54198208c5df802eca64c371bcc5cffafb1a4303fc24b065c6b6ca08ee84fbc4 (file)
  |                +-- eab2000b93112b85257650d597275c571cdedc13eee01c0b9568250fb83e82d1 (file)
  |                +-- 0cc0c39c5edb5d2a08642eb60e1f402890f279b12dd54248851c63a33cb6c748 (file)
  |                +-- 2bcfd9a1239552778a799f683bf11428dd0a82a8bb21955106cf0d7c2f477560 (file)
  |                +-- df4b1dc9bd96567d23815718432fb5fa254559cec78aac3645876839d2e28825 (file)
  |
  +-- Bundled Files
  |    |
  |    +-- 1 (XML)
  |    +-- 9b2837b8b5f37c4661b9d9e9559c757ef5c454d181cf9c127e566ab197f0ab06 (file)
  |    +-- fa10977a1c455d978f9a0d67211d109adfde5718848df251379eadda8ed12485 (file)
  |    +-- 83252b25376bbdb062beed858c3639e4283fb072aeb22266e8f35e3d9e199568 (file)
  |    +-- 353a1ec7b5d932a0cde20205a718ebb1466d076981bf9b9ced55e2b6f7ea2907 (file)
  |    +-- 097553aa7c4e47f2186e049a37791726713b7cf28b1996605970c40b29e37713 (file)
  |
  +-- Contacted Domains
  |    |
  |    +-- query.prod.cms.rt.microsoft.com (Created: 1991-02-02, Registrar: MarkMonitor Inc.)
  |
  +-- Contacted IPs
       |
       +-- 184.25.191.235 (Autonomous System: 16625, Country: US)
       +-- 192.229.211.108 (Autonomous System: 15133, Country: US)
       +-- 20.99.133.109 (Autonomous System: 8075, Country: US)
       +-- 20.99.186.246 (Autonomous System: 8075, Country: US)
       +-- 23.216.147.76 (Autonomous System: 20940, Country: US)
```

#### Main File: TorGPT.exe
| Type | Text | Has Detections | Type Tag |
|------|------|----------------|----------|
| file | TorGPT.exe | true | peexe |

____

#### Dropped Files
| Type | Text | File Type | Name | File Size |
|------|------|-----------|------|----------|
| file | | Win32 DLL | cfb22ef7-547c-4043-b2cc-30ae6b292def.dll | 462.00 KB |

___

#### Bundled Files (Main File)
| Type | Text | File Type | Name |
|------|------|-----------|------|
| file | | XML | 1 |
| file | | file | 9b2837b8b5f37c4661b9d9e9559c757ef5c454d181cf9c127e566ab197f0ab06 |
| file | | file | fa10977a1c455d978f9a0d67211d109adfde5718848df251379eadda8ed12485 |
| file | | file | 83252b25376bbdb062beed858c3639e4283fb072aeb22266e8f35e3d9e199568 |
| file | | file | 353a1ec7b5d932a0cde20205a718ebb1466d076981bf9b9ced55e2b6f7ea2907 |
| file | | file | 097553aa7c4e47f2186e049a37791726713b7cf28b1996605970c40b29e37713 |

___

#### Execution Parents of `cfb22ef7-547c-4043-b2cc-30ae6b292def.dll`
| Scanned | Detections | Type | Name |
|---------|------------|------|------|
| 2023-12-20 | 70/74 | Win32 EXE | TJprojMain |
| 2024-08-09 | 43/75 | Win32 EXE | TorGPT.exe |
| 2024-03-26 | 45/72 | Win32 EXE | SpyNote X.exe |
| 2024-07-09 | 43/72 | Win32 EXE | SpyNote X.exe |
| 2024-05-26 | 69/74 | Win32 EXE | TJprojMain |

___

#### Bundled Files of `cfb22ef7-547c-4043-b2cc-30ae6b292def.dll`
| Type | Name |
|------|------|
| file | 54198208c5df802eca64c371bcc5cffafb1a4303fc24b065c6b6ca08ee84fbc4 |
| file | eab2000b93112b85257650d597275c571cdedc13eee01c0b9568250fb83e82d1 |
| file | 0cc0c39c5edb5d2a08642eb60e1f402890f279b12dd54248851c63a33cb6c748 |
| file | 2bcfd9a1239552778a799f683bf11428dd0a82a8bb21955106cf0d7c2f477560 |
| file | df4b1dc9bd96567d23815718432fb5fa254559cec78aac3645876839d2e28825 |

____

#### Contacted Domains
| Domain | Created | Registrar |
|--------|---------|-----------|
| query.prod.cms.rt.microsoft.com | 1991-02-02 | MarkMonitor Inc. |

____

#### Contacted IP Addresses
| IP | Autonomous System | Country |
|----|-------------------|---------|
| 184.25.191.235 | 16625 | US |
| 192.229.211.108 | 15133 | US |
| 20.99.133.109 | 8075 | US |
| 20.99.186.246 | 8075 | US |
| 23.216.147.76 | 20940 | US |

____


### Type Definitions

- `System.Object`
- `System.Type`
- `System.RuntimeTypeHandle`
- `System.EventArgs`
- `System.String`
- `System.IDisposable`
- `System.EventHandler`
- `System.Exception`
- `System.Uri`
- `System.Char`
- `System.Action`
- `System.Environment`
- `System.StringSplitOptions`
- `System.STAThreadAttribute`
- `System.AppDomain`
- `System.StringComparison`
- `System.Byte`
- `System.ResolveEventArgs`
- `System.ResolveEventHandler`
- `System.Action`1
- `System.MulticastDelegate`
- `System.IAsyncResult`
- `System.AsyncCallback`
- `System.ValueType`
- `System.GC`
- `System.Array`
- `System.RuntimeFieldHandle`
- `System.IntPtr`
- `System.Guid`
- `System.Int32`
- `System.Resources.ResourceManager`
- `System.Globalization.CultureInfo`
- `System.Reflection.Assembly`
- `System.Reflection.AssemblyName`
- `System.Reflection.AssemblyNameFlags`
- `System.Reflection.AssemblyTitleAttribute`
- `System.Reflection.AssemblyDescriptionAttribute`
- `System.Reflection.AssemblyConfigurationAttribute`
- `System.Reflection.AssemblyCompanyAttribute`
- `System.Reflection.AssemblyProductAttribute`
- `System.Reflection.AssemblyCopyrightAttribute`
- `System.Reflection.AssemblyTrademarkAttribute`
- `System.Reflection.AssemblyFileVersionAttribute`
- `System.ComponentModel.EditorBrowsableAttribute`
- `System.ComponentModel.EditorBrowsableState`
- `System.ComponentModel.IContainer`
- `System.ComponentModel.ComponentResourceManager`
- `System.ComponentModel.ISupportInitialize`
- `System.ComponentModel.Component`
- `System.CodeDom.Compiler.GeneratedCodeAttribute`
- `System.Diagnostics.DebuggerNonUserCodeAttribute`
- `System.Diagnostics.DebuggerStepThroughAttribute`
- `System.Diagnostics.DebuggerHiddenAttribute`
- `System.Diagnostics.DebuggableAttribute`
- `System.Diagnostics.Process`
- `System.Runtime.CompilerServices.CompilerGeneratedAttribute`
- `System.Runtime.CompilerServices.AsyncVoidMethodBuilder`
- `System.Runtime.CompilerServices.AsyncStateMachineAttribute`
- `System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1
- `System.Runtime.CompilerServices.AsyncTaskMethodBuilder`
- `System.Runtime.CompilerServices.IAsyncStateMachine`
- `System.Runtime.CompilerServices.TaskAwaiter`1
- `System.Runtime.CompilerServices.TaskAwaiter`
- `System.Runtime.CompilerServices.CompilationRelaxationsAttribute`
- `System.Runtime.CompilerServices.RuntimeCompatibilityAttribute`
- `System.Runtime.CompilerServices.RuntimeHelpers`
- `System.Configuration.ApplicationSettingsBase`
- `System.Configuration.SettingsBase`
- `System.Windows.Forms.Form`
- `System.Windows.Forms.Button`
- `System.Windows.Forms.TextBox`
- `System.Windows.Forms.RichTextBox`
- `System.Windows.Forms.Label`
- `System.Windows.Forms.PictureBox`
- `System.Windows.Forms.Control`
- `System.Windows.Forms.Clipboard`
- `System.Windows.Forms.MessageBox`
- `System.Windows.Forms.DialogResult`
- `System.Windows.Forms.MessageBoxButtons`
- `System.Windows.Forms.MessageBoxIcon`
- `System.Windows.Forms.ImageLayout`
- `System.Windows.Forms.ButtonBase`
- `System.Windows.Forms.FlatStyle`
- `System.Windows.Forms.PictureBoxSizeMode`
- `System.Windows.Forms.ContainerControl`
- `System.Windows.Forms.AutoScaleMode`
- `System.Windows.Forms.FormStartPosition`
- `System.Windows.Forms.TextBoxBase`
- `System.Windows.Forms.SaveFileDialog`
- `System.Windows.Forms.FileDialog`
- `System.Windows.Forms.CommonDialog`
- `System.Windows.Forms.Timer`
- `System.Windows.Forms.FormBorderStyle`
- `System.Windows.Forms.FormClosedEventHandler`
- `System.Windows.Forms.FormClosedEventArgs`
- `System.Windows.Forms.Application`
- `System.Windows.Forms.Screen`
- `System.Net.Http.HttpClient`
- `System.Net.HttpFormUrlEncodedContent`
- `System.Net.Http.HttpResponseMessage`
- `System.Net.Http.HttpContent`
- `System.Net.Http.MultipartFormDataContent`
- `System.Net.Http.StreamContent`
- `System.Net.Http.HttpMessageInvoker`
- `System.Net.Http.ByteArrayContent`
- `System.Speech.Synthesis.SpeechSynthesizer`
- `System.Speech.Synthesis.Prompt`
- `System.Collections.Generic.List`1
- `System.Collections.Generic.KeyValuePair`2
- `System.Collections.Generic.IEnumerable`1
- `System.Collections.Generic.IEnumerator`1
- `System.Collections.Generic.Dictionary`2
- `System.Threading.Tasks.Task`1
- `System.Threading.Tasks.Task`
- `System.Threading.Tasks.Parallel`
- `System.Threading.Tasks.ParallelLoopResult`
- `Newtonsoft.Json.Linq.JObject`
- `Newtonsoft.Json.Linq.JToken`
- `System.Drawing.Color`
- `System.Drawing.Image`
- `System.Drawing.Point`
- `System.Drawing.Size`
- `System.Drawing.Font`
- `System.Drawing.FontStyle`
- `System.Drawing.GraphicsUnit`
- `System.Drawing.SystemColors`
- `System.Drawing.ContentAlignment`
- `System.Drawing.SizeF`
- `System.Drawing.Icon`
- `System.Drawing.Rectangle`
- `System.Drawing.Bitmap`
- `System.Drawing.Graphics`
- `uncategorized.ControlCollection`
- `uncategorized.SpecialFolder`
- `uncategorized.DebuggingModes`
- `System.IO.FileStream`
- `System.IO.FileMode`
- `System.IO.Stream`
- `System.IO.Path`
- `System.IO.FileInfo`
- `System.IO.Directory`
- `System.IO.MemoryStream`
- `System.IO.File`
- `System.IO.DirectoryInfo`
- `System.Linq.Enumerable`
- `System.Collections.IEnumerator`
- `System.Drawing.Imaging.ImageFormat`
- `System.Drawing.Imaging.PixelFormat`
- `System.IO.Compression.DeflateStream`
- `System.IO.Compression.CompressionMode`
- `System.Threading.Monitor`
- `System.Threading.Interlocked`
- `System.Threading.Thread`
- `System.Runtime.InteropServices.ComVisibleAttribute`
- `System.Runtime.InteropServices.GuidAttribute`
- `System.Runtime.InteropServices.Marshal`
- `System.Runtime.Versioning.TargetFrameworkAttribute`
- `System.Net.WebClient`
- `System.Net.ServicePointManager`
- `System.Net.SecurityProtocolType`
- `System.Collections.Specialized.NameValueCollection`
- `System.Text.RegularExpressions.Regex`
- `System.Text.RegularExpressions.Match`
- `System.Text.RegularExpressions.Capture`
- `System.Security.Principal.WindowsIdentity`
- `System.Security.Principal.SecurityIdentifier`
- `System.Security.Principal.IdentityReference`

### External Modules

- `kernel32.dll`
- `kernel32`

### Unmanaged Method List

- `kernel32.dll: ExitProcess, LoadLibrary, GetProcAddress, VirtualProtect, AllocConsole`
- `kernel32: GetModuleHandle, LoadLibrary, GetProcAddress`


## Network Communication

### DNS Resolutions
- `query.prod.cms.rt.microsoft.com`

### IP Traffic
- `20.99.186.246:443`
- `192.229.211.108:80`
- `184.25.191.235:443` (query.prod.cms.rt.microsoft.com)
- `23.216.147.76:443`
- `20.99.133.109:443`

## Memory Pattern Domains
- `fontfabrik.com`
- `ipapi.co`
- `www.apache.org`
- `www.carterandcone.coml` (Note: The domain seems to have a typo, should be `www.carterandcone.com`)
- `www.fontbureau.com`
- `www.fonts.com`
- `www.founder.com.cn`
- `www.galapagosdesign.com`
- `www.goodfont.co.kr`
- `www.jiyu-kobo.co.jp`
- `www.sajatypeworks.com`
- `www.sakkal.com`
- `www.sandoll.co.kr`
- `www.tiro.com`
- `www.typography.net` (Note: The domain seems to have a typo, should be `www.typography.com`)
- `www.urwpp.de` (Note: The domain seems to have a typo, should be `www.urwpp.de`)
- `www.zhongyicts.com.cn`

## Memory Pattern URLs
- `http://fontfabrik.com`
- `http://www.apache.org/licenses/LICENSE-2.0`
- `http://www.carterandcone.com`
- `http://www.carterandcone.com/designers`
- `http://www.carterandcone.com/designers/cabarga.html`
- `http://www.carterandcone.com/designers/frere-jones.html`
- `http://www.carterandcone.com/designers8`
- `http://www.carterandcone.com/designersG`
- `http://www.carterandcone.com/designers?`
- `http://www.fontbureau.com`
- `http://www.founder.com.cn/cn/bThe`
- `http://www.founder.com.cn/cn/cThe`
- `http://www.galapagosdesign.com/staff/dennis.htm`
- `http://www.goodfont.co.kr`
- `http://www.jiyu-kobo.co.jp`
- `http://www.sajatypeworks.com`
- `http://www.sakkal.com`
- `http://www.tiro.com`
- `http://www.typography.netD`
- `https://://www.urwpp.deDPlease`
- `http://www.zhongyicts.com.cn`

## Security-2.0`

- `https://://ipapi.co/ip`
- `https://ipapi.co/ip%s`
- `https://www.ipapi.co/ip`
- `https://www.zhongyicts.com.cn`

## Security-2.0`

- `https://www.zhongyicts.com.cn"
- `https://www.zhongyicts.com.cn` (Note: The domain seems to have a typo, should be `www.zhongyicts.com.cn`)



## File System Actions

### Files Opened
- `C:\Users\user\AppData\Local\Microsoft\CLR_v4.0_32\UsageLogs\TorGPT_@SamsExploit.exe.log`
- `C:\Users\user\AppData\Local\Temp\tmpDA49.tmp`
- `C:\Users\user\AppData\Local\Temp\tmpDA49.tmp\3b9e0ce4-2017-4161-ae61-37f7d58d2d9a.dll`
- `C:\Users\user\Desktop\TorGPT_@SamsExploit.exe`
- `C:\Users\user\Desktop\TorGPT_@SamsExploit.exe.config`
- `C:\WINDOWS\FONTS\AGENCYB.TTF`
- `C:\WINDOWS\FONTS\AGENCYR.TTF`
- `C:\WINDOWS\FONTS\ALGER.TTF`
- `C:\WINDOWS\FONTS\ANTQUAB.TTF`
- `C:\WINDOWS\FONTS\ANTQUAB.TTF`
- `C:\WINDOWS\FONTS\ANTQUAI.TTF`
- `C:\WINDOWS\FONTS\ARIAL.TTF`
- `C:\WINDOWS\FONTS\ARIALBD.TTF`
- `C:\WINDOWS\FONTS\ARIALBI.TTF`
- `C:\WINDOWS\FONTS\ARIALI.TTF`
- `C:\WINDOWS\FONTS\ARIALN.TTF`
- `C:\WINDOWS\FONTS\ARIALN.TTF`
- `C:\WINDOWS\FONTS\ARIALNBI.TTF`
- `C:\WINDOWS\FONTS\ARIALNI.TTF`
- `C:\WINDOWS\FONTS\ARIBLK.TTF`
- `C:\WINDOWS\FONTS\ARLRDBD.TTF`
- `C:\WINDOWS\FONTS\BAHNS93.TTF`
- `C:\WINDOWS\FONTS\BAUHS.TTF`
- `C:\WINDOWS\FONTS\BAHNS93.TTF`
- `C:\WINDOWS\FONTS\BAUHSB.TTF`
- `C:\WINDOWS\FONTS\BAUHS93.TTF`
- `C:\WINDOWS\FONTS\BAUHSB.TTF`
- `C:\WINDOWS\FONTS\BAUHS93.TTF`
- `C:\WINDOWS\FONTS\BAHNSR.TTF`
- `C:\WINDOWS\FONTS\BAUHS93.TTF`
- `C:\WINDOWS\FONTS\BELLHC.TTF`
- `C:\WINDOWS\FONTS\BELLHC.TTF`
- `C:\WINDOWS\FONTS\BOD_B.TTF`
- `C:\WINDOWS\FONTS\BOD_PSTC.TTF`
- `C:\WINDOWS\FONTS\BOOKOS.TTF`
- `C:\WINDOWS\FONTS\BOD_PSTC.TTF`
- `C:\WINDOWS\FONTS\BOOKOS.TTF`
- `C:\WINDOWS\FONTS\BOD_PSTC.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOOKOS.TTF`
- `C:\WINDOWS\FONTS\BROADW.TTF`
- `C:\WINDOWS\FONTS\BRITANic.TTF`
- `C:\WINDOWS\FONTS\BRLNSB.TTF`
- `C:\WINDOWS\FONTS\BRLNSDB.TTF`
- `C:\WINDOWS\FONTS\BRLNSR.TTF`
- `C:\WINDOWS\FONTS\BROADW.TTF`
- `C:\WINDOWS\FONTS\BRLNSB.TTF`
- `C:\WINDOWS\FONTS\CASTELAR.TTF`
- `C:\WINDOWS\FONTS\BOD_B.TTF`
- `C:\WINDOWS\FONTS\CASTELAR.TTF`
- `C:\WINDOWS\FONTS\BOD_PSTC.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BOD_R.TTF`
- `C:\WINDOWS\FONTS\BRADHITC.TTF`
- `C:\WINDOWS\FONTS\BRITANIC.TTF`
- `C:\WINDOWS\FONTS\BRLNSR.TTF`
- `C:\WINDOWS\FONTS\BRLNSR.TTF`
- `C:\WINDOWS\FONTS\BROADW.TTF`
- `C:\WINDOWS\FONTS\BROADW.TTF`
- `C:\WINDOWS\FONTS\BRUSHSCI.TTF`
- `C:\WINDOWS\FONTS\COPIA.TTF`
- `C:\WINDOWS\FONTS\COPT0.TTF`
- `C:\WINDOWS\FONTS\COMIC.TTF`
- `C:\WINDOWS\FONTS\COMICI.TTF`
- `C:\WINDOWS\FONTS\CONSOLA.TTF`
- `C:\WINDOWS\FONTS\COOPBL.TTF`
- `C:\WINDOWS\FONTS\GABRIOLA.TTF`
- `C:\WINDOWS\FONTS\GADUGI.TTF`
- `C:\WINDOWS\FONTS\GADUGIB.TTF`
- `C:\WINDOWS\FONTS\GARA.TTF`
- `C:\WINDOWS\FONTS\GARABD.TTF`
- `C:\WINDOWS\FONTS\GARAIT.TTF`
- `C:\WINDOWS\FONTS\GEORGIA.TTF`
- `C:\WINDOWS\FONTS\GEORGIAI.TTF`
- `C:\WINDOWS\FONTS\GEORGIAZ.TTF`
- `C:\WINDOWS\FONTS\GIGI.TTF`
- `C:\WINDOWS\FONTS\GILBI____.TTF`
- `C:\WINDOWS\FONTS\GIL_____.TTF`
- `C:\WINDOWS\FONTS\GILC_____.TTF`
- `C:\WINDOWS\FONTS\GILI_____.TTF`
- `C:\WINDOWS\FONTS\GLECB.TTF`
- `C:\WINDOWS\FONTS\GLSNECB.TTF`
- `C:\WINDOWS\FONTS\GOTHIC.TTF`
- `C:\WINDOWS\FONTS\GOTHICB.TTF`
- `C:\WINDOWS\FONTS\GOTHICBI.TTF`
- `C:\WINDOWS\FONTS\GOTHICI.TTF`
- `C:\WINDOWS\FONTS\GOTHICCN.TTF`
- `C:\WINDOWS\FONTS\GOTHICCN.TTF`
- `C:\WINDOWS\FONTS\GOTHICIT.TTF`
- `C:\WINDOWS\FONTS\GOTHICN.TTF`
- `C:\WINDOWS\FONTS\GOTHIC.ttf`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARNGTON.TTF`
- `C:\WINDOWS\FONTS\HARNGTON.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWD`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARNGTON.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARNGTON.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWOWI.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`
- `C:\WINDOWS\FONTS\HARLOWSI.TTF`



 ## Files Written
- `C:\Users\user\AppData\Local\Temp\tmp2B81.tmp\5198dbfb-4c95-493e-8898-39266ef039aa.dll`
- `C:\Users\user\AppData\Local\Temp\tmpDA49.tmp\3b9e0ce4-2017-4161-ae61-37f7d58d2d9a.dll`
- `C:\Users\user\AppData\Roaming`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1D66.tmp.WERInternalMetadata.xml`
- `0:\Users\user\AppData\Local\Temp\tmp2B81.tmp\5198dbfb-4c95-493e-8898-39266ef039aa.dll`

## Files Deleted
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1D66.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E50.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E80.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2382.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2392.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2393.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296C.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296D.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A47.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A58.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A87.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A88.tmp.txt`
- `C:\Windows\System32\spp\store\2.0\cache\cache.dat`
- `C:\Users\user\AppData\Local\Temp\tmpDA49.tmp`

## Files Dropped
- `%USERPROFILE%\AppData\Local\Microsoft\CLR_v4.0_32\UsageLogs\TorGPT_@SamsExploit.exe.log`
- `%USERPROFILE%\AppData\Local\Microsoft\CLR_v4.0_32\UsageLogs\c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd.exe.log`
- `%USERPROFILE%\AppData\Local\Temp\tmpE87B.tmp`
- `%USERPROFILE%\AppData\Local\Temp\tmpE87B.tmp\8a45efc6-43dc-47c5-a83e-918ad0207457.dll`
- `%USERPROFILE%\AppData\Local\Temp\tmpF57B.tmp`
- `%USERPROFILE%\AppData\Local\Temp\tmpF57B.tmp\c9768aec-9e91-4ef8-a55d-c1d878e73bf7.dll`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1D66.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1D66.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E50.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E50.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E80.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER1E80.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2382.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2382.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2392.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2392.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2393.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2393.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296C.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296C.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296D.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER296D.tmp.WERInternalMetadata.xml`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A47.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A47.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A58.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A58.tmp.csv`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A87.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A87.tmp.txt`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A88.tmp`
- `C:\ProgramData\Microsoft\Windows\WER\Temp\WER2A88.tmp.txt`
- `C:\Windows\System32\spp\store\2.0\cache\cache.dat`
- `C:\Windows\System32\spp\store\2.0\data.dat.tmp`
- `C:\Users\user\AppData\Local\Temp\tmpDA49.tmp\3b9e0ce4-2017-4161-ae61-37f7d58d2d9a.dll`

## Registry Actions

### Registry Keys Opened
- `HKEY_CURRENT_USER\EUDC\1252`
- `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework\XML`
- `HKEY_CURRENT_USER\Software\Microsoft\.NETFramework`
- `HKEY_CURRENT_USER\Software\Microsoft\CTF\DirectSwitchHotkeys`
- `HKEY_CURRENT_USER\Software\Microsoft\Fusion`
- `HKEY_CURRENT_USER\Software\Microsoft\Installer\Assemblies\C:|Users|user|Desktop|TorGPT_@SamsExploit.exe`
- `HKEY_CURRENT_USER\Software\Microsoft\Installer\Assemblies\Global`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\KnownFolders`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders`
- `HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Explorer`
- `HKEY_CURRENT_USER_Classes`
- `HKEY_CURRENT_USER_Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}`
- `HKEY_CURRENT_USER_Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Instance`
- `HKEY_CURRENT_USER_Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Instance\Disabled`
- `HKEY_CURRENT_USER_Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Namespaces`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\AppID\TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{00000323-0000-0000-C000-000000000046}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Installer\Assemblies\C:|Users|user|Desktop|TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Installer\Assemblies\Global`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{00000323-0000-0000-C000-000000000046}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\InprocHandler`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\InprocHandler32`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\InprocServer32`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\LocalServer`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\LocalServer32`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{9FC8E510-A27C-4B3B-B9A3-BF65F00256A8}\TreatAs`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Namespaces`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\Interface\{00000134-0000-0000-C000-000000000046}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\Interface\{00000134-0000-0000-C000-000000000046}\ProxyStubClsid32`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\Interface\{00000160-0000-0000-C000-000000000046}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\WOW6432Node\Interface\{00000160-0000-0000-C000-000000000046}\ProxyStubClsid32`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\Policy\Servicing`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AppModel\Lookaside\Packages`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.Accessibility__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Configuration__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Core__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Data.SqlXml__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Drawing__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Net.Http__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Numerics__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Security__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Speech__31bf3856ad364e35`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Windows.Forms__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System.Xml__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\policy.4.0.System__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.Accessibility__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Configuration__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Core__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Data.SqlXml__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Drawing__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Net.Http__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Numerics__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Security__b03f5f7f11d50a3a`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Speech__31bf3856ad364e35`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Windows.Forms__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System.Xml__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Fusion\PublisherPolicy\Default\v4.0_policy.4.0.System__b77a5c561934e089`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\LanguagePack\DataStore_V1.0`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\LanguagePack\SurrogateFallback`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\LanguagePack\SurrogateFallback\Impact`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\LanguagePack\SurrogateFallback\Microsoft Sans Serif`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\Windows.Foundation.Diagnostics.AsyncCausalityTracer`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\Windows.Foundation.Diagnostics.AsyncCausalityTracer\CustomAttributes`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\Windows.Globalization.Language`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\Windows.Globalization.Language\CustomAttributes`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\Managed\S-1-5-21-1015118539-3749460369-599379286-1001\Installer\Assemblies\C:|Users|user|Desktop|TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\Managed\S-1-5-21-1015118539-3749460369-599379286-1001\Installer\Assemblies\Global`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Appx`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\AppContext`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\Policy\APTCA`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\Policy\Standards`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\XML`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\policy\standards\v4.0.30319`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\policy\v4.0`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319\SKUs\default`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\CTF`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\CTF\Compatibility\TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Cryptography\Defaults\Provider Types\Type 001`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Cryptography\Defaults\Provider Types\Type 024`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Cryptography\Defaults\Provider\Microsoft Enhanced RSA and AES Cryptographic Provider`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Cryptography\Defaults\Provider\Microsoft Strong Cryptographic Provider`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Fusion`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\NET Framework Setup\NDP\v4\Full`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\OLE`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\OLEAUT`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\OLE\Diagnosis`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Ole\Extensions`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\FontSubstitutes`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\LanguagePack\SurrogateFallback`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}\PropertyBag`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{5E6C858F-0E22-4760-9AFE-EA3317B67173}`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{5E6C858F-0E22-4760-9AFE-EA3317B67173}\PropertyBag`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\OOBE`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Policies\Microsoft\WindowsStore`
- `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\MUI\UILanguages\en-US`
- `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Dnscache\InterfaceSpecificParameters\{44C728A6-CC3C-434D-B238-E5B6541E3476}`
- `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\{3882a85b-858a-11eb-b9e1-806e6f6e6963}`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\CustomLocale`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Segment Heap`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Policies\Microsoft\Cryptography\Configuration`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\InterfaceSpecificParameters`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\FontCache\Parameters`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NetBT\Parameters`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters\Interfaces\{3882A85B-858A-11EB-B9E1-806E6F6E6963}`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters\Interfaces\{44C728A6-CC3C-434D-B238-E5B6541E3476}`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{3882A85B-858A-11EB-B9E1-806E6F6E6963}`
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{44C728A6-CC3C-434D-B238-E5B6541E3476}`
- `HKEY_LOCAL_MACHINE\Software\Classes`
- `HKEY_LOCAL_MACHINE\Software\Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}`
- `HKEY_LOCAL_MACHINE\Software\Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Instance`
- `HKEY_LOCAL_MACHINE\Software\Classes\WOW6432Node\CLSID\{FAE3D380-FEA4-4623-8C75-C6B61110B681}\Instance\Disabled`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\COM3`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Cryptography`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Fusion`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Fusion\PublisherPolicy\Default`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Ole`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Ole\FeatureDevelopmentProperties`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Disable8And16BitMitigation`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\TorGPT_@SamsExploit.exe`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\OEM`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsRuntime`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Wow64\x86`
- `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Cryptography`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\.NETFramework`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\.NETFramework\NGen\Policy\v4.0`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\.NETFramework\Policy\`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\.NETFramework\v4.0.30319\SKUs\`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\AvalonGraphics`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Cryptography\DESHashSessionKeyBackward`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Cryptography\Offload`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\DirectWrite`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Input`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\LanguageOverlay\OverlayPackages\en-US`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\OLE`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\OLE\AppCompat`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\OLE\Tracing`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Rpc`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\StrongName`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion\AppCompatFlags`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion\GRE_Initialize`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion\ProfileList`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-1015118539-3749460369-599379286-1001`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\KnownFolderSettings`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\SideBySide`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\SideBySide\AssemblyStorageRoots`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Windows\Dwm`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\MUI\Settings`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\System\DNSClient`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\Windows NT\DnsClient`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\WindowsNT\Rpc`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\Windows\Display`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\Windows\Explorer`
- `HKEY_LOCAL_MACHINE\Software\WOW6432Node\Policies\Microsoft\Windows\Safer\CodeIdentifiers`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\ComputerName\ActiveComputerName`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\FileSystem`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa\FipsAlgorithmPolicy`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\MUI\Settings\Language`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\MUI\UILanguages`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\MUI\UILanguages\PendingDelete`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\NLS\Language`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\NLS\CustomLocale`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\NLS\ExtendedLocale`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\NLS\Sorting\Ids`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\NLS\Sorting\Versions`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SafeBoot\Option`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Srp\GP\DLL`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\StateSeparation\RedirectionMap\Keys`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\DNS`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{3882A85B-858A-11EB-B9E1-806E6F6E6963}`
- `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{44C728A6-CC3C-434D-B238-E5B6541E3476}`
- `HKEY_LOCAL_MACHINE\System\Setup`
- `HKEY_USERS.DEFAULT`
- `HKEY_USERS.DEFAULT\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders`

## Process and Service Actions

### Processes Created
- `%SAMPLEPATH%\TorGPT_@SamsExploit.exe`
- `%SAMPLEPATH%\c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd.exe`
- `C:\Windows\System32\wuapihost.exe`
- `C:\Users\user\Desktop\TorGPT_@SamsExploit.exe`

### Shell Commands
- `%SAMPLEPATH%\TorGPT_@SamsExploit.exe`
- `%SAMPLEPATH%\c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd.exe`
- `C:\Windows\System32\wuapihost.exe -Embedding`

### Processes Terminated
- `%SAMPLEPATH%\TorGPT_@SamsExploit.exe`
- `%SAMPLEPATH%\c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd.exe`
- `C:\Windows\System32\wuapihost.exe`

### Processes Tree
- `3952: explorer.exe`
  - `3228: TorGPT_@SamsExploit.exe`
  - `616: svchost.exe`
    - `2944: wuapihost.exe`
  - `1204: TorGPT_@SamsExploit.exe`

### Modules Loaded
- Runtime modules
  - `%SAMPLEPATH%\c77eabee5160b93b7f5242b351d1c3648105e9b1c567ebc1e1c005408191d9dd.exe`
  - `%USERPROFILE%\AppData\Local\Temp\tmpE87B.tmp\8a45efc6-43dc-47c5-a83e-918ad0207457.dll`
  - `%USERPROFILE%\AppData\Local\Temp\tmpF57B.tmp\c9768aec-9e91-4ef8-a55d-c1d878e73bf7.dll`

> [!NOTE]
> ![](https://profile-counter.glitch.me/TorGPT-Scam/count.svg)

