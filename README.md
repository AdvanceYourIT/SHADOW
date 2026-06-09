# SHADOW – Stealthy Handler for Automated Data Optimization and Workflow

SHADOW is a Microsoft Edge browser extension that supercharges NinjaOne administrators with powerful, secure tools for managing, comparing, and migrating scripts and monitoring policies. SHADOW integrates directly into the NinjaOne web interface and connects seamlessly with your private GitHub repository for version control and change auditing.

<img width="128" height="128" alt="shadow_128x128" src="https://github.com/user-attachments/assets/5f28a902-69eb-47b7-86d0-d0263103a35b" />

*Disclaimer: This is a private, independent project and is in no way officially affiliated with, endorsed by, or sponsored by NinjaOne, Inc.*

Use at your own risk.

---

## Support

For questions, feedback, or feature requests, please open an issue in this repository or contact the maintainer.

Or visit me in Discord! https://discord.gg/FuS6nTQft

---

## **Version 1.1.7 released!**

- **Import from Public GitHub (ShadowGitSync).** New Template Library button that updates NinjaOne scripts directly from a raw GitHub URL placed in the script's description (`ShadowGitSync:<url>`), using a preview-first diff overlay and code-only updates that preserve every other field.
- **Backup choice before sync updates.** Applying a sync update now shows a single dialog with three explicit choices — **Back up, then update**, **Update without backup**, or **Cancel** — and automatically detects when a full backup was already made today so you can skip a redundant one.
- **New Script Usage overlay.** A single **Script Usage** button on the Automation page shows where each script runs across policies, scheduled tasks, and system tray configurations, and flags scripts that are not referenced anywhere — combining what were previously the separate Used Scripts and Unused Scripts buttons into one view. Improved filtering lets you live-search by name or ID independently on the Used and Unused tabs, with large reference lists collapsed behind expandable summaries.
- **New Script Manager (Mass Script Management).** A **Script Manager** button on the Automation page opens a single overlay listing every script (metadata only), with live filtering by name/description/ID plus Language, OS, Architecture, and Category. Select scripts (or select-all) and **bulk add or remove a category** in one click — additions never clobber a script's existing categories. An optional **Edit mode** allows inline renaming and description editing, with a guard that warns before a description change would drop a `ShadowGitSync:` marker. Scripts open in a new tab so the filtered view stays put, and native scripts are correctly limited to category-only changes.
- **Descriptions reserved for sync tags.** Imports and template syncs no longer auto-write or overwrite script descriptions, leaving the field free for the `ShadowGitSync:` tag.
- **New Custom Field Manager.** A **Custom Field Manager** button on the Device, Organization, and Location Custom Fields pages opens a single filterable table of every custom field and its **Technician / Script / API** permissions. An optional **Edit mode** lets you change those permissions plus each field's description, footer text, and tooltip text inline, then **Apply** writes all pending changes in one batched, throttled pass with automatic MFA detection — every other property of each field is preserved (full-replace per field).
- **New Import from Private GitHub button.** Alongside **Import from Public GitHub** (the renamed ShadowGitSync button), a new **Import from Private GitHub** button on the Template Library page browses and bulk-imports scripts from your token-authenticated private repository — multi-select with category selection and progress feedback.
- **New custom-field Usage report.** A **Usage** button on the Device, Organization, and Location Custom Fields pages scans every script's code for custom-field reads/writes (`Get`/`Set-NinjaProperty`, `Ninja-Property-*`, `ninjarmm-cli`, and more) and cross-references them against the defined fields. The two-tab report (By Field / By Script) surfaces defined-but-unused fields, used-but-undefined names, and read/write conflicts with a field's Script permission, with CSV and JSON export.
- **New Bulk Set Values for organization fields.** A **Bulk Set Values** button on the Organization Custom Fields page sets one organization custom field's value across all or selected organizations at once. It reads each org's current values and writes back only the chosen field — every other value is left untouched — with a type-aware input for text, checkbox, number, date, dropdown, and multi-select fields.
- **OBVIOUS Template Library phased out.** The OBVIOUS Template Library workflow has been removed this release; curated and version-controlled imports are now handled through the **Import from Public/Private GitHub** buttons.

## Features

