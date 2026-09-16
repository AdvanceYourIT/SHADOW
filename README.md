# SHADOW – Stealthy Handler for Automated Data Optimization and Workflow

SHADOW is a Microsoft Edge browser extension that supercharges NinjaOne administrators with powerful, secure tools for managing, comparing, and migrating scripts and monitoring policies. SHADOW integrates directly into the NinjaOne web interface and connects seamlessly with your private GitHub repository for version control and change auditing.

<img width="128" height="128" alt="shadow_128x128" src="https://github.com/user-attachments/assets/5f28a902-69eb-47b7-86d0-d0263103a35b" />

*Disclaimer: This is a private, independent project and is in no way officially affiliated with, endorsed by, or sponsored by NinjaOne, Inc.*

SHADOW works by talking to NinjaOne's own internal, undocumented web-app endpoints — the same private routes the NinjaOne web interface itself calls — rather than a published, supported public API. NinjaOne can change, rename, or remove these endpoints at any time without notice, which can break one or more SHADOW features until an update is released. Use at your own risk, and keep the extension up to date.

---

## Support

For questions, feedback, or feature requests, please open an issue in this repository or contact me.

Or visit me in Discord! [https://discord.gg/nY2WUKWn6P](https://discord.gg/nY2WUKWn6P)

---

## **Version 1.1.11 released!**

- **New: Technician Permissions Report.** A **Technician Permissions** button on the Reporting page audits every NinjaOne technician role's full permission set as a searchable, category-grouped role × permission matrix — filter, compare roles side by side with diff highlighting, and export the current view as CSV. A **Summary** tab and a **Role Changes** tab (with per-role change cards and best-effort "changed by" attribution from NinjaOne's activity log) round out the report, and **Save Snapshot** records the current state to your GitHub backup repo (in a dedicated `reports/` folder) so later runs can diff against it. Export the full report as a standalone HTML file or straight to PDF. Sysadmin-only by default — enable it for other technicians from **SHADOW-Admin: RBAC Settings**. Layout and interactions are modeled on the standalone, MIT-licensed [ninjaone-technician-permissions-reporter](https://github.com/Icarari/ninjaone-technician-permissions-reporter) project, reimplemented natively so it runs on your already-logged-in NinjaOne session instead of a separate browser login.
- **New: Technician permissions audit included in the full backup.** **Backup Now** and **Auto Backup** now also collect a technician role-permissions snapshot (into a `technician-permissions/` folder), alongside Scripts, Custom Fields, and Policies — with its own checkbox on the **Options** tab so it can be left out like any other category.
- **New: Print PDF for Policy Override Audit and Technician Permissions Report.** Both reports previously offered only Save HTML; they now have a **Print PDF** button matching Policy Report and Asset Report, built on a shared print engine so page breaks and margins behave consistently across all four Reporting-page reports. A duplicated "Generated" timestamp on printed Asset Reports is also fixed — it now appears once, not twice. Since the browser adds its own header/footer to anything you print (title, URL, date, page number) and SHADOW can't turn that off, each Print PDF button now notes that you can disable it yourself under "Headers and footers" in the browser's print dialog.
- **New: Bitbucket Cloud as a third backup destination.** A new **Bitbucket** tab in the SHADOW popup (Access Token, Workspace, Repository, Branch, Path) lets **Backup Now** / **Auto backup** push to a Bitbucket Cloud repository, alongside the existing GitHub and Azure DevOps options — **Backup destination** on the **Options** tab now offers all three, still mutually exclusive. Authentication uses a Bitbucket **Repository Access Token** rather than an App Password, since Bitbucket has fully deprecated those. There's no Compare with Bitbucket yet — it's a backup-only destination for now.
- **New: NinjaOne US Federal instance support.** SHADOW now activates on `fed.ninjarmm.com`, so NinjaOne's FED-region customers get the full toolset — API base detection was already region-agnostic, so every feature works without further configuration.
- **Fixed: large backups could silently fail and fall back to a local ZIP.** Backups on large tenants (thousands of scripts/policies) build one giant payload and send it to the background script in a single message; Chrome's content-script-to-background messaging has a size ceiling that call could exceed, failing with an opaque "Could not serialize message" error. Oversized messages are now split into chunks and reassembled before dispatching, so large backups upload correctly instead of quietly falling back to a local download.

## Features

### Script & GitHub Automation
- **Download Scripts / Download Scripts (JSON)** – Export your entire NinjaOne script library as either PowerShell packages or raw JSON archives with SHA-validated payloads for compliance, cold storage, or tenant migrations.
- **Import Scripts from JSON** – Queue any number of SHADOW exports for re-import, preserve valid source category mappings when present, and only assign the SHADOW category when category data is missing, while skipping unsupported languages and respecting backup guardrails.
- **Compare with GitHub** – Launch a full-screen diff overlay that progressively loads your entire library (no file-count cap), highlights status buckets, filters results, previews inline diffs, and queues uploads per detected language/extension. JSON uploads now mirror **Backup Now** outputs, preserving script variables/parameters so GitHub exports stay identical to local backups. Selected scripts can be uploaded to GitHub, or applied from GitHub back into NinjaOne.
- **Compare with Azure DevOps** – The same full-screen diff overlay, filters, and per-script upload queue as Compare with GitHub — including applying selected changes back into NinjaOne — but against the Azure DevOps repository configured on the popup's **Azure DevOps** tab, an independent, additional sync target, not a replacement for GitHub.
- **Import from GitHub** – Browse the connected repository directly inside NinjaOne, multi-select PS1/SH/BAT/JS/VBS/CMD/JSON assets, choose a destination category (or preserve source categories), and import them in bulk with progress feedback and auto-refresh.
- **Script Usage** – Open a single overlay that combines the former Used Scripts and Unused Scripts views, revealing where each script runs across policies, scheduled tasks, and system tray configurations, and flagging scripts that are not referenced anywhere so you can retire them safely. Each tab has its own live name/ID filter, and long reference lists collapse behind expandable summaries to keep large libraries readable. An optional **Include device-level overrides** toggle additionally scans every device's effective policy for automations added or changed directly on the device (one API call per device, with progress feedback), so scripts used only through device-level policy overrides are no longer reported as unused.
- **Script Manager (Mass Script Management)** – Open one overlay that lists every script (metadata only — no script body is decoded) for fast, library-wide organization, with two tabs. **Categories**: filter live by name/description/ID and by Language, OS, Architecture, or Category; select scripts individually or with select-all; then **bulk add or remove a category** across the selection — additions are computed from each script's current categories so they're never overwritten, and a removal that would empty a script falls back to Uncategorized. An optional **Edit mode** enables inline renaming and description editing, with a confirmation guard that protects `ShadowGitSync:` markers. **Script Variables**: bulk add a new variable across every selected script, or update an existing one (matched by name) wherever it's already used — define it manually (type, default value, required flag, description) or **copy it from an existing variable on another script**, which pre-fills the same fields so it can still be tweaked before applying. Choose whether to also add it to scripts that don't have it yet or only update where it's already present, and a usage summary flags variable names that are reused across scripts with different types. Script names open in a new browser tab so the filter stays open in the original, and native scripts are automatically restricted to category-only changes.

### Template Library Sync
- **Compare Templates** – Analyse your environment against the official NinjaOne Template Library, review the remediation list, and optionally apply or skip updates with confirmation prompts.
- **Import from Public GitHub (ShadowGitSync)** – *(formerly the “Sync from GitHub” button — behavior unchanged.)* Keep individual NinjaOne scripts aligned with a third-party GitHub source by storing a raw URL in the script's description (`ShadowGitSync:<url>`). A preview-first overlay scans every tagged script, shows **Up to date / Changed / Error** status with side-by-side diffs, and applies code-only updates (name, categories, parameters, language/OS, and the description tag are all preserved). Before the first update is written, a single dialog lets you choose **Back up, then update**, **Update without backup**, or **Cancel** — and SHADOW notes when a full backup was already made today so you can skip a redundant one.
- **Import from Private GitHub** – Browse your token-authenticated **private** repository from the Template Library page, multi-select scripts, choose a destination category (or preserve the source categories), and import them in bulk with progress feedback. Configure the Token, Repository, Branch, and Path in the SHADOW popup → **Private GitHub** tab first.

### Ad-hoc Script Execution
- **Run Script Now (Device Search & Device pages)** – Paste a raw PowerShell script and run it once on selected devices, without saving it to the script library first — no need to build a permanent script for a one-off task. A step wizard walks through **Select targets** (organizations, then a device checklist filtered to Windows for this release), **Paste script**, and **Review & run**, with a persistent sidebar summarizing the chosen devices and script and Edit links back to either step. The pasted script is never written to your NinjaOne script library: SHADOW auto-creates a small reusable wrapper script (once, on first use) that decodes and executes a base64-encoded payload handed to it at run time — the same technique tools like ImmyBot and Rewst use to run ad-hoc scripts against NinjaOne. All selected devices are dispatched in a single request.

### Custom Fields & Metadata
- **Device Custom Fields – Export / Import** – Back up device-level metadata or restore field definitions from JSON exports, with per-file validation and backup enforcement.
- **Organization Custom Fields – Export / Import** – Move customer-specific metadata between tenants and ensure the definitions stay consistent across environments.
- **Location Custom Fields – Export / Import** – Keep branch/location data aligned by exporting and re-importing standardized definitions with a single click.
- **Custom Field Manager (Device / Organization / Location)** – Open one filterable table of every custom field for the selected scope and review each field's **Technician**, **Script**, and **API** permission. Switch on **Edit mode** to adjust those permissions plus the description, footer text, and tooltip text inline; **Apply** then submits every pending edit as throttled, MFA-aware writes that preserve writable field data while filtering out NinjaOne read-only/UI-only response fields before saving.
- **Custom Field Usage (Device / Organization / Location)** – Open the **Usage** button to scan every NinjaOne script's code for custom-field reads and writes (`Get`/`Set-NinjaProperty`, `Ninja-Property-Get/Set/Clear/Options`, the `ninjarmm-cli` get/set forms, and documentation variants) and cross-reference them against the defined fields. A two-tab report (**By Field** / **By Script**) shows which scripts read or write each field, flags defined-but-unused fields, used-but-undefined names, and read/write conflicts with a field's Script permission, and exports to CSV or JSON.
- **Bulk Set Values (Organization)** – Set organization custom field values across all or selected organizations at once, two ways: **(1) enter a value manually** with a type-aware input for text, checkbox, numeric, date, single-select Dropdown, and Multi-select fields, or **(2) copy from a source organization** — pick one source org and select which field(s) to copy onto the targets. SHADOW writes only the record(s) for the chosen field(s) — every other value on the organization is left untouched; fields the source organization has not set are skipped so targets are never blanked out. Both pickers only show fields your technician role has Edit access to, since NinjaOne rejects any write that includes even one field you can't edit. Relational reference types are shown disabled. Includes org search, select-all, an "only where empty" option, and per-org progress with throttled writes.

### Policy, Backup & Compliance Operations
- **NinjaRemote Confirmation Audit (Device Search)** – Open the NinjaRemote Confirmation Audit overlay from Device Search, filter by organization (searchable multiselect, defaults to all) and click **Start Scan** to detect workstations where effective remote confirmation resolves to false, review organization/location plus Effective/Override context, optionally hide inherited rows, export CSV findings, and reset selected devices to organization defaults.

- **Backup Now** – Run a full backup from the System Dashboard that collects scripts, policies, and custom fields (each individually toggleable in the popup's **GitHub** tab), applies automatic path sanitisation, honours the “force local backup” setting, and pushes to hardened, rate-limited GitHub, Azure DevOps, or Bitbucket writes — whichever destination is selected via the **Backup destination** toggle; only one runs per backup, never more than one active at once. The progress card has a **Minimize** button that docks it as a small status pill next to the SHADOW flyout menu (bottom-left) so it doesn't sit blocking the corner of the screen for the whole backup; click the pill to expand it again.
- **Auto Backup Scheduling** – When Auto backup is enabled, SHADOW checks backup age at extension startup and automatically runs a full backup once it exceeds the configurable max-age threshold, keeping backups fresh without manual effort or redundant runs.

<img width="640" height="400" alt="shadowbackup" src="https://github.com/user-attachments/assets/4537e643-213e-46d2-a67b-6fe899ab46c0" />

- **Bulk Policy Assignment** – Apply a monitoring policy to a selected device role across every organisation at once via an overlay that supports progressive loading, select-all toggles, and real-time success/error reporting.
- **Download Policies (JSON)** – From the Administration ▸ Policies page (Agent/NMS/VM/MDM tabs), export every policy as a JSON ZIP grouped by device class — the same backup captured by **Backup Now**, available as a standalone, on-demand download.
- **Compare Policies with GitHub** – From the Administration ▸ Policies page, open a structural (path-by-path, not text) diff between each live NinjaOne policy and its GitHub-backed-up copy, with a **View Diff** per changed policy showing exactly which fields differ, **Select All** / **Upload Selected**, search and device-class filters, and a progress bar for both loading and bulk upload since a tenant can have 500+ policies. Upload only ever writes to GitHub, never back into NinjaOne.
- **Compare Policies with Azure DevOps** – The same structural diff, per-field **View Diff**, filters, and progress bar as Compare Policies with GitHub, against the Azure DevOps repository instead. Push Selected only ever writes to Azure DevOps, never back into NinjaOne — same read-only-toward-NinjaOne design as the GitHub version.
- **Policy Report** – Generate structured policy documentation from the Reporting page with selectable sections, PDF-friendly output, and optional raw JSON companion exports.
- **Policy Override Audit** – Launch the Overrides audit from the Reporting page to identify inherited vs overridden values across a policy tree and its devices. Device-level scanning is off by default (policy-level overrides only) since it fetches one config per device in the tree — check **Include device-level overrides** before running to also scan devices, at the cost of a slower run on large tenants. Filter by search/category/type, toggle **Hide clean** to drop blocks with no overrides, **Expand/Collapse all** at once, jump to any policy with **Open in NinjaOne**, and export results as a self-contained HTML file, JSON, CSV, or straight to PDF via **Print PDF**.
- **Asset Report** – Open a dedicated asset lifecycle view from the Reporting page that analyzes managed workstation/server inventory by warranty-age buckets, surfaces insights (e.g., old/unknown devices), and provides exportable report output — including a **Print PDFs** button that generates one PDF per selected organization — for stakeholder reviews. Narrow the report itself to specific **Organizations** and/or **Devices** via searchable multiselect filters (Devices narrows to whichever organizations are selected); the Hardware Lifecycle Report toggle is available whenever exactly one organization is selected.
- **Fleet Health Report** – A tenant-wide and per-organization health scorecard from the Reporting page, built from the same aggregate data behind NinjaOne's own System Dashboard: device health, patch and vulnerability status, backup failures, triggered alerts, and fleet composition, with organizations sorted by how much attention they need. Narrow which organizations are shown via a searchable **Organizations** filter (combines with the existing text search); no Devices filter here since the underlying data is aggregate-only, with no per-device rows. Export as a standalone HTML file, CSV, or straight to PDF via **Print PDF**.
- **Automation Usage Report** – From the Reporting page, see which automations (policy-condition-triggered actions and manually/scheduled-run scripts) ran over a date range, how many times each ran, and success vs. failure counts, with a **By Organization** breakdown sortable by run count, name, or failures, plus a **Recent Failures** list and a live **Queued / Pending Automations** section for automations already dispatched but not yet run on their target device. Narrow the query itself to specific **Organizations** and/or **Devices** via searchable multiselect filters, matching Activities Report's own picker. Switch to **Executive Summary** for a condensed, client-ready view (headline numbers, top automations, per-organization rollup) — unlike the Full Report, it never defaults to showing every organization at once: select the client(s) in the Organizations filter and click Run, since this view is meant to be handed to a specific client. The search box, sort controls, and export buttons all apply to whichever view is currently shown, and Print PDF/Save HTML strip the filter controls so the exported document shows only the results. Export as a standalone HTML file, CSV, or straight to PDF via **Print PDF**.
- **Technician Permissions Report** – From the Reporting page, audit every technician role's full permission set as a searchable role × permission matrix, save a snapshot to GitHub, and see a **Changes** tab diffing the current state against that snapshot on later runs (who changed what, where activity-log attribution is available). Export as a standalone HTML file, CSV, or straight to PDF via **Print PDF**. Sysadmin-only by default — enable it for other technicians from the **SHADOW-Admin: RBAC Settings** overlay.
  <br>*Inspired by [ninjaone-technician-permissions-reporter](https://github.com/Icarari/ninjaone-technician-permissions-reporter) (MIT license) — reimplemented natively in SHADOW rather than embedded, since that project is a separate standalone Python/Playwright tool.*
- **Activities Report** – From the Reporting page, pull the tenant activity log for a minute-precision date/time range and filter by activity type (searchable multiselect — pick multiple types at once) and description text together (e.g. "Automation Completed" + "Software Updated" + description contains "SentinelOne") — combined filtering NinjaOne's own Activities page doesn't offer. Narrow the query itself to specific **Organizations** and/or **Devices** via searchable multiselect filters. Export as a standalone HTML file, CSV, or straight to PDF via **Print PDF**. Sysadmin-only by default — enable it for other technicians from the **SHADOW-Admin: RBAC Settings** overlay.

*A note on PDFs: SHADOW's own report timestamps and layout are print-tuned, but the browser itself also adds its own header/footer (page title, URL, date, page number) to anything you print — that's Edge/Chrome behavior, not something SHADOW can turn off. For a clean PDF, uncheck **Headers and footers** under **More settings** in the print dialog before saving.*

<img width="832" height="739" alt="bulkpolicyassignment" src="https://github.com/user-attachments/assets/f2d65b95-35ea-4d99-acae-b0238a17f0b1" />

- **Backup Security Overlay** – Track when backups were completed, view the daily status, and require a same-day backup before any script or field import can proceed.

### IT Asset Management (ITAM) Templates
- **Add Unmanaged Devices** – Open an overlay with curated ITAM parent/child templates grouped by technology stack, preview icon matches from your existing tenant, and queue role creation with automatic batching, countdown pauses, and resume prompts that avoid MFA lockouts.
- **Import Unmanaged Devices** – Load JSON template files generated by SHADOW to reproduce complex ITAM role structures in new tenants while reusing stored custom fields and hierarchy metadata.
- **Export Unmanaged Devices** – Capture your current unmanaged-role definitions as JSON for audit trails, change reviews, or peer sharing.
- Automations include smart batching, MFA-friendly pauses, resume prompts, and clear success/error toasts to keep long deployments reliable.

### Cloud Monitor Tools
- **Cloud Monitor Tools** – A button on the Organizations page opens an overlay for bulk Cloud Monitor work, with two modes.
- **Clone with new IP(s)** – Pick a template — either an existing Cloud Monitor (its type, frequency, timeout, and alert conditions are cloned as-is) or **Build new** (Ping, DNS, Port Scan, or HTTP/HTTPS, configured from scratch with no existing monitor required, though it starts with no alert conditions). Paste a list of new IP addresses (optionally `IP, Name` pairs), pick a target organization and location, and create one new monitor per line in a single throttled batch.
- **Move to organization** – Select one or more existing Cloud Monitors and move them to a different organization/location in a single bulk request.
- Toggle it on/off in the SHADOW popup under Options, and restrict it to sysadmins via RBAC Settings like any other feature.

### Role-Based Access Control (RBAC)
- **SHADOW-Admin: RBAC Settings** – A button visible only to sysadmin technicians, on the Automation page next to the other script buttons. Opens an overlay listing every toggleable SHADOW feature with a checkbox, grouped by page (Scripts page, Template Library, Custom Fields, Reporting, Other) the same way the popup's Options tab groups its button-visibility toggles — this is an **allowlist**: only checked features are available to non-sysadmin technicians, and click **Apply** to save. Unchecked features (including any new SHADOW feature added in a future update, until you explicitly check it here) stay sysadmin-only. The first time you open it, before anything has been saved, every box is pre-checked to match the current always-on behavior, so clicking Apply without changing anything doesn't lock anyone out.
- **How it's stored** – Your choices are saved as a JSON payload in the value of a single, auto-created, tenant-wide **GLOBAL-scope custom field** (one value for the whole tenant, not per-device/org/location). The field's Technician permission is Read-only and its Script/API permissions are off, so every technician can read it (gating works instantly, with no NinjaOne Role changes needed) but none can edit it directly. Because GLOBAL-scope fields are fetched through a different endpoint than device/organization/location fields, this field never appears in SHADOW's own Custom Field Manager or Bulk Set Values pickers, for any technician. When you click **Apply**, SHADOW automatically and briefly makes the field editable (NinjaOne requires this to write a GLOBAL field's value), writes the new settings, then locks it straight back down to Read-only — no manual field editing required.
- **Enforcement** – Every gated SHADOW button/overlay checks the current technician's NinjaOne `sysadmin` flag live before rendering; features not on the allowlist are hidden entirely for non-sysadmins (not just disabled), including their checkbox in the SHADOW popup's Options tab.
- **Fail-safe design** – If the configuration was never saved, or can't be read at all (network error), features fail **open** (nothing restricted) so the extension never silently breaks for the whole team before it's configured. Once a sysadmin has explicitly saved an allowlist — even an empty one — it's enforced literally, restricting anything not on it to sysadmins. If a technician's sysadmin status itself can't be determined, that check fails **closed** (treated as non-sysadmin).

### Global Coverage & Secure Workflows
- Works with every NinjaOne domain worldwide (Europe, Canada, Australia, US Federal, multi-tenant, and more).
- Buttons, overlays, and settings are injected directly in the NinjaOne UI with streamlined refresh handling for a smooth operator experience.
- All automation respects strict security practices with validated inputs, encrypted credential storage, secure GitHub rate limiting, and migration helpers that remove any legacy plaintext tokens.

## Button Reference Inside NinjaOne

| NinjaOne area | Buttons added by SHADOW | What they do |
| --- | --- | --- |
| **System Dashboard ▸ Overview** | Backup Now | Trigger a full backup of scripts, policies and custom fields to whichever destination is selected in the popup — GitHub, Azure DevOps, or Bitbucket, never more than one — or force a local ZIP download when configured, with automatic sanitization and hardened, rate-limited writes. |
| **Administration ▸ Library ▸ Template Library (Scripting & Automation)** | Compare Templates · Import from Public GitHub · Import from Private GitHub | Check your tenant against the official Template Library, run **Import from Public GitHub** (ShadowGitSync) to update scripts straight from a raw GitHub URL stored in their description — with a preview diff and a back up / skip / cancel choice before any change is written — or use **Import from Private GitHub** to browse and bulk-import scripts from your token-authenticated private repository. |
| **Administration ▸ Library ▸ Automation** | Download Scripts · Download Scripts (JSON) · Import Scripts from JSON · Compare with GitHub · Compare with Azure DevOps · Script Usage · Script Manager · SHADOW-Admin: RBAC Settings (sysadmin only) | Export scripts as PowerShell or JSON archives, import JSON exports back into NinjaOne, launch the GitHub or Azure DevOps diff viewer to audit live vs. version-controlled code (both can apply selected changes back into NinjaOne), open **Script Usage** — one overlay (replacing the former Used and Unused Scripts buttons) that reviews where scripts are used and which are unused across policies, scheduled tasks, and system tray configurations — or open **Script Manager** for Mass Script Management: list every script, filter by name/description/ID, Language, OS, Architecture, or Category, with a **Categories** tab (bulk add/remove categories, plus optional inline rename and description editing) and a **Script Variables** tab (bulk add/update a script variable by name across selected scripts, with a usage summary that flags naming conflicts). Sysadmin technicians additionally see **SHADOW-Admin: RBAC Settings** to choose which SHADOW features are restricted to sysadmins only. |
| **Administration ▸ Devices ▸ Device Custom Fields** | Export · Import · Custom Field Manager · Usage | Save device custom field definitions to JSON or restore them from a backup, open **Custom Field Manager** to review and bulk-edit field permissions (Technician/Script/API) plus description, footer, and tooltip text, or open **Usage** to see which scripts read/write each field. |
| **Administration ▸ Customers ▸ Organization Custom Fields** | Export · Import · Custom Field Manager · Usage · Bulk Set Values | Migrate organization-level custom fields between tenants or back them up safely, open **Custom Field Manager** to review and bulk-edit permissions and details, open **Usage** to see which scripts read/write each field, or use **Bulk Set Values** to set field values across all/selected organizations at once — either a manually entered value or values copied from a source organization (other values preserved). |
| **Administration ▸ Customers ▸ Location Custom Fields** | Export · Import · Custom Field Manager · Usage | Export or import location-specific custom fields in bulk, open **Custom Field Manager** to review and bulk-edit permissions and details, or open **Usage** to see which scripts read/write each field. |
| **Administration ▸ Customers ▸ Organizations** | Bulk Policy Assignment · Cloud Monitor Tools | Assign a monitoring policy to a device role for every organization at once, or open **Cloud Monitor Tools** to bulk-clone Cloud Monitors across new IP addresses (from an existing monitor or built from scratch) or move existing Cloud Monitors to a different organization. |
| **Administration ▸ Policies** (Agent/NMS/VM/MDM tabs) | Download Policies (JSON) · Compare Policies with GitHub · Compare Policies with Azure DevOps | Export every policy as a JSON ZIP grouped by device class, or launch a structural (path-by-path) diff between live NinjaOne policies and their GitHub- or Azure DevOps-backed-up copies, with **Select All** / **Upload Selected** (or **Push Selected**), a **View Diff** per changed field, filters, and a progress bar for large policy counts. Both only ever write to GitHub/Azure DevOps — by design, policy comparison has no import-back-into-NinjaOne option (unlike script comparison). |
| **Reporting** | SHADOW Policy Report · Overrides · Asset Report · Fleet Health · Automation Usage · Technician Permissions · Activities | Open the full Policy Report builder, the Policy Override Audit overlay, the Asset Report overlay, the Fleet Health Report, the Automation Usage Report, the Technician Permissions Report, or the Activities Report. Asset Report shows managed workstation/server age distribution (based on warranty start), highlights aging/unknown assets, and supports export for planning and documentation. Fleet Health Report shows a per-organization scorecard (device health, patches, vulnerabilities, backups, alerts) from NinjaOne's own dashboard data. Automation Usage Report shows run counts and success/failure rates per automation over a date range, plus currently-queued automations. Technician Permissions Report shows a role × permission matrix with GitHub-backed change tracking; sysadmin-only until enabled via RBAC Settings. Activities Report filters the tenant activity log by type and description text together over a date range; sysadmin-only until enabled via RBAC Settings. |
| **Device Search** | NinjaRemote Confirmation Audit · Run Script Now | Audit Windows workstations where effective Ask Confirmation resolves to false, view Organization/Location plus Effective/Override columns, optionally hide inherited rows, export CSV, and reset selected devices to org defaults. **Run Script Now** opens a wizard to pick organizations/devices, paste a PowerShell script, and run it once on the selected Windows devices — without saving it to the script library. |
| **Device Dashboard (individual device page)** | Run Script Now | Same ad-hoc PowerShell runner as Device Search, pre-scoped to the device you're viewing — paste a script and run it once on just that device. |
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
2. Select **Fine-grained tokens** (recommended — see note below).
3. Click **Generate new token**.
4. Name your token (e.g., `SHADOW Extension`).
5. Set an **Expiration** as needed.
6. **Scopes:** Select your repository and enable `Contents: Read and Write`.
7. Click **Generate token** and copy it securely (you won't see it again).

> **Fine-grained tokens are recommended over classic tokens.** A fine-grained token can be scoped to just the one repository SHADOW needs. Classic tokens can't be scoped that narrowly — the `repo` permission they require grants access to *all* of your private repositories, not just the one you're connecting. If your GitHub host doesn't yet support fine-grained tokens, a classic token with the `repo` scope will still work, but the fine-grained option keeps SHADOW's access limited to what it actually uses.

#### **Step 3: Enter Your GitHub Details in SHADOW**

1. Click the SHADOW extension icon in Edge.
2. Fill in:
    - **GitHub token**
    - Repository (e.g., `username/ninjaone-scripts`)
    - Branch (e.g., `main`)
    - (Optional) Path (subfolder in your repo)
    - Auto-refresh comparison after upload (enable to reload the diff overlay automatically)
3. Click **Test Connection** and review the success banner, then choose **Save Settings** to persist your preferences.
4. If anything looks off, SHADOW now highlights the exact field that needs attention before encrypting and storing your GitHub details.
5. Backup-related settings — what **Backup Now** includes, **Force local backup**, and **Auto backup** / **Auto backup max age** — live on the **Options** tab and save immediately, independent of this token-gated save.

### 3. Connect SHADOW to Your Azure DevOps Organization (optional, extra sync target)

Azure DevOps runs alongside GitHub, not instead of it — connect it if you want a second place scripts/policies sync to, or if Azure DevOps is your primary source control instead of GitHub. Skip this section entirely if you only use GitHub.

#### **Step 1: Create a Project and Repository in Azure DevOps**

1. Log in to [Azure DevOps](https://dev.azure.com).
2. Open (or create) the organization you want SHADOW to use.
3. Open (or create) a project, and open (or create) a Git repository inside it (e.g., `ninjaone-scripts`).

#### **Step 2: Create an Azure DevOps Personal Access Token**

1. From your Azure DevOps profile, go to **Personal Access Tokens** → **New Token**.
2. Name your token (e.g., `SHADOW Extension`).
3. **Organization:** select the specific organization from step 1 — not "All accessible organizations".
4. Set an **Expiration** as short as your workflow tolerates, and plan to rotate the token periodically.
5. **Scopes:** choose **Custom defined**, then under **Code** enable **Read & write**. Leave every other scope category untouched.
6. Click **Create** and copy the token securely (you won't see it again).

> **Azure DevOps PATs can't be scoped to a single repository.** Unlike a GitHub fine-grained token, the narrowest an Azure DevOps PAT can go is one organization with just the `Code` permission — that applies to every repository you can access in that organization, not just the one you're connecting. Picking the specific organization (step 3 above) and the `Code`-only custom scope (step 5) is still the least-privilege option Azure DevOps offers; there's no way to narrow it further to one repo.

#### **Step 3: Enter Your Azure DevOps Details in SHADOW**

1. Click the SHADOW extension icon in Edge, and switch to the **Azure DevOps** tab.
2. Fill in:
    - **Personal Access Token**
    - Organization (e.g., `myorg`)
    - Project
    - Repository
    - Branch (e.g., `main`)
    - (Optional) Path (subfolder in your repo)
3. Click **Test Connection** and review the success banner, then choose **Save Settings** to persist your preferences.
4. If you also want Azure DevOps to be the target for **Backup Now** / **Auto backup** (instead of GitHub), switch to the **Options** tab (the first tab in the popup) and set **Backup destination** to Azure DevOps — the two destinations are mutually exclusive, so only one is ever the active backup target. **Compare with Azure DevOps** and **Compare Policies with Azure DevOps** work independently of this choice either way.

### 4. Connect SHADOW to Your Bitbucket Repository (optional, extra sync target)

Bitbucket runs alongside GitHub and Azure DevOps, not instead of them — connect it if you want another place backups sync to, or if Bitbucket is your primary source control. Skip this section entirely if you don't use Bitbucket. Unlike GitHub/Azure DevOps, there's no Compare with Bitbucket yet — it's a backup (push-only) destination for now.

#### **Step 1: Create (or choose) a Repository in Bitbucket**

1. Log in to [Bitbucket](https://bitbucket.org).
2. Open (or create) the repository you want SHADOW to back up to (e.g., `ninjaone-scripts`).

#### **Step 2: Create a Bitbucket Repository Access Token**

1. Open the repository in Bitbucket, then go to **Repository settings** → **Access tokens**.
2. Click **Create access token**.
3. Name it (e.g., `SHADOW Extension`).
4. Under **Permissions**, enable **Repositories: Write**.
5. Click **Create** and copy the token securely (you won't see it again).

> **Bitbucket App Passwords are deprecated — use a Repository Access Token instead.** Bitbucket also offers Workspace Access Tokens, but those require a Bitbucket Cloud **Premium** subscription. A Repository Access Token works on every plan, and is also the more narrowly-scoped, least-privilege option since it's tied to just the one repository rather than every repository in the workspace.

#### **Step 3: Enter Your Bitbucket Details in SHADOW**

1. Click the SHADOW extension icon in Edge, and switch to the **Bitbucket** tab.
2. Fill in:
    - **Access Token**
    - Workspace (the first segment of your repo's URL: `bitbucket.org/<workspace>/<repo>`)
    - Repository (the second segment — the repo slug)
    - Branch (e.g., `main`)
    - (Optional) Path (subfolder in your repo)
3. Click **Test Connection** and review the success banner, then choose **Save Settings** to persist your preferences.
4. If you want Bitbucket to be the target for **Backup Now** / **Auto backup**, switch to the **Options** tab and set **Backup destination** to Bitbucket — GitHub, Azure DevOps, and Bitbucket are mutually exclusive as backup targets, so only one is ever active.

## Extension Popup Settings

The popup opens on the **Options** tab by default. It holds everything that
isn't specific to one repository connection — backup destination and
contents, the script editor position, every button-visibility toggle, and
debug logging — so it's the one tab you'll return to most often, and it
saves each setting immediately rather than behind a "Save Settings" button.

### Options tab

| Control | Purpose |
| --- | --- |
| **Backup destination (GitHub / Azure DevOps / Bitbucket)** | Chooses which one **Backup Now** / **Auto backup** pushes to — only one is ever active at a time. Defaults to GitHub. Compare with GitHub / Compare with Azure DevOps are unaffected by this setting (there's no Compare with Bitbucket yet). |
| **Backup Now includes** (Scripts / Custom fields / Policies / Technician permissions audit) | Unchecking a category leaves it out of both the local ZIP and the GitHub/Azure DevOps upload the next time Backup Now (or Auto backup) runs. All four default to checked. |
| **Force local backup (skip remote upload)** | Overrides uploads and always downloads a ZIP to your machine — ideal for air-gapped backups — instead of uploading to the backup destination above. |
| **Auto backup** | Runs full backup automatically on extension startup when backup age exceeds the threshold below. |
| **Auto backup max age (days)** | Maximum allowed backup age before startup auto-backup is triggered. |
| **Script editor on right** | Off = default left, On = custom right-side placement for the NinjaOne script editor. |
| **Buttons** | One checkbox per SHADOW button/overlay, grouped by page — unchecking one hides it from NinjaOne immediately on open tabs. Sysadmin-only features (per the RBAC allowlist) don't appear here for non-sysadmin technicians. |
| **Enable debug logging** | Toggles additional console diagnostics (with secrets redacted). |

Every control on this tab saves immediately on change — there's no "Save
Settings" button here, and none of them require a GitHub, Azure DevOps, or
Bitbucket connection to already be configured.

### GitHub tab

| Control | Purpose |
| --- | --- |
| **GitHub token** | Stored encrypted with AES-GCM so the extension can authenticate to your private repository. |
| **Repository** | Accepts `owner/repo` or a full HTTPS URL; determines where backups and comparisons read/write data. |
| **Branch** | Defaults to `main`, but can target any branch that contains your automation assets. |
| **Path (optional)** | Limits operations to a specific folder within the repository (leave blank for the repo root). |
| **Auto-refresh comparison after upload** | When enabled, the GitHub diff overlay reloads automatically after pushing updates. |
| **Test Connection** | Validates the token, repository, and branch before you run exports or backups. |
| **Save Settings** | Validates and sanitizes GitHub fields before persisting preferences for future sessions. |

### Azure DevOps tab

| Control | Purpose |
| --- | --- |
| **Personal Access Token** | Stored encrypted with AES-GCM, the same way the GitHub token is. |
| **Organization** | The Azure DevOps organization name (e.g. `myorg` in `dev.azure.com/myorg`). |
| **Project** | The Azure DevOps project that contains the target repository. |
| **Repository** | The Git repository name within that project. |
| **Branch** | Defaults to `main`, but can target any branch that contains your automation assets. |
| **Path (optional)** | Limits operations to a specific folder within the repository (leave blank for the repo root). |
| **Test Connection** | Validates the PAT, organization, project, and repository before you run comparisons or backups. |
| **Save Settings** | Persists the Azure DevOps connection details for future sessions. |

### Bitbucket tab

| Control | Purpose |
| --- | --- |
| **Access Token** | A Bitbucket Repository Access Token (Workspace Access Tokens are also accepted, but require Bitbucket Cloud Premium), stored encrypted with AES-GCM, the same way the GitHub token is. |
| **Workspace** | The Bitbucket workspace slug — the first segment of `bitbucket.org/<workspace>/<repo>`. |
| **Repository** | The repository slug — the second segment of that same URL. |
| **Branch** | Defaults to `main`, but can target any branch that contains your automation assets. |
| **Path (optional)** | Limits operations to a specific folder within the repository (leave blank for the repo root). |
| **Test Connection** | Validates the token, workspace, and repository before you run backups. |
| **Save Settings** | Persists the Bitbucket connection details for future sessions. |

### Private GitHub tab

Import-only — used solely by **Import from Private GitHub** on the Template
Library page (browsing and importing scripts into NinjaOne). Never used for
backups or comparisons, and stored separately from the GitHub tab's token.

| Control | Purpose |
| --- | --- |
| **GitHub Token** | Stored encrypted with AES-GCM, separately from the GitHub tab's token. |
| **Repository** | Accepts `owner/repo` or a full HTTPS URL for the private repository to import from. |
| **Branch** | Defaults to `main`, but can target any branch that contains your automation assets. |
| **Path (optional)** | Limits browsing/import to a specific folder within the repository (leave blank for the repo root). |
| **Test Connection** | Validates the token, repository, and branch before you browse/import. |
| **Save Settings** | Persists the private repository connection details for future sessions. |

### Help tab

| Control | Purpose |
| --- | --- |
| **Help / Readme** | Opens this README on GitHub in a new browser tab. |
| **What's New** | Opens the release notes for the currently installed version in a new browser tab. |

---

## Typical Workflows

- **Backup:** Export all scripts from NinjaOne to a local ZIP archive or your GitHub repository via the Backup Now button, benefitting from automatic path sanitization and secure GitHub API writes.
- **Bulk Update:** Import multiple scripts by selecting one or more unpacked JSON files for upload to NinjaOne.
- **Audit/Compare:** Compare scripts in NinjaOne with versions in your GitHub repository to identify differences and maintain consistency.
- **Custom Field Migration:** Export device, organization, or location custom fields from one tenant and import them into another.
- **Policy Rollout:** Apply a monitoring policy to a device role across every organization with Bulk Policy Assignment.
- **Asset Lifecycle Review:** Use **Reporting → Asset Report** to quickly spot devices nearing replacement age, identify unknown warranty data, and export the overview for internal planning or customer-facing reports.
- **ITAM Template Deployment:** Use the unmanaged-device overlays to add curated templates, or import/export JSON packs to keep roles aligned across tenants.
- **One-off Script Run:** Use **Run Script Now** on Device Search or a device's own dashboard page to paste and run a PowerShell script once on selected devices, without saving it to the script library first.

---

## Security

- GitHub tokens, the Azure DevOps PAT, and the Bitbucket access token are AES-GCM encrypted in the extension's local storage rather than kept as plaintext, each under its own storage key, and are only ever decrypted inside the background service worker — the NinjaOne page, its own scripts, and SHADOW's page-side modules never see the token value itself. (As with any browser extension, this protects against casual inspection, not against something that already has the same level of access as the extension itself — there's no OS keychain available to extensions to go further than that.)
- Messages that reach the GitHub/token-handling background code are authenticated with a per-page-load token embedded only in SHADOW's own injected script, so a forged message from any other script running on the page is rejected before it can reach the background service worker.
- Strict permissions and hardened content security policy by default.
- All user inputs—including repository names, folder selections, and file paths—are sanitized before any GitHub call is attempted.
- Every GitHub read/write flows through the secure wrapper with token validation, rate-limited requests, and hardened error handling to prevent misuse or credential leakage.
- Bulk backup uploads validate each generated destination path and abort safely if sanitization fails, keeping your repository free of unexpected files.
- Script, policy, task, device, and device role names sourced from NinjaOne, plus file names from GitHub, are HTML-escaped before being rendered in reports and pickers, preventing stored-XSS via maliciously named objects.
- Sysadmin-restricted features (e.g. the Ad-hoc Script Runner, when gated via RBAC Settings) re-check the current technician's sysadmin status inside the feature itself, not only at the button that opens it.

---

## Who Benefits from SHADOW?

- **NinjaOne administrators** with multiple tenants or environments.
- **IT teams and MSPs** using version control for scripts.
- Anyone who wants a fast, reliable, and auditable workflow for automation content in NinjaOne.

---

## Support

For questions, feedback, or feature requests, please open an issue in this repository or contact the maintainer.

---

## Credits

SHADOW got its start from a PowerShell script by [David Spzunar](https://github.com/dszp), which grew into this extension.

The Technician Permissions Report's layout and interactions are modeled on [ninjaone-technician-permissions-reporter](https://github.com/Icarari/ninjaone-technician-permissions-reporter) by Kevin Icart (MIT license), reimplemented natively in SHADOW so it runs on your already-logged-in NinjaOne session instead of a separate browser login.

---

*Disclaimer: This is a private, independent project and is in no way officially affiliated with, endorsed by, or sponsored by NinjaOne, Inc.*

SHADOW works by talking to NinjaOne's own internal, undocumented web-app endpoints — the same private routes the NinjaOne web interface itself calls — rather than a published, supported public API. NinjaOne can change, rename, or remove these endpoints at any time without notice, which can break one or more SHADOW features until an update is released. Use at your own risk, and keep the extension up to date.
