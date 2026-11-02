# Security policy

## Supported versions

Only the latest release receives fixes.

## Reporting a vulnerability

Please report vulnerabilities privately through GitHub's
[private vulnerability reporting](https://github.com/discreteRegister/ha-stagg-ekg-plus/security/advisories/new)
(Security > Report a vulnerability), not in a public issue.

## Automated checks

- **Vulnerable dependencies:** Dependabot alerts and security updates; a daily
  `uv audit` of `uv.lock` (`.github/workflows/vulnerability-scan.yml`) reported
  under Security > Code scanning.
- **Code:** CodeQL code scanning (Python and GitHub Actions).
- **Workflows:** zizmor and actionlint on every pull request (the "Workflow
  security" job).
- **Secrets:** secret scanning with push protection.

The integration itself has no third-party requirements beyond Home Assistant;
the locked dependencies are the development and test tools.
