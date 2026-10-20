# Log4Shell Security Audit

## Advisory

Official GitHub Security Advisory details for Log4Shell (CVE-2021-44228):

- **GHSA ID:** GHSA-jfh8-c2jp-5v3q
- **Affected package:** `org.apache.logging.log4j:log4j-core`
- **Severity:** Critical (CVSS 3.1 score 10.0, vector `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H`)
- **Patched version:** 2.16.0

The advisory, titled "Remote code injection in Log4j", states that Log4j versions
prior to 2.16.0 are subject to a remote code execution vulnerability via the LDAP
JNDI parser. An attacker who can control log messages or log message parameters
can execute arbitrary code loaded from LDAP servers when message lookup
substitution is enabled. From Log4j 2.16.0, this behavior has been disabled by
default.

Note that version 2.15.0 contained an earlier fix, but that patch did not disable
attacker-controlled JNDI lookups in all situations; 2.16.0 is the version at which
the package is considered patched. Additional backports of the fix are available
in versions 2.3.1, 2.12.2, and 2.12.3. Only the `log4j-core` package is directly
affected; `log4j-api` should be kept at the same version as `log4j-core` if in use.

Advisory URL: https://github.com/advisories/GHSA-jfh8-c2jp-5v3q

## CI evidence

The audit workflow (`.github/workflows/audit.yml`) was dispatched on the default
branch and completed successfully. The following line was captured directly from
the job's log text:

> AUDIT-PASSED

- **Workflow run ID:** 39383517501
- **Workflow run URL:** https://github.com/mcpmark-eval-liuhezi/log4shell-audit/actions/runs/39383517501
- **Run conclusion:** success
- **Job name:** verify (job ID 118497875807)
