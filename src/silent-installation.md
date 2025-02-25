# Silent Installation

## 4. Silent Installation

### 4.1 Silent Installation Package
The silent installation package is delivered by Zucchetti into a common accessibly gdrive folder

### 4.2 Command-Line Parameters
The following command-line parameters are available for the silent installer:

- `--install-dir <path>`: Specifies the installation directory. (Required)
- `--config-file <path>`: Path to the configuration file. (Optional)
- `--log-file <path>`: Path to the installation log file. (Optional)
- `--silent`: Runs the installer in silent mode. (Optional)

### 4.3 Example Silent Installation Commands
Here are some examples of command-line commands for different installation scenarios:

1. Basic silent installation:
   ```
   POS_Application_Silent_Installer.exe --install-dir "C:\Program Files\POS Application" --silent
   ```

2. Silent installation with a custom configuration file:
   ```
   POS_Application_Silent_Installer.exe --install-dir "C:\Program Files\POS Application" --config-file "C:\configs\config.json" --silent
   ```

3. Silent installation with logging:
   ```
   POS_Application_Silent_Installer.exe --install-dir "C:\Program Files\POS Application" --log-file "C:\logs\install.log" --silent
   ```

### 4.4 Installation Log
The installation log is located in the specified log file path or defaults to `C:\Program Files\POS Application\install.log`. This log contains detailed information about the installation process and can be used for troubleshooting.

### 4.5 Troubleshooting Silent Installation
Common issues encountered during silent installation include:

- **Insufficient Permissions**: Ensure that you have administrative privileges to run the installer.
- **Invalid Installation Directory**: Verify that the specified installation directory exists and is accessible.
- **Configuration File Errors**: Check the syntax and validity of the provided configuration file.

For further assistance, refer to the support contact information in the appendix.