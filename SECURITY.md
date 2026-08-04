# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in RVTMA, please report it responsibly.

**Do NOT open a public GitHub issue for security vulnerabilities.**

Instead, please email the maintainer directly or use GitHub's private vulnerability reporting feature.

## Scope

RVTMA processes RVTools Excel exports which may contain sensitive infrastructure data (hostnames, IP addresses, MAC addresses, storage configurations). The application:

- Runs entirely client-side in the browser (no data is sent to external servers)
- When running in a container, PDF generation processes documents locally
- No telemetry, analytics, or external API calls are made
- All data stays on the user's machine or within their container environment

## Supported Versions

Only the latest minor line gets security fixes. Older lines are best-effort, no commitment.

| Version | Supported |
|---------|-----------|
| 2.0.x   | Yes       |
| 1.x     | No        |

## Best Practices for Users

- Do not expose the RVTMA container to the public internet without authentication
- RVTools exports contain sensitive infrastructure data - handle them accordingly
- When sharing RVTMA-generated reports, review for sensitive data before distribution