### Script & GitHub Automation
- **Download Scripts / Download Scripts (JSON)** – Export your entire NinjaOne script library as either PowerShell packages or raw JSON archives with SHA-validated payloads for compliance, cold storage, or tenant migrations.
- **Import Scripts from JSON** – Queue any number of SHADOW exports for re-import, preserve valid source category mappings when present, and only assign the SHADOW category when category data is missing, while skipping unsupported languages and respecting backup guardrails.
- **Compare with GitHub** – Launch a full-screen diff overlay that progressively loads large libraries (50–1000 files), highlights status buckets, filters results, previews inline diffs, and queues uploads per detected language/extension. JSON uploads now mirror **Backup Now** outputs, preserving script variables/parameters so GitHub exports stay identical to local backups.
- **Import from GitHub** – Browse the connected repository directly inside NinjaOne, multi-select PS1/SH/BAT/JS/VBS/CMD/JSON assets, choose a destination category (or preserve source categories), and import them in bulk with progress feedback and auto-refresh.
- **Script Usage** – Open a single overlay that combines the former Used Scripts and Unused Scripts views, revealing where each script runs across policies, scheduled tasks, and system tray configurations, and flagging scripts that are not referenced anywhere so you can retire them safely. Each tab has its own live name/ID filter, and long reference lists collapse behind expandable summaries to keep large libraries readable.
- **Script Manager (Mass Script Management)** – Open one overlay that lists every script (metadata only — no script body is decoded) for fast, library-wide organization. Filter live by name/description/ID and by Language, OS, Architecture, or Category; select scripts individually or with select-all; then **bulk add or remove a category** across the selection — additions are computed from each script's current categories so they're never overwritten, and a removal that would empty a script falls back to Uncategorized. An optional **Edit mode** enables inline renaming and description editing, with a confirmation guard that protects `ShadowGitSync:` markers. Script names open in a new browser tab so the filter stays open in the original, and native scripts are automatically restricted to category-only changes.

### Template Library Sync
- **Compare Templates** – Analyse your environment against the official NinjaOne Template Library, review the remediation list, and optionally apply or skip updates with confirmation prompts.
- **Import from Public GitHub (ShadowGitSync)** – *(formerly the “Sync from GitHub” button — behavior unchanged.)* Keep individual NinjaOne scripts aligned with a third-party GitHub source by storing a raw URL in the script's description (`ShadowGitSync:<url>`). A preview-first overlay scans every tagged script, shows **Up to date / Changed / Error** status with side-by-side diffs, and applies code-only updates (name, categories, parameters, language/OS, and the description tag are all preserved). Before the first update is written, a single dialog lets you choose **Back up, then update**, **Update without backup**, or **Cancel** — and SHADOW notes when a full backup was already made today so you can skip a redundant one.
- **Import from Private GitHub** – Browse your token-authenticated **private** repository from the Template Library page, multi-select scripts, choose a destination category (or preserve the source categories), and import them in bulk with progress feedback. Configure the Token, Repository, Branch, and Path in the SHADOW popup → **Private Repo** tab first.

### Custom Fields & Metadata
- **Device Custom Fields – Export / Import** – Back up device-level metadata or restore field definitions from JSON exports, with per-file validation and backup enforcement.
- **Organization Custom Fields – Export / Import** – Move customer-specific metadata between tenants and ensure the definitions stay consistent across environments.
- **Location Custom Fields – Export / Import** – Keep branch/location data aligned by exporting and re-importing standardized definitions with a single click.
- **Custom Field Manager (Device / Organization / Location)** – Open one filterable table of every custom field for the selected scope and review each field's **Technician**, **Script**, and **API** permission. Switch on **Edit mode** to adjust those permissions plus the description, footer text, and tooltip text inline; **Apply** then submits every pending edit as throttled, MFA-aware writes that perform a full-replace while preserving all other properties of each field — no separate re-fetch needed.
- **Custom Field Usage (Device / Organization / Location)** – Open the **Usage** button to scan every NinjaOne script's code for custom-field reads and writes (`Get`/`Set-NinjaProperty`, `Ninja-Property-Get/Set/Clear/Options`, the `ninjarmm-cli` get/set forms, and documentation variants) and cross-reference them against the defined fields. A two-tab report (**By Field** / **By Script**) shows which scripts read or write each field, flags defined-but-unused fields, used-but-undefined names, and read/write conflicts with a field's Script permission, and exports to CSV or JSON.
- **Bulk Set Values (Organization)** – Set one organization custom field value across all or selected organizations at once. SHADOW reads each org's current value records and writes back only the chosen field — every other value is preserved exactly — using a type-aware input for text, checkbox, numeric, date, single-select Dropdown, and Multi-select fields (relational reference types are shown disabled). Includes org search, select-all, and per-org progress with throttled writes.

