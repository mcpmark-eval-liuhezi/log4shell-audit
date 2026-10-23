# Advisory Summary — Log4Shell

Reference summary of the GitHub Security Advisory Database entry for Log4Shell, prepared for the security review postmortem packet.

## Key facts

- **GHSA ID:** GHSA-jfh8-c2jp-5v3q
- **CVE:** CVE-2021-44228
- **Severity:** Critical (CVSS 3.1 score 10.0, CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H)
- **Package:** `org.apache.logging.log4j:log4j-core` (Maven)
- **Vulnerable version range:** any Log4j version prior to v2.15.0 (versions before 2.16.0 remain exposed to follow-on JNDI lookup behavior)
- **First patched version:** 2.16.0
- **Advisory URL:** https://github.com/advisories/GHSA-jfh8-c2jp-5v3q

## Impact note

In our own words: Log4Shell allowed any attacker who could get a crafted string into a logged message — whether direct user input, HTTP headers, exception traces, or authentication failure logs — to make the logging library perform a JNDI lookup against a server the attacker controlled and load attacker-supplied code into the JVM. The practical impact is trivially exploitable remote code execution against any internet-facing service that logged untrusted data, with no authentication or user interaction required, which is why the advisory is rated critical with a perfect CVSS score. The maintainers' guidance is that the only complete fix is 2.16.0, which disables JNDI lookups by default and removes message lookups entirely; the interim 2.15.0 release was insufficient in certain recursive-lookup scenarios, and Log4j 1.x is end-of-life and should not be considered a mitigation.

## Remediation pointers

- Upgrade `org.apache.logging.log4j:log4j-core` to 2.16.0 or later; keep `log4j-api` at the same version.
- Backports exist for 2.3.1, 2.12.2, and 2.12.3 where a full upgrade is not immediately possible.
- Log4j 1.x is EOL and vulnerable to other RCE vectors; plan migration rather than patching.
