# Auto-Update System for NPTintingFrontend

This document explains how to set up and use the auto-update system for the NPTintingFrontend application.

## Overview

The auto-update system allows you to push updates to customers without manually distributing executable files. The system includes:

- **Automatic update checking** on application startup
- **Manual update checking** through the Settings menu
- **Update configuration** for administrators
- **User-friendly update dialogs** with progress tracking
- **Version management** and update history

## Setup Instructions

### 1. Update Server Setup

You need to host a JSON file that contains update information. The file should be accessible via HTTP/HTTPS.

#### Example Update Server JSON Structure:

```json
{
  "Version": "1.0.1.0",
  "DownloadUrl": "https://your-server.com/downloads/NPTintingFrontend-1.0.1.0.exe",
  "FileSize": 5242880,
  "ReleaseNotes": "Bug fixes and performance improvements",
  "ReleaseDate": "2024-01-15T10:00:00Z",
  "IsMandatory": false,
  "FileName": "NPTintingFrontend-1.0.1.0.exe"
}
```

#### Field Descriptions:

- **Version**: The version number of the update (must be higher than current version)
- **DownloadUrl**: Direct download link to the new executable
- **FileSize**: Size of the update file in bytes
- **ReleaseNotes**: Description of changes in this update
- **ReleaseDate**: When the update was released
- **IsMandatory**: Whether users can skip this update
- **FileName**: Name of the executable file (optional, will auto-generate if not provided)

### 2. Application Configuration

#### Update URL Configuration:

1. Open the application
2. Go to **Settings** → **Updates**
3. Enter your update server URL (e.g., `https://your-server.com/updates.json`)
4. Configure other settings as needed
5. Click **Save**

#### Configuration Options:

- **Update Server URL**: URL to your JSON update file
- **Auto Check for Updates**: Enable/disable automatic update checking
- **Check Interval**: How often to check for updates (in days)
- **Current Version**: Shows the current application version
- **Last Update Check**: Shows when updates were last checked

### 3. Building and Deploying Updates

#### Step 1: Build Your Update

1. Make your changes to the application
2. Update the version number in `Properties/Settings.settings`:
   ```xml
   <Setting Name="CurrentVersion" Type="System.String" Scope="Application">
     <Value Profile="(Default)">1.0.1.0</Value>
   </Setting>
   ```
3. Build the application in Release mode
4. The executable will be in `bin/Release/NPTintingFrontend.exe`

#### Step 2: Upload to Server

1. Upload the new executable to your web server
2. Update your JSON file with the new version information
3. Ensure the download URL points to the correct file

#### Step 3: Test the Update

1. Run an older version of the application
2. Go to **Settings** → **Updates**
3. Click **Check for Updates Now**
4. Verify the update dialog appears with correct information
5. Test the download and installation process

## User Experience

### Automatic Updates

- The application checks for updates on startup (if enabled)
- If an update is available, a dialog appears with update information
- Users can choose to:
  - **Update Now**: Download and install the update
  - **Remind Later**: Check again later
  - **Skip This Version**: Never show this version again

### Manual Updates

- Users can manually check for updates via **Settings** → **Updates**
- Administrators can configure update settings
- Test connection functionality to verify server connectivity

### Update Process

1. **Download**: The application downloads the new executable
2. **Backup**: Current executable is backed up
3. **Replace**: New executable replaces the old one
4. **Restart**: Application restarts with the new version
5. **Cleanup**: Temporary files are removed

## Security Considerations

### File Integrity

- Consider implementing file hash verification
- Use HTTPS for all update communications
- Sign your executables with a trusted certificate

### Update Server Security

- Ensure your update server is secure
- Implement proper access controls
- Monitor for unauthorized access

### User Permissions

- The application needs write permissions to its installation directory
- Consider running as administrator for system-wide installations

## Troubleshooting

### Common Issues

1. **Update not found**: Check the JSON file format and URL
2. **Download fails**: Verify the download URL is accessible
3. **Installation fails**: Check file permissions and antivirus software
4. **Version comparison issues**: Ensure version numbers follow semantic versioning

### Debug Information

The application logs update-related activities to the debug output. Check the debug console for:
- Update check attempts
- Download progress
- Installation steps
- Error messages

### Configuration Reset

If the update system becomes misconfigured:
1. Delete the application settings file
2. Restart the application
3. Reconfigure the update settings

## Advanced Features

### Custom Update Servers

You can implement custom update logic by:
1. Creating a custom `IUpdateService` implementation
2. Modifying the `UpdateService` class
3. Adding additional update channels

### Update Notifications

The system supports:
- Progress tracking during downloads
- Status updates during installation
- Error reporting for failed updates

### Version Management

- Automatic version comparison
- Skipped version tracking
- Update history (can be extended)

## Support

For issues with the update system:
1. Check the debug output for error messages
2. Verify server connectivity
3. Test with a simple update first
4. Contact your system administrator

## Example Update Server Implementation

Here's a simple example using Node.js and Express:

```javascript
const express = require('express');
const app = express();

app.get('/updates.json', (req, res) => {
  res.json({
    "Version": "1.0.1.0",
    "DownloadUrl": "https://your-server.com/downloads/NPTintingFrontend-1.0.1.0.exe",
    "FileSize": 5242880,
    "ReleaseNotes": "Bug fixes and performance improvements",
    "ReleaseDate": "2024-01-15T10:00:00Z",
    "IsMandatory": false,
    "FileName": "NPTintingFrontend-1.0.1.0.exe"
  });
});

app.listen(3000, () => {
  console.log('Update server running on port 3000');
});
```

This provides a complete auto-update solution that can be easily integrated into your existing deployment workflow. 
