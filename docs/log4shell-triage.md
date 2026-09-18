# Log4Shell Advisory Triage Report

This report documents the Log4Shell vulnerability (CVE-2021-44228) for the security review of our Java services that still depend on Log4j. All facts below were pulled live from the GitHub Advisory Database — version ranges in this advisory have been revised over time, so these values reflect what the advisory database returns today.

## Advisory facts

| Field | Value |
| --- | --- |
| GHSA ID | GHSA-jfh8-c2jp-5v3q |
| CVE | CVE-2021-44228 |
| Severity | Critical |
| CVSS 3.1 | 10.0 — `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H` |
| Summary | Remote code injection in Log4j |
| Affected package | Maven: `org.apache.logging.log4j:log4j-core` |
| Advisory URL | https://github.com/advisories/GHSA-jfh8-c2jp-5v3q |

## Vulnerable version ranges and first patched versions

As returned by the advisory database for `org.apache.logging.log4j:log4j-core`:

| Vulnerable version range | First patched version |
| --- | --- |
| >= 2.13.0 | 2.15.0 |
| >= 2.0-beta9 | 2.3.1 |
| >= 2.4 | 2.12.2 |

Per the advisory: any Log4j version prior to v2.15.0 is affected by this specific issue. Version 2.15.0 contained an earlier fix, but that patch did not disable attacker-controlled JNDI lookups in all situations.

## Remediation

- The advisory recommends updating to 2.16.0 where possible: it disables JNDI by default and completely removes support for message lookups.
- Additional backports of this fix are available in versions 2.3.1, 2.12.2, and 2.12.3.
- Only `org.apache.logging.log4j:log4j-core` is directly affected; `org.apache.logging.log4j:log4j-api` should be kept at the same version as `log4j-core` to ensure compatibility if in use.
- Log4j v1 is End Of Life, will not receive patches for this issue, and is vulnerable to other RCE vectors — migrate to 2.16.0 where possible.

## Tracking

This documentation is tracked in issue #1 ("Log4Shell advisory triage"): https://github.com/mcpmark-eval-liuhezi/log4shell-audit/issues/1
