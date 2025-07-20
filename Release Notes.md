# SHADOW – Release Notes

## v1.4

### New Features
- **Expanded Domain Support:**  
  SHADOW now supports all global NinjaOne and RMM domains, including new European, Canadian, Australian, and multi-tenant environments.
- **Template Library Compare & Update Enhancements:**  
  Improved comparison and update workflow for scripts from the NinjaOne Template Library.  
  Faster detection of missing/outdated templates and one-click updates to your environment.
  - **Note:** The basic “Template Library Compare & Update” feature was introduced in v1.3; this release further improves speed, accuracy, and usability.
- **Advanced GitHub Integration:**  
  Enhanced feedback and stricter input validation in the GitHub settings popup, with real-time validation and clearer error messages.
  - **Note:** Improved GitHub connection testing and user input validation were already introduced in v1.3; this release brings even stricter and more interactive validation.
- **Auto-Refresh Improvements:**  
  More reliable and consistent auto-refresh after compare or upload operations, so your view is always up to date.
  - **Note:** Auto-refresh improvements expand on the “Better Auto-Refresh for Compare” added in v1.3.
- **Paginated & Optimized Large Repository Handling:**  
  Handles very large repositories and script lists with improved pagination and async processing for faster, more stable operation.
  - **Note:** This extends the “Improved Handling for Large GitHub Repositories” of v1.3 with even better stability for high-volume use cases.
- **Selectable Max Files Limit:**  
  Now allows users to configure the maximum number of GitHub files processed (50–1000) for better performance control.
- **Comprehensive Input Validation:**  
  All user input, including settings and script data, is strictly validated and sanitized before use.
  - **Note:** Comprehensive validation was introduced in v1.3; this release makes it even stricter and broader.
- **User Experience Tweaks:**  
  Smoother popups and overlays, more accessible messages, and improved error handling throughout the UI.

### Security & Stability
- **Stronger Input Validation and Sanitization:**  
  All user-supplied data is now validated both client- and server-side, minimizing attack surface and improving reliability.
- **Global Token Encryption & Migration:**  
  Further optimized secure storage and automatic repair/migration for GitHub tokens, ensuring all tokens are encrypted and legacy issues are self-healing.
- **Strict Content Security Policy and Permissions:**  
  CSP and extension permissions further hardened in line with latest best practices for browser extensions.
- **Improved Error Handling & Logging:**  
  All error messages are now sanitized for the user, with internal logs kept for diagnostics.

### Bugfixes & Improvements
- **Robust Import/Export:**  
  Fixed issues with special characters and rare edge cases in filenames, and made the import/export routines more resilient to API or network hiccups.
- **More Efficient Script Injection:**  
  Better handling and sequencing of dynamic scripts for improved performance and safety.
- **Template Library Sync Fixes:**  
  Improved consistency in syncing with the Template Library, even when scripts are renamed or moved.

---

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
