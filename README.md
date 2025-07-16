# SHADOW – NinjaOne Automation Extension

**SHADOW** (**S**tealthy **H**andler for **A**utomated **D**ata **O**ptimization and **W**orkflow)  
A Microsoft Edge browser extension for fast, auditable management of NinjaOne scripts and monitoring policies, including live GitHub comparison.

<img width="128" height="128" alt="shadow_128x128" src="https://github.com/user-attachments/assets/5f28a902-69eb-47b7-86d0-d0263103a35b" />

---

## Features

- **Bulk Export of Scripts & Monitoring Policies**
  - Export all scripts or policies from NinjaOne with one click.
  - Download as a ZIP archive for easy backup, documentation, or migration.

- **Bulk Import of Scripts & Monitoring Policies**
  - Import multiple scripts or policies at once using unpacked JSON files from your computer.
  - Perfect for onboarding, migration, or updating your automation set.

- **Live Compare with GitHub**
  - Instantly compare scripts and policies in NinjaOne with versions in your private GitHub repository.
  - See differences and keep your automation libraries aligned.
  - _Note: SHADOW does **not** yet support restoring/importing from GitHub to NinjaOne—this is planned for future versions._

- **Settings Panel for GitHub Integration**
  - Enter and save your GitHub token, repository, branch, and target path securely in the extension popup.

- **Direct NinjaOne UI Integration**
  - SHADOW injects custom buttons and overlays in the NinjaOne web interface for direct access to all features.

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
4. Give your token a clear name (e.g., `SHADOW Extension`).
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

## Who Benefits from SHADOW?

- **NinjaOne administrators** with multiple tenants or environments.
- **IT teams and MSPs** using version control for scripts and policies.
- Anyone who wants a fast, reliable, and auditable workflow for automation content in NinjaOne.

---

## Security Notice

- Your GitHub token is stored locally and is only used by SHADOW for GitHub API access in your browser session.
- Use private repositories for all sensitive automation code and configuration.
- Rotate or revoke unused tokens regularly.

---

## Support

For questions, feedback, or feature requests, please open an issue in this repository or contact the maintainer.

---

## Release Notes

Release notes for SHADOW can be found [here](https://github.com/AdvanceYourIT/SHADOW/blob/main/Release%20Notes.md).

---

*SHADOW is an independent project and not affiliated with NinjaOne, LLC.*
