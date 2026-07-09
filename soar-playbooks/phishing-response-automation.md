# SOAR Playbook: Phishing Report Triage & Response

**Trigger:** User-reported phishing email (via report button) or automated detection (SEG/email security alert)
**Goal:** Reduce analyst manual triage time on the ~80% of reports that are low-risk or duplicate, while surfacing genuine threats fast.

## Automated Workflow

### Stage 1 — Enrichment (automated, no analyst involvement)
1. Extract IOCs from the reported email: sender address, sender domain, URLs, attachment hashes
2. Query threat intel sources for each IOC (reputation score, known-malicious status, first-seen date)
3. Check if the sender domain/URL has appeared in other reports within the past 30 days (campaign correlation)
4. Compute a preliminary risk score based on: threat intel hits, campaign correlation match, presence of credential-harvesting indicators (login-page keywords, brand impersonation), attachment file type risk

### Stage 2 — Auto-Triage Decision
| Risk Score | Action |
|---|---|
| Low (known-safe sender, no IOC hits, single report) | Auto-close with notification to reporting user — no analyst time spent |
| Medium (some IOC hits or first-time sender with suspicious characteristics) | Queue for analyst review with enrichment data pre-attached |
| High (known-malicious IOC match, credential harvesting indicators, or matches an active campaign) | Auto-escalate to high-priority queue AND trigger Stage 3 containment actions immediately, analyst notified in parallel rather than waiting on review |

### Stage 3 — Automated Containment (High-risk only, auto-triggered)
1. Search and purge the matching email from all mailboxes it was delivered to (not just the reporter's)
2. Block the sender domain/URL at the email security gateway
3. If credential-harvesting indicators are present, check authentication logs for the reporting user and any other recipients for logins matching the phishing page's timeframe — flag any hits for immediate account containment
4. Open a formal incident ticket with all enrichment data attached, pre-populated for analyst review

### Stage 4 — Analyst Actions (human-in-the-loop)
- [ ] Review auto-triage decision and enrichment data
- [ ] Confirm or override the automated risk classification
- [ ] For high-risk cases: verify containment actions completed successfully, check for any recipients who interacted with the email before containment
- [ ] Close out with disposition (confirmed phishing / false positive / spam-not-phishing) — this disposition feeds back into the detection tuning loop

## Why Automate This Specifically

Phishing report volume is high and mostly low-signal — automating enrichment and auto-closing the clearly-safe reports is where SOAR delivers the most analyst time back, without touching the judgment calls that still need a human (campaign attribution, user account risk decisions, executive/VIP-targeted phishing).

## Metrics to Track

- Mean time to containment for high-risk cases (target: under 15 minutes from report to purge/block)
- Percentage of reports auto-closed without analyst involvement
- False-positive rate on auto-triage classification (review monthly, retune thresholds)

---
*Part of the [siem-soar-detection-engineering](../README.md) repository.*
