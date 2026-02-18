# Black Duck SCA → Copilot Agent Remediation Demo

This project demonstrates an automated vulnerability remediation pipeline that bridges **Black Duck Software Composition Analysis (SCA)** with the **GitHub Copilot coding agent**.

## How It Works

```
Black Duck SCA Scan → GitHub Issues → Copilot Coding Agent → Pull Requests
```

1. **Vulnerability Detection** — A Black Duck SCA scan identifies known vulnerabilities in project dependencies.
2. **Issue Creation** — A GitHub Actions workflow parses the scan results and creates a structured GitHub Issue for each vulnerability, including CVE details, severity, affected component, and the recommended fix version.
3. **Agent Assignment** — Each issue is automatically assigned to the Copilot coding agent (`copilot`), which picks up the issue and submits a pull request to update the vulnerable dependency.

## Project Structure

```
├── package.json                          # Demo Node.js app with intentionally vulnerable dependencies
├── .github/
│   ├── mock-data/
│   │   └── blackduck-vulns.json          # Simulated Black Duck scan output (3 CVEs)
│   └── workflows/
│       └── sca-to-agent.yml              # GitHub Actions workflow: scan → issues → agent
└── README.md
```

## Vulnerable Dependencies (Intentional)

| Package        | Pinned Version | CVE               | Severity | Fixed Version |
|----------------|---------------|-------------------|----------|---------------|
| `lodash`       | 4.17.20       | CVE-2021-23337    | HIGH     | 4.17.21       |
| `axios`        | 0.21.1        | CVE-2023-45857    | CRITICAL | 1.6.0         |
| `jsonwebtoken` | 8.5.1         | CVE-2022-23529    | HIGH     | 9.0.0         |

## Running the Demo

1. **Trigger the workflow** — Go to **Actions → SCA Vulnerability → Issue → Agent Remediation → Run workflow** and select `mock` as the data source.
2. **Issues are created** — The workflow reads the mock Black Duck data and opens one GitHub Issue per vulnerability, labeled with `sca`, the severity, the CVE ID, and `agent-remediation`.
3. **Copilot agent remediates** — Each issue is assigned to the Copilot coding agent, which analyzes the issue body and metadata, then opens a pull request bumping the dependency to the fixed version in `package.json`.

## Extending for Production

The workflow includes a placeholder for calling the real **Black Duck API**. To integrate with a live scan:

- Replace the mock data step with an API call to your Black Duck server.
- Store your Black Duck API token as a GitHub Actions secret.
- Change the `scan_source` input to `blackduck` when triggering the workflow.