### Policy, Backup & Compliance Operations
- **NinjaRemote Confirmation Audit (Device Search)** – Open the NinjaRemote Confirmation Audit overlay from Device Search to detect workstations where effective remote confirmation resolves to false, review organization/location plus Effective/Override context, optionally hide inherited rows, export CSV findings, and reset selected devices to organization defaults.

- **Backup Now** – Run a full backup from the System Dashboard that collects scripts and custom fields, applies automatic path sanitisation, honours the “force local backup” setting, and uses hardened, rate-limited GitHub writes.
- **Auto Backup Scheduling** – When Auto backup is enabled, SHADOW checks backup age at extension startup and automatically runs a full backup once it exceeds the configurable max-age threshold, keeping backups fresh without manual effort or redundant runs.

<img width="640" height="400" alt="shadowbackup" src="https://github.com/user-attachments/assets/4537e643-213e-46d2-a67b-6fe899ab46c0" />

- **Bulk Policy Assignment** – Apply a monitoring policy to a selected device role across every organisation at once via an overlay that supports progressive loading, select-all toggles, and real-time success/error reporting.
- **Policy Report** – Generate structured policy documentation from the Reporting page with selectable sections, PDF-friendly output, and optional raw JSON companion exports.
- **Policy Override Audit** – Launch the Overrides audit from the Reporting page to identify inherited vs overridden values and export results as a self-contained HTML file.
- **Asset Report** – Open a dedicated asset lifecycle view from the Reporting page that analyzes managed workstation/server inventory by warranty-age buckets, surfaces insights (e.g., old/unknown devices), and provides exportable report output for stakeholder reviews.

<img width="832" height="739" alt="bulkpolicyassignment" src="https://github.com/user-attachments/assets/f2d65b95-35ea-4d99-acae-b0238a17f0b1" />

- **Backup Security Overlay** – Track when backups were completed, view the daily status, and require a same-day backup before any script or field import can proceed.

### IT Asset Management (ITAM) Templates
- **Add Unmanaged Devices** – Open an overlay with curated ITAM parent/child templates grouped by technology stack, preview icon matches from your existing tenant, and queue role creation with automatic batching, countdown pauses, and resume prompts that avoid MFA lockouts.
- **Import Unmanaged Devices** – Load JSON template files generated by SHADOW to reproduce complex ITAM role structures in new tenants while reusing stored custom fields and hierarchy metadata.
- **Export Unmanaged Devices** – Capture your current unmanaged-role definitions as JSON for audit trails, change reviews, or peer sharing.
- Automations include smart batching, MFA-friendly pauses, resume prompts, and clear success/error toasts to keep long deployments reliable.

### Global Coverage & Secure Workflows
- Works with every NinjaOne domain worldwide (Europe, Canada, Australia, multi-tenant, and more).
- Buttons, overlays, and settings are injected directly in the NinjaOne UI with streamlined refresh handling for a smooth operator experience.
- All automation respects strict security practices with validated inputs, encrypted credential storage, secure GitHub rate limiting, and migration helpers that remove any legacy plaintext tokens.

## Button Reference Inside NinjaOne

