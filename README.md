# Awesome Silent Security (Noise‑Free) [![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)

![Cover Image](cover.png)

> **Operate below the radar.**
> A curated collection of tools, techniques, and knowledge for mastering stealth tradecraft – moving, persisting, and exfiltrating without generating noise in telemetry, logs, or detection stacks.

**Silent security** is the art of invisibility. Whether you're a red teamer conducting a covert engagement, a purple teamer emulating a sophisticated threat, or a defender learning how the quietest adversaries evade your sensors – this list is for you.

Every entry favours free/open‑source projects, low‑and‑slow methodologies, and **OPSEC‑first** design. Commercial "stealth" products are excluded.

Contributions that improve signal quality are warmly welcomed. Please read the [Contributing Guidelines](CONTRIBUTING.md) before opening a PR.

---

## Contents

- [C2 Frameworks for Stealth](#c2-frameworks-for-stealth)
- [EDR & AV Evasion](#edr--av-evasion)
- [Logging & Telemetry Suppression](#logging--telemetry-suppression)
- [Linux Kernel & Userland Stealth](#linux-kernel--userland-stealth)
- [Memory & Process Evasion](#memory--process-evasion)
- [Network Evasion](#network-evasion)
- [Offensive Tooling (Noise‑Free)](#offensive-tooling-noise-free)
- [Covert Persistence](#covert-persistence)
- [Living Off the Land (LOLBins & Scripts)](#living-off-the-land-lolbins--scripts)
- [OPSEC & Tradecraft Guides](#opsec--tradecraft-guides)
- [Detection Engineering for Stealth](#detection-engineering-for-stealth)
- [Curiosities & Research](#curiosities--research)
- [Contributing](#contributing)
- [License](#license)

---

## C2 Frameworks for Stealth
Frameworks engineered from the ground up to reduce forensic footprint, randomise network indicators, and avoid in‑memory signatures.

- [Havoc](https://github.com/HavocFramework/Havoc) – Modern post‑exploitation framework with the malleable `Demon` agent. Features sleep obfuscation (Ekko), return address spoofing, indirect syscalls, and a full team server UI.
- [Sliver](https://github.com/BishopFox/sliver) – Cross‑platform implant framework by Bishop Fox. Supports MTLS, WireGuard, HTTP(S), and DNS with JA3/JA4S fingerprint randomisation, session migration, and operator‑side multi‑player.
- [Mythic](https://github.com/its-a-feature/Mythic) – Highly modular C2 with a Docker‑based architecture. Agents (Apollo, Athena, Merlin, etc.) support custom crypto, dynamic loading, and flexible transport options.
- [Cobalt Strike Malleable C2 Profiles](https://github.com/rsmudge/Malleable-C2-Profiles) – The definitive collection of profiles that reshape Beacon traffic to mimic legitimate protocols. Study these to understand how to blend into enterprise networks.
- [Nimbo-C2](https://github.com/frkngksl/Nimbo-C2) – Lightweight, asynchronous C2 written in Nim, demonstrating advanced obfuscation, encrypted comms, and minimal process footprint.

---

## EDR & AV Evasion
Techniques and instruments that bypass user‑land hooks, kernel callbacks, and behavioural detection engines without crashing the process.

- [Magnetar](https://github.com/0xjrx/magnetar) – Feature‑rich shellcode loader with direct syscalls, ETW/AMSI patching, Early Bird APC injection, and process protection via security descriptor modification.
- [EDRChoker](https://github.com/TwoSevenOneT/EDRChoker) – Throttles EDR telemetry by leveraging Windows QoS policies. No process injection, no driver loading, and no suspicious memory operations.
- [Artemis](https://github.com/0x3xp/Artemis) – "Syscall Forge" that dynamically discovers syscall numbers and generates fresh direct/indirect stubs, evading static syscall tables.
- [BYOVD Library (VectorKernel)](https://github.com/daem0nc0re/VectorKernel) – Curated collection of vulnerable signed drivers and accompanying exploits to disarm EDR agents from kernel mode.
- [SysWhispers3](https://github.com/klezVirus/SysWhispers3) – Generates header/ASM files for direct syscalls. Supports egg‑hunting and indirect syscall modes, bypassing hooked NTDLL functions.
- [ScareCrow](https://github.com/optiv/ScareCrow) – Payload generation framework that produces DLL side‑loading, ETW/AMSI patching, and syscall‑based shellcode execution artifacts.

---

## Logging & Telemetry Suppression
Silencing, tampering, or disabling logging subsystems without triggering obvious alerts like event log clearing.

- [EvtMute](https://github.com/bats3c/EvtMute) – Applies YARA‑like filters to the Windows Event Log pipeline *before* events are written. Includes `SharpEvtMute.exe` for C# integration.
- [BOF-patchit](https://github.com/ASkyeye/BOF-patchit) – All-in-one Cobalt Strike BOF to patch, check, and revert AMSI & ETW in memory, leaving no disk artifacts.
- [Phant0m](https://github.com/hlldz/Phant0m) – Technique that locates and kills the Event Log service thread instead of clearing logs, avoiding the canonical 1102 event.
- [ETW-Patch](https://github.com/EvilBytecode/ETW-Patch) – Minimal CGO code snippet demonstrating how to patch `EtwEventWrite` in ntdll.dll, effectively blinding multiple ETW providers.
- [SilenceSysmon](https://github.com/mattifestation/SilenceSysmon) – PoC that manipulates Sysmon’s internal pipe to prevent event delivery, demonstrating a subtle approach to blinding a key sensor.

---

## Linux Kernel & Userland Stealth
Rootkits, eBPF‑based backdoors, and file/process hiding that resist modern monitoring stacks.

- [Diamorphine](https://github.com/m0nad/Diamorphine) – Stable LKM rootkit for Linux 2.6–4.x. Hides processes, files, sockets, and provides a root backdoor with a magic prefix.
- [TripleCross](https://github.com/h3xduck/TripleCross) – eBPF rootkit that uses `bpf_probe_write_user` and map tampering to hide processes, files, and network connections without loading a kernel module.
- [Curing (io_uring rootkit)](https://github.com/armosec/curing) – Research rootkit that abuses Linux's `io_uring` interface to bypass syscall hooks, evading even eBPF‑based monitors.
- [KoviD](https://github.com/carloslack/KoviD) – Lightweight LKM rootkit that hides files, processes, and provides a backdoor, written with modern kernel compatibility in mind.
- [Reptile (Archived fork)](https://github.com/fengjixuchui/linux-rootkits/tree/main/Reptile) – Historic but instructive LKM rootkit with reverse shell, file/process hiding, and port knocking. Useful for studying kernel stealth.

---

## Memory & Process Evasion
Executing payloads without touching disk, avoiding memory scans, and using injection techniques that minimise suspicious thread creation.

- [Donut](https://github.com/TheWover/donut) – Generates position‑independent shellcode from .NET assemblies, VBScript, JScript, and more, ready for in‑memory execution.
- [Shhhloader](https://github.com/icyguider/Shhhloader) – Syscall‑only shellcode loader with polymorphic obfuscation and built‑in anti‑sandbox/anti‑analysis features.
- [ThreadlessInject](https://github.com/CCob/ThreadlessInject) – Novel injection technique that executes shellcode by hijacking an existing thread’s execution context, never calling `CreateRemoteThread`.
- [Process Injection Techniques (Atomic Red Team T1055)](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1055/T1055.md) – Run‑ready tests for Doppelgänging, Hollowing, APC injection, and more – invaluable for understanding what defenders are watching.
- [TartarusGate](https://github.com/trickster0/TartarusGate) – Implements a hardware breakpoint‑based unhooking method to bypass user‑land hooks without patching.

---

## Network Evasion
Polymorphic protocols, fingerprint randomisation, and traffic manipulation to bypass IDS/IPS and deep packet inspection.

- [Geneva (Genetic Evasion)](https://github.com/Kkevsterrr/geneva) – Genetic algorithm that automatically discovers packet manipulation strategies to evade national‑level DPI systems.
- [ICMP Exfiltration Toolkit](https://github.com/PolEspurnes/ICMP-Exfiltration-Scripts) – Python and PowerShell scripts for covert file transfer over ICMP, designed to blend with normal host network behaviour.
- [ProxyCannon](https://github.com/proxycannon/proxycannon) – Rapidly deploys disposable, multi‑region proxy chains in cloud environments to diversify egress IPs and complicate geolocation tracking.
- [Sliver Fingerprint Randomisation](https://github.com/BishopFox/sliver) – Highlighted for its ability to randomise TLS client hello signatures (JA3/JA4S) and session metadata, a critical network‑level evasion technique.
- [Malleable C2 Profiles (Cobalt Strike)](https://github.com/rsmudge/Malleable-C2-Profiles) – Includes highly customisable HTTP, HTTPS, and DNS profiles that mimic real applications like Microsoft Updates or Google Play.

---


## Offensive Tooling (Noise‑Free)
Standalone tools with minimal dependencies, small binary size, and built‑in OPSEC awareness.

- [Blackglass Suite](https://github.com/GnomeMan4201/Blackglass_Suite) – Offline‑first payload generation platform (QR codes, HTA, LNK droppers) accompanied by an extensive PowerShell library.
- [NimlineWhispers](https://github.com/ajpc500/NimlineWhispers) – Direct syscall generation for Nim, producing tiny Windows executables that are extremely difficult to reverse and signature.
- [EvilTree](https://github.com/t3l3machus/eviltree) – Stealthy directory tree reconstruction via native NT APIs, completely bypassing standard `FindFirstFile/FindNextFile` hooks.
- [Freeze](https://github.com/optiv/Freeze) – Suspends and resumes processes in a manner that evades EDR user‑land hooks, useful for payload staging.
- [PowerShell without Event Logs](https://github.com/Arno0x/PowerShellC2) – Techniques for reflective loading and custom PowerShell remoting sessions with downgraded logging.

---

## Covert Persistence
Long‑term access methods that avoid the top‑10 most‑monitored persistence locations.

- [WMI Event Subscription (Permanent)](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Persistence.md#wmi) – WMI event filters and consumers that trigger silently, often missed by automated scanners and even some EDRs.
- [COM Hijacking](https://github.com/outflanknl/COM-Hijacking) – Abusing CLSID registry entries to launch payloads via legitimate COM object loading, blending into normal system activity.
- [Scheduled Task Obfuscation](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1053/T1053.md) – Techniques for creating tasks with randomised names, custom triggers, and encoded command lines to defeat rule‑based detection.
- [Linux @reboot cron + Rootkit](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Persistence.md) – Pair a simple cron `@reboot` entry with a rootkit that hides the crontab line, making the persistence invisible to admins.

---

## Living Off the Land (LOLBins & Scripts)
Proxy execution, lateral movement, and data exfiltration using only built‑in operating system tools.

- [LOLBAS Project](https://lolbas-project.github.io/) – Comprehensive database of Windows binaries, libraries, and scripts that can be abused for execution, download, and UAC bypass.
- [GTFOBins](https://gtfobins.github.io/) – Unix/Linux binaries that can be exploited to escalate privileges, bypass security restrictions, or execute commands under the radar.
- [WMI for Silent Lateral Movement](https://github.com/ankh2054/wmi-pentest) – Tools and techniques for `wmic` and `Invoke-WmiMethod` based lateral movement with minimal event log noise.
- [PowerShell Remoting with Log Downgrade](https://github.com/Arno0x/PowerShellC2) – Custom PSRemoting sessions that disable command logging and module loading, leaving far fewer traces.

---

## OPSEC & Tradecraft Guides
Mindset, planning frameworks, and deep technical dives that separate tools from true tradecraft.

- [Red Team OPSEC – The Manual](https://www.offensive-security.com/offsec/red-team-opsec-manual/) – OffSec’s structured methodology for identifying operational risks and planning counter‑surveillance.
- [Evading EDR – SpecterOps Series](https://posts.specterops.io/evading-edr-the-definitive-guide-c3c0b8b1a8b9) – The definitive guide to understanding modern EDR internals and the most effective bypass strategies.
- [Silent Cylinder Blog](https://silentcylinder.com/) – Deep dives into Windows internals, process manipulation, and advanced stealth techniques.
- [MITRE ATT&CK – Defense Evasion (TA0005)](https://attack.mitre.org/tactics/TA0005/) – The canonical taxonomy of adversary evasion. Map your TTPs against it to identify detection gaps.

---

## Detection Engineering for Stealth
What defenders are looking for – and what you must avoid to stay silent.

- [Sigma Rules](https://github.com/SigmaHQ/sigma) – 3,000+ community‑maintained detection rules for SIEMs. Studying these reveals exactly which behaviours trigger alerts.
- [SigmaOptimizer](https://github.com/YusukeJustinNakajima/SigmaOptimizer-UI) – LLM‑assisted tool that generates and optimises Sigma rules, offering a sneak peek at emerging detection logic.
- [EventTrace](https://github.com/fafalone/EventTrace) – ETW‑based file activity monitor that exposes low‑level I/O operations, useful for testing your own evasion.
- [auditd Hardening Rules](https://github.com/Neo23x0/auditd) – Florian Roth's ruleset catches kernel module loading, `memfd_create` fileless execution, and other Linux stealth techniques.

---

## Curiosities & Research
Experimental techniques, PoCs, and niche projects that push the boundaries of stealth.

- [Threadless Injection (Concept)](https://github.com/CCob/ThreadlessInject) – Demonstrates that shellcode can run entirely within an existing thread, leaving no new thread creation artifacts.
- [NyxC2](https://github.com/korakino/NyxC2) – Minimal, asynchronous C2 written in Python; illustrates XOR string obfuscation and dynamic API loading in a clean codebase.
- [Hiding Linux Processes from eBPF](https://github.com/h3xduck/TripleCross) – TripleCross’s method of tampering with eBPF maps and probes represents the cutting edge of kernel‑level stealth.
- [Magnetar Process Protection](https://github.com/0xjrx/magnetar#process-protection) – Using security descriptor modification to prevent other processes (including EDR) from opening or terminating your implant.

---

## Contributing

Your additions, corrections, and improvements are what keep this list sharp.  
Before submitting a pull request, please:

1. Check for duplicates – we favour quality over quantity.
2. Make sure the tool or resource is actively maintained and fits the “noise‑free” philosophy.
3. Use clear, concise descriptions that explain *why* the entry is silent.
4. Follow the existing formatting and alphabetical order within sections (where reasonable).

See the full [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## License

[![CC BY 4.0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by.svg)](https://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
