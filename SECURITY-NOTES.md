# Log4Shell Security Notes (CVE-2021-44228)

## Summary

Log4Shell is a critical remote-code-execution vulnerability in Apache Log4j 2, exploitable via JNDI lookup features. An attacker who can control log messages or log message parameters can force Log4j to load and execute arbitrary code from an attacker-controlled LDAP (or other JNDI) server.

## Core Facts

- **CVE identifier:** CVE-2021-44228
- **Affected versions (Log4j 2):** `[2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0)` — i.e., 2.0-beta9 through 2.14.1, excluding the security backports 2.3.1, 2.12.2, and 2.12.3
- **Fixed versions:** 2.15.0 (Java 8 and later); 2.12.2 (Java 7); 2.3.1 (Java 6)
- **Fully remediated release:** 2.16.0 (and later 2.17.1 / 2.12.4 / 2.3.2 for subsequent related CVEs) — from 2.16.0 the JNDI lookup functionality is completely removed
- **Severity:** CVSS 3.1 Base Score **10.0 — CRITICAL** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H)
- **Affected component:** `log4j-core` only (the `log4j-api` JAR is not impacted)
- **Discovery:** Chen Zhaojun of the Alibaba Cloud Security Team

## Remediation Actions

1. **Upgrade path:** Upgrade `org.apache.logging.log4j:log4j-core` to 2.15.0 or later; 2.16.0+ is recommended as it fully removes JNDI lookup functionality. Java 7 users: 2.12.2; Java 6 users: 2.3.1.
2. **Disable JNDI lookups (temporary mitigation):** Set system property `log4j2.formatMsgNoLookups=true` (or env var `LOG4J_FORMAT_MSG_NO_LOOKUPS=true`) for releases >= 2.10. For releases >= 2.7 and <= 2.14.1, use `%m{nolookups}`, `%msg{nolookups}`, or `%message{nolookups}` in pattern layouts. For releases >= 2.0-beta9 and < 2.7, remove the `JndiLookup` class from the classpath: `zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class`.
3. **Detecting vulnerable deployments:** Identify all deployments shipping log4j-core versions in the affected ranges; scan application jars and containers; check for `JndiLookup` class presence; review log patterns for unbounded `%m`/`%msg` usage.
4. **Automated dependency scanning:** Integrate SCA tooling (e.g., OWASP Dependency-Check, Dependabot, Snyk, Trivy) to detect vulnerable Log4j versions in the dependency graph and fail builds on CVE-2021-44228.

## Sources

- Apache Logging Services security page: https://logging.apache.org/security.html
- NVD CVE-2021-44228: https://nvd.nist.gov/vuln/detail/cve-2021-44228
- GitHub Advisory Database (GHSA-JFH8-C2JP-5V3Q): https://github.com/advisories/ghsa-jfh8-c2jp-5v3q
- Apache Log4j 2 issue LOG4J2-3214 (mitigation guidance): https://issues.apache.org/jira/browse/LOG4J2-3214
- CISA conocido exploited vulnerabilities context: https://www.cisa.gov/strategic-partnerships/known-exploited-vulnerabilities-catalog