| NinjaOne area | Buttons added by SHADOW | What they do |
| --- | --- | --- |
| **System Dashboard ▸ Overview** | Backup Now | Trigger a full backup of scripts and custom fields to GitHub (or force a local ZIP download when configured) with automatic sanitization and hardened GitHub writes. |
| **Administration ▸ Library ▸ Template Library (Scripting & Automation)** | Compare Templates · Import from Public GitHub · Import from Private GitHub | Check your tenant against the official Template Library, run **Import from Public GitHub** (ShadowGitSync) to update scripts straight from a raw GitHub URL stored in their description — with a preview diff and a back up / skip / cancel choice before any change is written — or use **Import from Private GitHub** to browse and bulk-import scripts from your token-authenticated private repository. |
| **Administration ▸ Library ▸ Automation** | Download Scripts · Download Scripts (JSON) · Import Scripts from JSON · Compare with GitHub · Script Usage · Script Manager | Export scripts as PowerShell or JSON archives, import JSON exports back into NinjaOne, launch the GitHub diff viewer to audit live vs. version-controlled code, open **Script Usage** — one overlay (replacing the former Used and Unused Scripts buttons) that reviews where scripts are used and which are unused across policies, scheduled tasks, and system tray configurations — or open **Script Manager** for Mass Script Management: list every script, filter by name/description/ID, Language, OS, Architecture, or Category, and bulk add/remove categories (plus optional inline rename and description editing). |
| **Administration ▸ Devices ▸ Device Custom Fields** | Export · Import · Custom Field Manager · Usage | Save device custom field definitions to JSON or restore them from a backup, open **Custom Field Manager** to review and bulk-edit field permissions (Technician/Script/API) plus description, footer, and tooltip text, or open **Usage** to see which scripts read/write each field. |
| **Administration ▸ Customers ▸ Organization Custom Fields** | Export · Import · Custom Field Manager · Usage · Bulk Set Values | Migrate organization-level custom fields between tenants or back them up safely, open **Custom Field Manager** to review and bulk-edit permissions and details, open **Usage** to see which scripts read/write each field, or use **Bulk Set Values** to set one field's value across all/selected organizations at once (other values preserved). |
| **Administration ▸ Customers ▸ Location Custom Fields** | Export · Import · Custom Field Manager · Usage | Export or import location-specific custom fields in bulk, open **Custom Field Manager** to review and bulk-edit permissions and details, or open **Usage** to see which scripts read/write each field. |
| **Administration ▸ Customers ▸ Organizations** | Bulk Policy Assignment | Assign a monitoring policy to a device role for every organization at once. |
| **Reporting** | SHADOW Policy Report · Overrides · Asset Report | Open the full Policy Report builder, the Policy Override Audit overlay, or the Asset Report overlay. Asset Report shows managed workstation/server age distribution (based on warranty start), highlights aging/unknown assets, and supports export for planning and documentation. |
| **Device Search** | NinjaRemote Confirmation Audit | Audit Windows workstations where effective Ask Confirmation resolves to false, view Organization/Location plus Effective/Override columns, optionally hide inherited rows, export CSV, and reset selected devices to org defaults. |
| **Administration ▸ Devices ▸ Roles (Unmanaged)** | Add Unmanaged Devices · Import Unmanaged Devices · Export Unmanaged Devices | Launch the ITAM template overlay to create curated unmanaged roles, import JSON template packs, or export your current unmanaged-role structure for reuse. |

---

## Getting Started

### 1. Installation

