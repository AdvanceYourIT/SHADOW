# SHADOW – Release Notes

## v1.0.5

### New Features

- **Expanded Domain Support**  
  SHADOW now supports all global NinjaOne and RMM domains, including new European, Canadian, Australian, and multi-tenant environments.
- **Template Library Compare & Update Enhancements**  
  Improved comparison and update workflow for scripts from the NinjaOne Template Library. Faster detection of missing/outdated templates and one-click updates.
- **Custom Fields Export & Import**  
  You can now export and import custom fields for Devices, Organizations, and Locations, making it easy to migrate, back up, or synchronize custom metadata between NinjaOne environments.
- **Advanced GitHub Integration**  
  Enhanced feedback and stricter input validation in the GitHub settings popup, including real-time validation and clearer error messages.
- **Auto-Refresh Improvements**  
  More reliable and consistent auto-refresh after compare or upload operations, so your view is always up to date.
- **Paginated & Optimized Large Repository Handling**  
  Handles very large repositories and script lists with improved pagination and async processing for faster, more stable operation.
- **Selectable Max Files Limit**  
  Users can now configure the maximum number of GitHub files processed (50–1000) for better performance control.
- **Comprehensive Input Validation**  
  All user input, including settings and script data, is strictly validated and sanitized before use.
- **User Experience Tweaks**  
  Smoother popups and overlays, more accessible messages, and improved error handling throughout the UI.

### Security & Stability

- **Stronger Input Validation and Sanitization**  
  All user-supplied data is now validated both client- and server-side, minimizing attack surface and improving reliability.
- **Global Token Encryption & Migration**  
  Further optimized secure storage and automatic repair/migration for GitHub tokens, ensuring all tokens are encrypted and legacy issues are self-healing.
- **Strict Content Security Policy and Permissions**  
  CSP and extension permissions further hardened in line with latest best practices for browser extensions.
- **Improved Error Handling & Logging**  
  All error messages are now sanitized for the user, with internal logs kept for diagnostics.

### Bugfixes & Improvements

- **Robust Import/Export**  
  Fixed issues with special characters and rare edge cases in filenames, and made the import/export routines more resilient to API or network hiccups.
- **More Efficient Script Injection**  
  Better handling and sequencing of dynamic scripts for improved performance and safety.
- **Template Library Sync Fixes**  
  Improved consistency in syncing with the Template Library, even when scripts are renamed or moved.

---
