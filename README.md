# Dayz-Hub RDP Blocker

**Windows desktop tool that automatically blocks RDP brute-force attacks** by monitoring failed Remote Desktop logins and adding Windows Firewall rules to block abusive IPs. Ideal for game servers, dev machines, and any PC exposed to the internet over RDP.

---

## What it does

- **Monitors** the Windows Security event log for failed RDP logon events (Event ID 4625, Logon Type 10).
- **Auto-blocks** IPs that exceed a configurable number of failed login attempts by creating **Windows Firewall** inbound block rules for the RDP port.
- **Whitelists** trusted IPs so they are never auto-blocked and can be given explicit firewall Allow rules.
- Provides a **simple UI** to view failed logins, manage blocked IPs, and adjust settings.

Run as **Administrator** for block/unblock and whitelist actions; the app can still run without admin to view data (auto-block won’t apply until elevated).

---

## Features

| Feature | Description |
|--------|-------------|
| **Auto-block after N failures** | Configurable threshold (1–100); IPs exceeding it are blocked via firewall. |
| **Custom RDP port** | Works with default 3389 or any custom port (e.g. 3366). |
| **Whitelist** | Add IPs that must never be auto-blocked; optional firewall Allow rules for them. |
| **Failed logins grid** | Shows IP, attempt count, last attempt time, and target username. |
| **Blocked IP list** | View all currently blocked IPs; unblock selected with one click. |
| **Manual block** | Block by IP, block selected rows from failed logins, or “block all” above threshold. |
| **Copy & whitelist** | Right-click to copy IPs or add failed-login IPs to the whitelist. |
| **Activity log** | Log panel shows firewall commands and results. |
| **Auto-start** | Optional startup with Windows (Registry Run key). |
| **Launch minimised** | Option to start minimised when using auto-start. |

---

## Requirements

- **Windows** (uses Windows Security event log and Windows Firewall).
- **.NET Framework 4.7.2** or later.
- **Run as Administrator** for blocking, unblocking, and whitelist firewall rules.

---

## Usage

1. **Run as Administrator** (right-click exe → “Run as administrator”) so the app can add/remove firewall rules.
2. Open **Settings** to set:
   - Failed login threshold (e.g. 5 attempts).
   - RDP port (default 3389 or your custom port).
   - Auto-start with Windows and launch minimised (optional).
3. The main window refreshes periodically: failed RDP logins appear in the grid; IPs above the threshold are auto-blocked unless whitelisted.
4. Use **Whitelist** to add trusted IPs so they are never auto-blocked.
5. Use **Unblock** to remove blocks; you can optionally add an IP to the whitelist when unblocking.

---

## How blocking works

The app uses **PowerShell** (with admin rights) to manage Windows Firewall:

- **Block:** `New-NetFirewallRule` with display name `RDPBLOCK <ip>`, Inbound, TCP, your RDP port, Action Block, RemoteAddress = that IP.
- **Unblock:** Removes the rule with that display name.
- **Whitelist:** Creates an Allow rule `RDP WHITELIST <ip>` for the same port so the IP is explicitly allowed.

Block rules take precedence for that IP; whitelist rules are used so trusted IPs are explicitly allowed.

---

## License

Use and modify as needed. No warranty; use at your own risk.
VIRUS SCANS
https://virusscan.jotti.org/en-US/filescanjob/2h84uko6sp
https://www.virustotal.com/gui/file/3cffa11e2c9fdfc4c229b089250c2a6e6ed87598dda3ec6768c8ba5c433e304f/detection
