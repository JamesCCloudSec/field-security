# Advanced Malware Beaconing Incident Response

**Analyst:** James Castro
**Date:** June 2025
**Repository Purpose:** Real-world documentation of a persistent malware incident and full response lifecycle. Includes logs, detection methodology, tooling, and resolution.

---

## Executive Summary

In June 2025, I identified and neutralized a stealth malware infection exhibiting beaconing behavior over outbound TLS connections. This incident was not a simulation. It bypassed Windows Defender (MpDefenderCoreService), evaded detection for multiple days, and operated via cloaked processes without executable paths. The malware was uncovered through my own custom telemetry and confirmed using PowerShell and Wireshark. The infection's scope included persistence across reboots, avoidance of standard AV signatures, and usage of unique IPs per beacon to avoid IP-based blacklisting.

---

## Initial Suspicion

**Tool Used:** Wireshark

I began analyzing network activity for a personal project and identified abnormal `RST, ACK` behavior and persistent outbound TCP connections to port 443 with no DNS resolution records. A sample log:

```
Frame 78: 54 bytes on wire (432 bits)
Src: 192.168.1.233 → Dst: 3.233.158.50
TCP 49973 → 443 [RST, ACK] Seq=3 Ack=24 Win=0 Len=0
[Conversation completeness: Incomplete (60)]
```

**Initial reflection:**
"What’s crazy is I just wanted to mess around with Wireshark at first."

---

## Detection Methodology

### Step 1: Wireshark Filters

Used a series of Wireshark display filters:

* `ip.addr == 142.251.46.238`
* `tcp.flags.reset == 1`
* `tcp.port == 443`
* `!(dns)` to identify non-DNS outbound connections

### Step 2: PowerShell Monitor

Created a real-time PowerShell script to log and flag all new 443 connections, extracting:

* PID
* Process name
* Executable path
* SHA256 hash (for VirusTotal lookup)

Example output:

```
🚨 New outbound 443 connection detected!
IP: 204.79.197.222
PID: 14304
Process: msedgewebview2
Path: C:\Program Files (x86)\Microsoft\EdgeWebView\...
```

> "It rarely uses the same IP twice."
>
> "Nothing came up with DNS and ip.addr == 142.251.46.238"

---

## Malware Behavior Profile

**Red Flags Identified:**

* Processes with **no executable path**
* Executables running from unusual directories (AppData, Temp)
* Signed by unknown or no vendor
* `MpDefenderCoreService` making beacon connections with no visible payload or traffic volume

**PowerShell Log Excerpt:**

```
[CRITICAL] New 443 Connection
IP: 23.44.73.253
PID: 8640
Process: svchost
Path: 
Threat Score: 5
```

```
[CRITICAL] New 443 Connection
IP: 13.74.138.55
PID: 8640
Process: svchost
Path: 
Threat Score: 5
```

**Quote:**

> "It had to be a beacon, right?"

---

## Behavior of IP 142.251.46.238

This address was first flagged by Wireshark during a stream marked `Complete, WITH_DATA`, yet it had no DNS resolution, no associated process path, and triggered a full TLS handshake over port 443 without user activity. It was repeatedly flagged by custom PowerShell telemetry.

> "Nothing came up with DNS and ip.addr == 142.251.46.238"

> "The address is sending me loads of data too"

Despite repeated monitoring, it evaded traditional detection mechanisms.

---

## Containment & Eradication

### Steps Taken:

1. **Disabled networking** immediately upon pattern recognition
2. **Disconnected all LAN systems and switch connections**, including a shared network with another PC

   > "I disconnected them both and then wiped them again, just to cover that attack vector."
3. **Full disk wipe** using Windows install media
4. **Deleted all partitions**, including 10MB+ unlabeled segments and 12GB encrypted leftovers
5. **Clean Windows reinstall**, configured with:

   * Secure Boot enabled
   * BitLocker enabled
   * No restored user data except Stardew Valley saves
6. **USB and external drives scanned** via offline, trusted machines

   > "Well I just wiped everything twice... deleted and wiped all partitions, reinstalled Windows. If it survived that well it earned it."

---

## Post-Incident Hardening

* Rotated all passwords and enabled 2FA on:

  * Bitwarden
  * Email
  * Financial services
* Modified bootloader config
* Verified BIOS for tampering
* Implemented continuous PowerShell logging and connection scoring system:

```powershell
Threat Score = Missing Path + Unsigned + Non-standard Dir + Suspicious Proc Name
```

> "I think everything is okay. Changed Bitwarden. 2FA on everything important. Stardew Valley save isn’t corrupted and functional."
>
> "Fresh installs. Fresh partitions. No connection via switch on wipe."

---

## Lessons Learned

* **Real malware does not need flashy payloads** — beaconing and data profiling are quiet but critical
* **AV is not a catch-all**; the infection ran undetected for days
* **Even security tools and cracked installers** can be plausible vectors
* **Network forensics is often the only source of truth**

---

## Attribution & Speculation

**Possible attack vectors:**

> "It’s either from random EXEs I used or Wii U downloader. Gotta be."

* FitGirl repack (though downloaded from official site)
* Wii U Downloader (flagged by AV heuristics)
* Unknown tool or "clean" movie file from MegaThread

> "Could’ve been Wii U downloader but thousands of people claim it’s a false positive..."

**Unlikely to be:**

> "VLC-opened MKV files (scanned, single playback, clean hash)"
>
> "Detected/removed cracked game payload (never ran)"

---

## Confidence Level and Limitations

While it is **extremely likely** that this was a beaconing malware infection, the following factors make it impossible to fully confirm:

* No binary samples or forensic memory dumps were captured before eradication
* Encrypted TLS traffic prevented packet-level payload inspection
* System was wiped before any sandboxing or reverse engineering could occur
* No persistence artifacts or registry hooks were recovered
* BIOS or firmware integrity was not verified via third-party imaging tools

Despite this, the convergence of all observed behaviors — rotating destination IPs, absent executable paths, stealth execution under `svchost`, and zero DNS footprint — make the beaconing malware conclusion highly credible.

---

## Outcome

* System fully restored with no signs of reinfection
* No loss of data
* All secure accounts preserved
* APT-like behavior halted

> "I need a security job doing stuff like this. I was both nervous and excited to deal with the infection."
>
> "If the threat still exists at this point or does any damage — man, I tip my hat to them."
