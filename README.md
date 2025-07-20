# SHADOW – Stealthy Handler for Automated Data Optimization and Workflow

SHADOW is a Microsoft Edge browser extension that supercharges NinjaOne administrators with powerful, secure tools for managing, comparing, and migrating scripts and monitoring policies. SHADOW integrates directly into the NinjaOne web interface and connects seamlessly with your private GitHub repository for version control and change auditing.

<img width="128" height="128" alt="shadow_128x128" src="https://github.com/user-attachments/assets/5f28a902-69eb-47b7-86d0-d0263103a35b" />

---

## Features

- **Bulk Export of Scripts & Monitoring Policies**
  - Export all scripts or policies from NinjaOne in one click.
  - Download as a ZIP archive for backup, migration, or documentation.

- **Bulk Import of Scripts & Monitoring Policies**
  - Import multiple scripts or policies using unpacked JSON files from your computer.
  - Efficient for onboarding, migration, and keeping environments in sync.

- **Custom Fields Export & Import**
  - Export and import custom fields for Devices, Organizations, and Locations.
  - Ideal for migrating, backing up, or synchronizing custom metadata across NinjaOne environments.

- **Live Compare with GitHub**
  - Instantly compare your scripts and policies in NinjaOne with those stored in your private GitHub repository.
  - See clear differences and keep automation libraries aligned.
  - Optimized for large repositories, with a configurable max file limit (50–1000).
  - Note: Importing/restoring directly from GitHub to NinjaOne is planned for future releases.

- **Template Library Comparison and Update**
  - Compare scripts in your NinjaOne environment with scripts available in the NinjaOne Template Library.
  - Instantly see which templates are missing, outdated, or changed, and update your environment with a single click.
  - Now even faster, with more accurate detection and improved workflow.

- **Expanded Domain Support**
  - SHADOW now works with all NinjaOne & RMM environments worldwide (including Europe, Canada, Australia, and multi-tenant domains).

- **Settings Panel for GitHub Integration**
  - Enter your GitHub token, repository, branch, and path in the extension popup.
  - Secure storage and encryption for your credentials.

- **Direct UI Integration in NinjaOne**
  - SHADOW injects custom buttons and overlays in the NinjaOne web interface for direct access to all features.

- **Auto-Refresh & UX Improvements**
  - More reliable auto-refresh after compare/upload actions, with improved feedback and smooth overlays/popups.

- **Advanced Security**
  - Secure storage for sensitive data, with AES-GCM encryption for GitHub tokens.
  - All user inputs are strictly validated and sanitized.
  - Strict permissions and a hardened content security policy.

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
3. Test and save your settings.

---

## Typical Workflows

- **Backup:** Export all scripts or monitoring policies from NinjaOne to a local ZIP archive.
- **Bulk Update:** Import multiple scripts or policies by selecting one or more unpacked JSON files for upload to NinjaOne.
- **Audit/Compare:** Instantly compare scripts or policies in NinjaOne with versions in your GitHub repository to identify differences and maintain consistency.

---

## Security

- GitHub tokens are stored encrypted using AES-GCM and never in plaintext.
- Strict permissions and hardened content security policy by default.
- All user inputs are validated and sanitized.

---

## Release Notes

Release notes for SHADOW can be found [here](./Release%20Notes.md).

---

## Who Benefits from SHADOW?

- **NinjaOne administrators** with multiple tenants or environments.
- **IT teams and MSPs** using version control for scripts and policies.
- Anyone who wants a fast, reliable, and auditable workflow for automation content in NinjaOne.

---

## Support

For questions, feedback, or feature requests, please open an issue in this repository or contact the maintainer.

---

*SHADOW is an independent project and not affiliated with NinjaOne, LLC.*
