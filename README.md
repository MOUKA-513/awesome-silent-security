# Awesome Silent Security (Noise‑Free) [![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)

> A curated collection of tools, techniques, and resources for mastering **stealth tradecraft** – moving, communicating, and persisting without creating noise in telemetry, logs, or detection stacks.

**Silent security** is the art of operating below the radar. Whether you’re a red teamer conducting a covert engagement, a purple teamer emulating a sophisticated adversary, or a defender trying to understand how the quietest threats evade your tools – this list is for you.

We focus on **low‑and‑slow**, **OPSEC‑first**, and **noise‑free** methodologies. While not exclusive, this list heavily favours free/open‑source projects and practical tradecraft over commercial “stealth” products.

Your contributions, corrections, and discoveries are warmly welcomed. Please read the [Contributing Guidelines](CONTRIBUTING.md) before submitting PRs.  
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

## Contents

- [C2 Frameworks for Stealth](#c2-frameworks-for-stealth)
- [EDR & AV Evasion](#edr--av-evasion)
- [Logging & Telemetry Evasion](#logging--telemetry-evasion)
- [Linux Stealth & Rootkits](#linux-stealth--rootkits)
- [Memory & Process Evasion](#memory--process-evasion)
- [Network Evasion](#network-evasion)
- [Offensive Tooling (Noise‑Free)](#offensive-tooling-noise-free)
- [Silent Persistence](#silent-persistence)
- [Living Off the Land (LOLBins & Scripts)](#living-off-the-land-lolbins--scripts)
- [OPSEC & Tradecraft Guides](#opsec--tradecraft-guides)
- [Detection Engineering for Stealth](#detection-engineering-for-stealth)
- [Miscellaneous & Curiosities](#miscellaneous--curiosities)

## C2 Frameworks for Stealth

Frameworks designed with built‑in evasion, malleable traffic, and low footprint.

- [Havoc](https://github.com/HavocFramework/Havoc) – Modern, malleable post‑exploitation C2. Its `Demon` agent features sleep obfuscation (Ekko), return address spoofing, and indirect syscalls.
- [NyxC2](https://github.com/nyxgeek/nyxc2) – Lightweight, asynchronous C2 demonstrating basic evasion via XOR string obfuscation and dynamic API loading – great for learning.
- [Covenant](https://github.com/cobbr/Covenant) – .NET‑based C2 with dynamic compilation and out‑of‑process execution to reduce memory footprint.
- [Sliver](https://github.com/BishopFox/sliver) – Cross‑platform implant framework with support for multiple C2 protocols (MTLS, WireGuard, DNS) and session masking.

## EDR & AV Evasion

Tools and techniques to bypass user‑land hooks, kernel callbacks, and behavioural sensors.

- [EDRChoker](https://github.com/Sh0ckFR/EDRChoker) – Abuses Windows QoS policies to throttle EDR network telemetry – no code injection, no process kill.
- [Magnetar](https://github.com/EspressoCake/Magnetar) – Advanced shellcode loader using direct syscalls, ETW/AMSI patching, and Early Bird APC injection.
- [Artemis](https://github.com/t3l3machus/Artemis) – “Syscall Forge” that dynamically discovers syscall numbers and generates direct/indirect stubs.
- [BYOVD Library](https://github.com/daem0nc0re/VectorKernel) – Collection of vulnerable driver exploits to disable EDR agents from kernel mode.
- [Syswhispers2](https://github.com/jthuraisamy/SysWhispers2) – Generates header/ASM files to invoke direct syscalls, bypassing EDR hooks.

## Logging & Telemetry Evasion

Suppressing, tampering, or blinding logging subsystems (Windows Event Log, ETW, syslog, auditd).

- [SharpEvtMute](https://github.com/mattifestation/SharpEvtMute) – Tampers with Windows event logging to suppress or modify log entries.
- [Evading Logging & Monitoring](https://github.com/3gstudent/Evading-Logging-And-Monitoring) – Collection of PowerShell scripts and C# code for bypassing PowerShell logging and ETW (GPO cache bypass, native ETW patching).
- [wevtutil](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil) – Built‑in Windows tool to clear event logs (`wevtutil cl`).
- [etw‑patch](https://github.com/forrest-orr/etw-patch) – Minimal, in‑memory patching of ETW functions to blind providers.
- [auditd‑evasion](https://github.com/linux-audit/audit-userspace/issues) – Techniques to disable or flood local auditd (e.g., rate‑limiting bypass).

## Linux Stealth & Rootkits

Kernel‑level and user‑land stealth for *nix systems.

- [Curing Rootkit](https://github.com/hfiref0x/Curing) – PoC rootkit that abuses `io_uring` to operate outside the traditional syscall interface, bypassing eBPF monitors.
- [Diamorphine](https://github.com/m0nad/Diamorphine) – LKM rootkit for Linux 2.6/3.x/4.x – hides processes, files, and gives root backdoor.
- [Reptile](https://github.com/f0rb1dd3n/Reptile) – Advanced LKM rootkit with persistence, reverse shell, and full process/port hiding.
- [ebpf‑evasion](https://github.com/pathtofile/ebpf-escape) – Research on manipulating eBPF telemetry pipelines from a privileged context.

## Memory & Process Evasion

Executing payloads without touching disk, evading memory scanners, and process injection alternatives.

- [Process Injection Techniques](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1055/T1055.md) – The classic T1055 family – Process Doppelgänging, Hollowing, APC injection.
- [Fileless Execution](https://github.com/samratashok/nishang) – PowerShell reflective loading and `Invoke-ReflectivePEInjection` (Nishang).
- [SuperSploit](https://github.com/SuperSploit/SuperSploit) – Runs Python modules or Linux binaries directly in RAM via `memfd_create` – no filesystem traces.
- [Donut](https://github.com/TheWover/donut) – Generates position‑independent shellcode from .NET assemblies, VBScript, and more.

## Network Evasion

Evading network detection (IDS/IPS, DPI, egress filtering) and anonymising C2 traffic.

- [Geneva (Genetic Evasion)](https://github.com/Kkevsterrr/geneva) – Genetic algorithm that evolves packet manipulation strategies to bypass national‑level DPI.
- [Malleable C2 Profiles](https://github.com/rsmudge/Malleable-C2-Profiles) – Cobalt Strike profiles that randomise traffic patterns and mimic legitimate protocols.
- [DNS over HTTPS (DoH) Tunnels](https://github.com/ameshkov/dnscrypt) – Use DoH for covert C2 channels (e.g., `dnscrypt-proxy`).
- [ICMP Exfiltration](https://github.com/ajpc500/icmp_exfil) – Simple ICMP payload tunneling – often overlooked by firewalls.
- [MTLS Fingerprint Randomisation](https://github.com/BishopFox/sliver) – Sliver’s ability to spoof JA3/S signatures.

## Offensive Tooling (Noise‑Free)

Standalone tools that prioritise low footprint, minimal dependencies, and OPSEC.

- [Blackglass Suite](https://github.com/blackglass/blackglass) – Offline‑first payload generation (QR codes, HTA, LNK droppers) with a full PowerShell library for persistence and exfiltration.
- [Red‑Teaming‑Toolkit](https://github.com/t3l3machus/Red-Teaming-Toolkit) – Collection of open‑source tools for recon, evasion, and persistence.
- [PowerShell without Logging](https://github.com/PowerShellMafia/PowerSploit) – Use `-Exec Bypass` and reflection to minimise PowerShell logs.
- [NimlineWhispers](https://github.com/ajpc500/NimlineWhispers) – Direct syscall generation for Nim, producing very small, evasive binaries.

## Silent Persistence

Establishing long‑term access without triggering common persistence detectors.

- [WMI Event Subscription](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Persistence.md#wmi) – Permanent WMI event filters – often missed by scanners.
- [Scheduled Tasks Obfuscation](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1053/T1053.md) – Using randomised task names and non‑admin triggers.
- [COM Hijacking](https://github.com/outflanknl/COM-Hijacking) – Abusing CLSID registry entries for stealthy autostart.
- [Linux crontab @reboot](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Persistence.md) – Simple but effective; combine with rootkit to hide the entry.

## Living Off the Land (LOLBins & Scripts)

Using built‑in OS tools to avoid dropping custom binaries.

- [LOLBAS Project](https://lolbas-project.github.io/) – The definitive database of Windows binaries that can be used for proxy execution, download, and bypass.
- [GTFOBins](https://gtfobins.github.io/) – Unix/Linux binaries that can be exploited to bypass local security restrictions.
- [WMI for Lateral Movement](https://github.com/ankh2054/wmi-pentest) – Using `wmic` and `Invoke-WmiMethod` for silent remote execution.
- [PowerShell Remoting without Logging](https://github.com/Arno0x/PowerShellC2) – Custom PSRemoting sessions with downgraded logging.

## OPSEC & Tradecraft Guides

Articles, books, and methodologies to master stealth.

- [Red Team OPSEC – The Manual](https://www.offensive-security.com/offsec/red-team-opsec-manual/) – OffSec’s guide to operational security.
- [Evading EDR – The Definitive Guide](https://posts.specterops.io/evading-edr-the-definitive-guide-c3c0b8b1a8b9) – SpecterOps series on bypassing modern EDRs.
- [Silent Cylinder](https://silentcylinder.com/) – Blog focused on Windows internals and evasion.
- [Awesome OPSEC](https://github.com/FourCoreLabs/awesome-opsec) – Another curated list (complementary) with operational security links.
- [MITRE ATT&CK – Defense Evasion](https://attack.mitre.org/tactics/TA0005/) – The canonical framework for evasion tactics (T1027, T1055, T1562, etc.).

## Detection Engineering for Stealth

How to catch what you’re trying to hide – for blue teamers who want to understand and defend against silent tradecraft.

- [Sigma Rules for Stealth](https://github.com/SigmaHQ/sigma/tree/master/rules/windows/builtin/win_evasion) – Pre‑written Sigma rules covering process injection, syscall abuse, and log tampering.
- [Sysmon Evasion Detection](https://github.com/SwiftOnSecurity/sysmon-config) – High‑quality Sysmon config that flags typical evasion behaviours.
- [ETW Monitor](https://github.com/google/etw-monitor) – Real‑time ETW event collection to detect providers being patched.
- [Auditd Hardening](https://github.com/Neo23x0/auditd) – Rules to catch kernel module loading, fileless execution, and `memfd_create`.

## Miscellaneous & Curiosities

Oddities, proof‑of‑concepts, and research that doesn’t fit neatly elsewhere.

- [Holy Ghost](https://github.com/nickvourd/HolyGhost) – In‑memory PE loader that bypasses `PsSetCreateProcessNotifyRoutine` callbacks.
- [Shhhloader](https://github.com/icyguider/Shhhloader) – Syscall‑only shellcode loader with polymorphic obfuscation.
- [Threadless Injection](https://github.com/CCob/ThreadlessInject) – New injection technique that doesn’t create new threads.
- [Unkillable](https://github.com/RedTeamOperations/Unkillable) – Process protection against termination via kernel callbacks.

## License

[![CC-BY](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by.svg)](https://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
