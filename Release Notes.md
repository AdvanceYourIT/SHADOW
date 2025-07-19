# SHADOW – Release Notes

## v1.2

### New Features
- **Encrypted GitHub Token Storage:**  
  GitHub tokens are now securely stored using AES-GCM 256-bit encryption.
- **Automatic Token Migration:**  
  Plaintext tokens from previous versions are automatically migrated to encrypted storage.
- **Comprehensive Input Validation:**  
  All GitHub integration settings are now strictly validated and sanitized before saving.
- **Enhanced UI Feedback:**  
  Improved error and status messaging, with generic user-facing errors and detailed internal logging.
- **Improved Script Injection Security:**  
  Validation and protection against malicious or failed dynamic script injections.

### Security Improvements
- **Strict Content Security Policy:**  
  Now includes `frame-ancestors 'none'` and reduced script/connect sources.
- **Scoped Host Permissions:**  
  Permissions limited to only necessary NinjaOne and GitHub API domains.
- **Web Resource Exposure Reduced:**  
  Sensitive scripts are no longer exposed to web pages.
- **Selective Credential Handling:**  
  API requests only include credentials when required.
- **Sanitized Error Messages:**  
  Technical details are never exposed to users or web pages.
- **Cross-Origin Message Validation:**  
  Only accepts and processes messages from trusted origins and with validated structure.

---

## v1.1

- Added live **Compare** functionality with GitHub.
    - Instantly compare scripts and monitoring policies in NinjaOne with versions in your private GitHub repository.
    - Visual difference highlighting and clear overview of changes.
- Introduced a new **Settings panel** in the extension popup for entering and saving your GitHub repository, token, branch, and target path.
- Improved user interface, both in the NinjaOne integration and in the extension popup.
- Updated documentation and onboarding instructions for GitHub integration.
- General performance and reliability improvements.

---

## v1.0

- Initial release of SHADOW for Microsoft Edge.
- **Bulk Export** of all scripts and monitoring policies from NinjaOne as a ZIP archive.
- **Bulk Import** of multiple scripts or policies into NinjaOne from local unpacked JSON files.
- Direct integration of SHADOW buttons and overlays within the NinjaOne web application.
- Informative extension popup and streamlined workflow.