- [**Download SHADOW from the Microsoft Edge Add-ons Store**](https://microsoftedge.microsoft.com/addons/detail/shadow/kalnkhmddnjjdakhccjinkneidcbnmak)
- Open NinjaOne in Microsoft Edge and ensure the SHADOW icon appears in your browser bar.

### 2. Connect SHADOW to Your GitHub Repository

#### **Step 1: Create a Private GitHub Repository**

1. Log in to [GitHub](https://github.com).
2. Click the **+** icon (top right) and choose **New repository**.
3. Enter a repository name (e.g., `ninjaone-scripts`).
4. Set **Visibility** to `Private`.
5. (Optional) Add a README file.
6. Click **Create repository**.

#### **Step 2: Create a GitHub Personal Access Token**

1. Go to **GitHub Settings** → **Developer settings** → **Personal access tokens**.
2. Select **Tokens (classic)** or **Fine-grained tokens**.
3. Click **Generate new token**.
4. Name your token (e.g., `SHADOW Extension`).
5. Set an **Expiration** as needed.
6. **Scopes:**
    - For classic tokens: Enable at least `repo` (full control of private repositories).
    - For fine-grained tokens: Select your repository and enable `Contents: Read and Write`.
7. Click **Generate token** and copy it securely (you won't see it again).

#### **Step 3: Enter Your GitHub Details in SHADOW**

1. Click the SHADOW extension icon in Edge.
2. Fill in:
    - **GitHub token**
    - Repository (e.g., `username/ninjaone-scripts`)
    - Branch (e.g., `main`)
    - (Optional) Path (subfolder in your repo)
    - Max files to process (defaults to 300; allowed range 50–1000 (effective limit: 1000))
    - Auto-refresh comparison after upload (enable to reload the diff overlay automatically)
    - Auto backup (enable periodic backup checks at extension startup)
    - Auto backup max age (days)
    - Force local backup (skip GitHub upload and always download ZIPs locally)
    - Enable debug logging (optional troubleshooting switch that redacts secrets)
3. Click **Test Connection** and review the success banner, then choose **Save Settings** to persist your preferences.
4. If anything looks off, SHADOW now highlights the exact field that needs attention before encrypting and storing your GitHub details.

## Extension Popup Settings

| Control | Purpose |
| --- | --- |
| **GitHub token** | Stored encrypted with AES-GCM so the extension can authenticate to your private repository. |
| **Repository** | Accepts `owner/repo` or a full HTTPS URL; determines where backups and comparisons read/write data. |
| **Branch** | Defaults to `main`, but can target any branch that contains your automation assets. |
| **Path (optional)** | Limits operations to a specific folder within the repository (leave blank for the repo root). |
| **Max files to process** | Caps GitHub comparison scope (50–1000). Use a lower value for faster diff sessions on large repos. |
| **Auto-refresh comparison after upload** | When enabled, the GitHub diff overlay reloads automatically after pushing updates. |
| **Auto backup** | Runs full backup automatically on extension startup when backup age exceeds threshold. |
| **Auto backup max age (days)** | Maximum allowed backup age before startup auto-backup is triggered. |
| **Force local backup (skip GitHub upload)** | Overrides uploads and always downloads a ZIP to your machine—ideal for air-gapped backups. |
| **Test Connection** | Validates the token, repository, and branch before you run exports or backups. |
| **Save Settings** | Validates and sanitizes GitHub fields before persisting preferences and checkbox selections for future sessions. |
| **Enable debug logging** | Toggles additional console diagnostics (with secrets redacted) under the Debug heading of the popup. |

---

## Typical Workflows

- **Backup:** Export all scripts from NinjaOne to a local ZIP archive or your GitHub repository via the Backup Now button, benefitting from automatic path sanitization and secure GitHub API writes.
- **Bulk Update:** Import multiple scripts by selecting one or more unpacked JSON files for upload to NinjaOne.
- **Audit/Compare:** Compare scripts in NinjaOne with versions in your GitHub repository to identify differences and maintain consistency.
- **Custom Field Migration:** Export device, organization, or location custom fields from one tenant and import them into another.
- **Policy Rollout:** Apply a monitoring policy to a device role across every organization with Bulk Policy Assignment.
- **Asset Lifecycle Review:** Use **Reporting → Asset Report** to quickly spot devices nearing replacement age, identify unknown warranty data, and export the overview for internal planning or customer-facing reports.
- **ITAM Template Deployment:** Use the unmanaged-device overlays to add curated templates, or import/export JSON packs to keep roles aligned across tenants.

---

## Security

- GitHub tokens are stored encrypted using AES-GCM and never in plaintext.
- Strict permissions and hardened content security policy by default.
- All user inputs—including repository names, folder selections, and file paths—are sanitized before any GitHub call is attempted.
- Every GitHub read/write flows through the secure wrapper with token validation, rate-limited requests, and hardened error handling to prevent misuse or credential leakage.
- Bulk backup uploads validate each generated destination path and abort safely if sanitization fails, keeping your repository free of unexpected files.

---

## Who Benefits from SHADOW?

- **NinjaOne administrators** with multiple tenants or environments.
- **IT teams and MSPs** using version control for scripts.
- Anyone who wants a fast, reliable, and auditable workflow for automation content in NinjaOne.

---

*Disclaimer: This is a private, independent project and is in no way officially affiliated with, endorsed by, or sponsored by NinjaOne, Inc.*

Use at your own risk.
