# Scenario 003 – Wi-Fi Connected but No Internet

## Overview

**Ticket:** Wi-Fi connected, but websites and Microsoft Teams cannot connect.

**User:** Jessica Brown – Marketing Department  
**Device:** MKT-LT-022 – Windows 11 Laptop  
**Network:** CORP-WIFI  
**Priority:** Normal (single user affected)

### Objective

Practise diagnosing a Windows laptop that connects to corporate Wi-Fi but cannot access network resources or the internet.

The main learning objective is to identify DHCP-related problems, gather evidence, perform safe troubleshooting, and recognise when to escalate.

---

## 1. Troubleshooting Workflow

Follow these steps in order. Don't jump randomly between possible solutions.

### Step 1 – Gather information

Ask the user:

- When did the problem start?
- Was everything working previously?
- Is anyone else experiencing the same issue?
- Does the problem affect all applications or just one?
- Were any updates or configuration changes recently installed?

**Goal:** Establish the scope and identify possible recent changes.

### Step 2 – Compare with a working device

If possible, ask a colleague nearby to connect to the same Wi-Fi network.

- Does their connection work?
- Are they using the same network?
- Is the issue isolated to one device?

**Goal:** Determine whether the problem is widespread or device-specific.

### Step 3 – Inspect IP configuration

Open Command Prompt:

```cmd
ipconfig /all
```

Check:

- IPv4 Address
- Subnet Mask
- Default Gateway
- DHCP Enabled
- DHCP Server
- DNS Servers

**Important:**

An address beginning with `169.254.x.x` is an APIPA address.

This usually means Windows failed to obtain an IPv4 address from DHCP.

A missing default gateway is another important clue.

### Step 4 – Test DHCP renewal

Run:

```cmd
ipconfig /release
ipconfig /renew
```

Observe the result.

If Windows reports that it cannot contact the DHCP server, record the error.

Don't assign a random static IP address.

### Step 5 – Reconnect to Wi-Fi

Disconnect from the wireless network and reconnect.

Confirm:

- Correct SSID
- Connected status
- Strong signal
- Whether internet access has returned

If the problem persists, continue investigating.

### Step 6 – Inspect the wireless connection

Run:

```cmd
netsh wlan show interfaces
```

Check:

- State
- SSID
- BSSID
- Signal
- Radio type
- Authentication

A connected state and strong signal confirm wireless association, but do not prove that DHCP or internet connectivity is working.

### Step 7 – Inspect the network adapter

Open Device Manager:

1. Expand Network adapters.
2. Locate the wireless adapter.
3. Open Properties.
4. Check Device status.
5. Open the Driver tab.
6. Record provider, date and version.

Don't roll back, uninstall or update drivers without a reason.

### Step 8 – Check Windows Event Viewer

Open:

```cmd
eventvwr.msc
```

Navigate to:

`Applications and Services Logs → Microsoft → Windows → DHCP-Client → Admin`

Look for DHCP-related warnings or errors around the time the problem started.

Record relevant events. Don't clear logs or change settings.

---

## 2. Understanding the Results

| Finding | What it suggests |
|---|---|
| 169.254.x.x address | DHCP lease was not obtained |
| Missing default gateway | No usable route to other networks |
| DHCP renewal timeout | DHCP communication failed |
| Strong Wi-Fi signal | Wireless signal is probably not the issue |
| Colleague connects successfully | General network outage is less likely |
| DHCP event errors | Further DHCP investigation is justified |

**Remember:** These findings narrow down the problem. They do not independently prove the root cause.

---

## 3. When to Stop Troubleshooting

This is one of the most important Tier 1 skills.

Stop and consider escalation when:

- You've completed the appropriate basic checks.
- You've identified a likely area of failure.
- Further investigation requires administrator or infrastructure access.
- Additional changes could affect security or network availability.
- You have enough evidence for another team to investigate.

Ask yourself:

**"What am I trying to prove or eliminate with my next action?"**

If you cannot answer that question, pause before proceeding.

Never experiment with corporate infrastructure.

---

## 4. Escalation

For this scenario, the likely area of investigation is DHCP connectivity or device-specific network authorization.

The network team should investigate:

- DHCP server logs.
- Whether requests from the device are reaching the DHCP server.
- Wireless authentication and access-control logs.
- NAC policies and device registration.
- Access point connectivity and forwarding.

Possible causes include device-specific authorization issues, DHCP infrastructure problems, or wireless network configuration issues.

The exact root cause remains unconfirmed until the network team investigates.

---

## 5. Ticket Documentation

### Example ticket notes

**Issue:** User connected to corporate Wi-Fi but unable to access websites or Microsoft Teams.

**Findings:**
- Other employees unaffected.
- Colleague successfully connected to the same SSID.
- Wireless adapter reported connected with 92% signal.
- IPv4 address: 169.254.84.23.
- Default gateway: Missing.
- DHCP renewal failed with timeout.
- DHCP-related Event Viewer errors observed.
- No obvious adapter warning in Device Manager.
- Recent Windows cumulative update identified.

**Actions performed:**
- Gathered user symptoms and scope.
- Inspected IP configuration.
- Attempted DHCP release and renewal.
- Reconnected to Wi-Fi.
- Inspected wireless interface and adapter driver.
- Reviewed DHCP event logs.

**Outcome:** Not resolved.

**Next action:** Escalated to network team for DHCP, wireless authentication and device authorization investigation.

---

## 6. Important Commands

| Command | Purpose |
|---|---|
| `ipconfig /all` | Inspect IP configuration |
| `ipconfig /release` | Release current DHCP lease |
| `ipconfig /renew` | Request a new DHCP lease |
| `netsh wlan show interfaces` | Inspect wireless connection |
| `eventvwr.msc` | Open Windows Event Viewer |

---

## 7. Lessons Learned

- Connected to Wi-Fi does not necessarily mean connected to the network.
- APIPA is an important indicator of DHCP failure.
- A missing gateway is a significant clue.
- DNS should not be the first suspect when a valid IP configuration is missing.
- Comparing a working device with an affected device helps isolate the problem.
- Event Viewer provides useful diagnostic evidence.
- Don't make changes without understanding their purpose.
- Escalation is a legitimate and important Tier 1 outcome.

### Final Reflection

The goal of a Help Desk technician is not to try every possible fix.

It is to investigate methodically, make safe decisions, document findings, and know when to escalate.

**A good technician follows the evidence, not guesses.**
