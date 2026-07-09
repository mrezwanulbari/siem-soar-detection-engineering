# SIEM/SOAR Detection Engineering

Detection-as-code and SOAR automation reference for enterprise security operations — platform-agnostic detection logic (portable across Splunk, Microsoft Sentinel, and QRadar) paired with automated response playbooks.

## Why This Exists

Detection rules that live only inside one vendor's proprietary query language become dead weight the moment the org migrates SIEM platforms. This repository writes detection logic in a portable, Sigma-compatible format first, with platform-specific translation as a secondary step — so the detection engineering investment survives platform changes.

## What's Inside

| Area | Description |
|---|---|
| `detections/brute-force-login-detection.yml` | Sigma-format detection rule for authentication brute-force patterns, with tuning notes for false-positive reduction |
| `detections/` *(expanding)* | Additional correlation rules added as the library grows |
| `soar-playbooks/phishing-response-automation.md` | SOAR automation playbook for phishing report triage and response |
| `soar-playbooks/` *(expanding)* | Additional automation playbooks for common SOC workflows |

## Detection Engineering Approach

1. **Write once, in Sigma format** — vendor-neutral detection logic
2. **Translate per platform** — Splunk SPL, Sentinel KQL, or QRadar AQL as needed
3. **Document false-positive tuning** — every rule ships with known noise sources and suggested exclusions, not just the raw logic
4. **Map to MITRE ATT&CK** — every detection references the technique(s) it covers for coverage tracking

## Who This Is For

Detection engineers building or migrating a correlation rule library, SOC teams wanting SOAR playbooks that reduce analyst toil on repetitive triage, and security architects evaluating SIEM platform migrations who need to understand what detection logic actually needs to move.

## Status

Actively maintained — this started as a platform-agnostic detection engineering practice built across enterprise SIEM deployments and continues to grow.

## About the Author

Maintained by Shakil Md. Rezwanul Bari, Cyber Security Engineer with 17+ years of experience in SIEM/SOAR (Splunk, QRadar, Microsoft Sentinel), detection engineering, and security operations. Connect on [LinkedIn](https://www.linkedin.com/in/rezwanulbari/).

## License

MIT — see [LICENSE](LICENSE).
