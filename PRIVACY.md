# Privacy Policy — SHADOW

**Publisher:** Advance Your IT
**Product:** SHADOW (Microsoft Edge extension)
**Last updated:** August 20, 2026
**Contact:** shadow@advanceyourit.nl

## 1. Overview

SHADOW is an administrative productivity extension for Microsoft Edge that extends the NinjaOne RMM (Remote Monitoring and Management) web console with in-page tools for exporting/importing scripts and custom fields, comparing and syncing data with a user-configured GitHub or Azure DevOps repository, running backup/compliance workflows, and generating policy/asset reports. SHADOW is an independent, community-built tool and is not affiliated with or endorsed by NinjaOne, Microsoft, GitHub, or Microsoft Azure DevOps.

SHADOW is designed to operate entirely within the user's own NinjaOne tenant and the third-party services the user explicitly configures. It does not send data to Advance Your IT or to any server operated by the publisher, and it contains no analytics, telemetry, or advertising code.

## 2. Information We Access

Depending on which SHADOW features are enabled, the extension may access:

- **NinjaOne data**: scripts, policies, custom fields, devices, organizations, technician roles/permissions, activity logs, and related configuration data, retrieved via NinjaOne's own web-app session/API using the credentials of the logged-in NinjaOne technician. SHADOW never handles or stores the user's NinjaOne login credentials — it relies on the existing, already-authenticated browser session.
- **Repository access tokens**: a GitHub Personal Access Token and/or an Azure DevOps Personal Access Token, only if the user configures those integrations in the SHADOW popup.
- **Repository contents**: script, policy, and report files read from or written to the GitHub or Azure DevOps repository the user configures, for backup, compare, and sync features.
- **Locally generated content**: user-configured settings (e.g., repository configuration, backup options, RBAC allowlist), UI state, and cached report/backup data produced from the sources above.

SHADOW does not access browsing history, other websites, or any data outside of the configured NinjaOne domains and the GitHub/Azure DevOps repositories the user explicitly connects.

## 3. How Information Is Used

All data accessed by SHADOW is used exclusively to power the feature the user actively invokes, such as:

- Displaying reports and overlays inside the NinjaOne console (e.g., policy override audits, asset reports, technician permissions reports).
- Performing actions the user explicitly triggers, such as backing up or syncing scripts, policies, and custom fields between NinjaOne and the user's GitHub or Azure DevOps repository, or comparing NinjaOne configuration against a repository copy.

SHADOW does not use this data for advertising, profiling, or any purpose other than the administrative workflow the user runs.

## 4. How Information Is Stored

- Repository tokens, settings, and cached report/backup data are stored **locally in the browser** (`chrome.storage`), scoped to the user's own browser profile and NinjaOne tenant.
- No data is transmitted to, or stored on, servers owned or operated by Advance Your IT.
- Data flows directly between the user's browser, the NinjaOne domains the user is logged into, and the GitHub/Azure DevOps repository the user has configured.

## 5. Third-Party Services

SHADOW communicates with the following hosts, only as required by the feature in use:

| Service | Purpose | Data involved |
|---|---|---|
| NinjaOne web app (`*.ninjarmm.com`, `*.rmmservice.*` regional domains) | Core functionality (scripts, policies, custom fields, devices, reports) | NinjaOne tenant data, existing session cookies/tokens |
| GitHub API (`api.github.com`, `raw.githubusercontent.com`) | Backup, compare, and sync of scripts/policies/custom fields | Repository contents, GitHub Personal Access Token |
| Azure DevOps (`dev.azure.com`, `*.visualstudio.com`) | Backup, compare, and sync of scripts/policies (alternative to GitHub) | Repository contents, Azure DevOps Personal Access Token |

Each of these services has its own privacy policy governing how it handles data once received. SHADOW acts only as a client connecting the user's browser to services the user already has accounts with, and only one backup destination (GitHub or Azure DevOps) is active at a time, as selected by the user.

SHADOW does not load or execute any remote code: all extension logic ships inside the extension package, and no `<script>` tag, module, or `eval()` call references externally hosted JavaScript or WebAssembly.

## 6. Data Sharing and Sale

We do not sell, rent, or share user data with third parties for advertising or marketing purposes. SHADOW does not include any analytics, tracking, or advertising code.

## 7. Data Retention and Deletion

- Locally stored tokens, settings, and cached data persist only within the user's browser profile until the user clears them or uninstalls the extension.
- Uninstalling SHADOW removes all locally stored extension data from the browser.
- Because SHADOW does not store data on external servers, there is no separate deletion request process — clearing browser storage or uninstalling the extension is sufficient.

## 8. Security

- Tokens are stored using the browser extension storage APIs and are not exposed to web pages outside the extension's own context.
- All communication with NinjaOne, GitHub, and Azure DevOps occurs over HTTPS.

## 9. Children's Privacy

SHADOW is a professional IT administration tool intended for use by IT administrators and is not directed at, or knowingly used by, children under 16.

## 10. Changes to This Policy

This policy may be updated as SHADOW's functionality changes. Material changes will be reflected in the "Last updated" date above and, where applicable, in the extension's release notes.

## 11. Contact

Questions about this policy or SHADOW's data handling can be directed to:
- Email: shadow@advanceyourit.nl
