# SHADOW – Release Notes

## v1.3

### New Features
- **Template Library Compare & Update:**  
  SHADOW now allows you to directly compare scripts in your NinjaOne environment with those available in the NinjaOne Template Library.  
  Instantly identify missing, outdated, or changed templates and update your scripts from the template library with a single click.
- **Improved GitHub Connection Testing:**  
  Enhanced reliability and feedback when testing the GitHub connection from the popup interface.
- **Better Auto-Refresh for Compare:**  
  Improved and more reliable auto-refresh after uploading or updating scripts and policies, ensuring the latest status is always visible.
- **Expanded Validation for User Inputs:**  
  Even stricter validation and sanitization for all GitHub settings fields, including more advanced edge cases and error handling for the popup UI.
- **Performance Enhancements:**  
  Noticeably faster loading and smoother interactions in both the NinjaOne overlays and the extension popup.
- **UI/UX Improvements:**  
  Refined status messages, better accessibility, and a more modern look and feel in the popup and overlays.
- **Improved Handling for Large GitHub Repositories:**  
  Optimized processing and pagination when working with repositories with hundreds or thousands of scripts or policies.
- **Enhanced Security Controls:**  
  Security logic updated in line with recent recommendations for browser extensions and GitHub token handling (see SECURITY.md for details).

### Changed
- **Updated Content Security Policy and Permissions:**  
  Ensured compliance with latest best practices for extension security, including stricter host permissions and CSP.

### Fixed
- **Resolved Edge Cases in Import/Export Flows:**  
  Bugfixes around rare situations when importing/exporting large amounts of data or working with unusual filenames.
- **Minor Bugfixes and Stability Tweaks:**  
  Numerous small issues addressed, resulting in a more robust and reliable extension experience.

---

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

### New Features
- Added live **Compare** functionality with GitHub.
    - Instantly compare scripts and monitoring policies in NinjaOne with versions in your private GitHub repository.
    - Visual difference highlighting and clear overview of changes.
- Introduced a new **Settings panel** in the extension popup for entering and saving your GitHub repository, token, branch, and target path.
- Improved user interface, both in the NinjaOne integration and in the extension popup.
- Updated documentation and onboarding instructions for GitHub integration.
- General performance and reliability improvements.

---

## v1.0

### New Features
- Initial release of SHADOW for Microsoft Edge.
- **Bulk Export** of all scripts and monitoring policies from NinjaOne as a ZIP archive.
- **Bulk Import** of multiple scripts or policies into NinjaOne from local unpacked JSON files.
- Direct integration of SHADOW buttons and overlays within the NinjaOne web application.
- Informative extension popup and streamlined workflow.
