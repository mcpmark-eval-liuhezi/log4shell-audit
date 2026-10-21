# Log4Shell Audit Findings

Working notes for the Log4Shell (CVE-2021-44228) security review follow-up.

## 1. GitHub Security Advisory (CVE-2021-44228, log4j-core)

- **GHSA ID:** GHSA-jfh8-c2jp-5v3q
- **CVE:** CVE-2021-44228
- **Summary:** Remote code injection in Log4j
- **Severity:** Critical (CVSS 3.1 score 10.0 — `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H`)
- **Affected package:** `org.apache.logging.log4j:log4j-core` (only this package is directly affected; `log4j-api` should be kept at the same version if in use)
- **First version that counts as patched:** **2.15.0** — the advisory states "Any Log4J version prior to v2.15.0 is affected to this specific issue." Backport fixes also shipped in 2.3.1, 2.12.2 and 2.12.3.
- **Advisory's updated guidance:** Log4j versions prior to **2.16.0** are subject to the RCE via the LDAP JNDI parser; 2.15.0 contained an earlier fix but did not disable attacker-controlled JNDI lookups in all situations. From 2.16.0 the behaviour is disabled by default. Log4j 1.x is EOL and will not receive patches.
- **Published:** 2021-12-10 (last updated 2025-10-22)
- **Advisory URL:** https://github.com/advisories/GHSA-jfh8-c2jp-5v3q
- **Source:** https://github.com/apache/logging-log4j2

## 2. Notifications

Could not be read: the notifications API returned
`403 Resource not accessible by personal access token`.
No notification count, repository or subject list could therefore be retrieved, and no security-related notifications could be identified.

## 3. Gists

- Gists returned: **none** (empty list). No public gists existed on the account before this audit.
- Zero stale/old gists to clean up.

## 4. CI billing reference

- **Repository:** actions/checkout
- **Workflow:** "Build and Test" (`.github/workflows/test.yml`, id 3160)
- **Most recent finished run:** run id 39024106845, run number 1852, event `pull_request`, branch `dependabot/npm_and_yarn/brace-expansion-1.1.21`, conclusion `success`, started 2026-10-17
  https://github.com/actions/checkout/actions/runs/39024106845
- **Billable duration (as returned by the usage data):** `run_duration_ms` = **105000** milliseconds
  (per-runner billable `total_ms` values were 0 for UBUNTU/6 jobs, MACOS/1 job, WINDOWS/1 job)

## 5. Community reports (top three GitHub issue search hits for CVE-2021-44228)

1. **lgeer16/vulnerability-analysis-portfolio** — issue #1, title "CVE-2021-44228" — **open**
2. **snasser8/vulnerability-analysis-portfolio** — issue #1, title "CVE-2021-44228" — **open**
3. **mcpmark-eval-liuhezi/log4shell-remediation** — issue #1, title "Upgrade log4j-core: CVE-2021-44228" — **open**

(Search returned 3,935 total matching issues; all three top hits were still open, i.e. tracked but not closed out.)
