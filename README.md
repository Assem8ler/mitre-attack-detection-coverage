# 🎯 MITRE ATT&CK Detection Coverage for Wazuh SIEM

Systematic Detection Engineering project: assess current detection coverage,
identify gaps, and engineer custom Wazuh detection rules mapped to MITRE ATT&CK.

## 📋 Project Overview

Most SOC teams deploy a SIEM and stop there. This project answers a different question:
**"Which attacker techniques can we actually detect — and which ones slip through silently?"**

Methodology:
1. **Assess** — map current detection coverage (from my previous labs)
2. **Prioritize** — select high-prevalence techniques detectable via Windows telemetry
3. **Engineer** — write custom Wazuh detection rules
4. **Validate** — test each rule with Atomic Red Team / native Windows tools
5. **Document** — record evidence, findings and lessons learned

## 🏗️ Lab Environment

| Role | System |
|------|--------|
| SIEM | Wazuh Manager (Amazon Linux 2023) |
| Endpoint | Windows 10/11 + Wazuh Agent + Sysmon |
| Attacker | Kali Linux / Atomic Red Team |

## 📊 Coverage Status

| Status | Count | Notes |
|--------|-------|-------|
| ✅ Detected (previous labs) | 4 | T1558.003, T1550.002, T1046, T1110 |
| ✅ Validated (this project) | 6 | T1070.001, T1059.001, T1053.005, T1547.001, T1003.001, T1021.002 |
| 🎯 Planned | 19 | see roadmap below |

## 🗺️ Technique Roadmap

| ID | Technique | Status |
|----|-----------|--------|
| T1070.001 | Indicator Removal: Clear Windows Event Logs | ✅ Done |
| T1059.001 | Command & Scripting: PowerShell | ✅ Done |
| T1053.005 | Scheduled Task | ✅ Done |
| T1547.001 | Registry Run Keys | ✅ Done |
| T1003.001 | LSASS Memory | ✅ Done |
| T1021.002 | SMB/Windows Admin Shares | ✅ Done |

## 📁 Structure

- `rules/` — custom Wazuh detection rules (XML), grouped by tactic
- `documentation/` — per-technique analysis & test results
- `screenshots/` — alert evidence from Wazuh Dashboard

## 🚀 How to deploy these rules

To use these detection rules in your own Wazuh environment:

**Step 1:** Copy the XML files from the `rules/` directory to the rules folder on your Wazuh Manager:

    sudo cp rules/*/*.xml /var/ossec/etc/rules/

**Step 2:** Ensure the rules are included in your Wazuh configuration. If you are using a custom file (e.g., `mitre_coverage.xml`), add the following to `/var/ossec/etc/ossec.conf` inside the `<ruleset>` block:

    <rule_dir>/var/ossec/etc/rules/</rule_dir>

**Step 3:** Validate the XML syntax before restarting to avoid manager crashes:

    sudo /var/ossec/bin/wazuh-logtest

**Step 4:** Restart the Wazuh Manager to apply the new rules:

    sudo systemctl restart wazuh-manager
    sudo systemctl status wazuh-manager
    
## ⚠️ Disclaimer

All testing is performed in an isolated home lab environment for educational purposes.

## 🔗 Related Projects

- [Active Directory Purple Team Lab](https://github.com/Assem8ler/active-directory-purple-team-lab)
- [Wazuh SIEM Fundamentals](https://github.com/Assem8ler/wazuh-siem-fundamentals)
