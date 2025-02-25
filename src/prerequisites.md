# Prerequisites

This document outlines the prerequisites for the POS application, including the necessary 3rd-party components.

## 3rd-Party Components 1st delivery from Zucchetti

### Cygwin64
- **Delivered by**: ZUC
- **Version**: 2.876
- **Path**: `c:\eurosuite\cygwin64\`

### Java 
- **Delivered by**: Download - optional
- **Version**: OpenJDK version "1.8.0_402"
- **Install File**: `OpenJDK8U-jdk_x64_windows_hotspot_8u402b06.msi`
- **Path**: Standard path (to change path if needed requires a change request)

### Fonts 
- **Delivered by**: ZUC
- **DejaVu (all)
- **OpenSans (all)
- **Location**: `c:\eurosuite\Fonts\`

### Erlang 
- **Delivered by**: Download - optional
- **Version**: 26.2.2.5
- **Install File**: `otp_win64_26.2.5.5.exe`
- **Path**: `c:\programme\`

### Python 
- **Delivered by**: Download - optional
- **Version**: 3.11.1
- **Install File**: `python-3.11.1-amd64.exe`
- **Path**: `c:\Python311`

### RabbitMQ 
- **Delivered by**: Download - optional
- **Version**: 3.13.7
- **Install File**: `rabbitmq-server-3.13.7.exe`
- **Silent Installer**: `rmq-main-installer-silent.bat` (includes Erlang and Python)
- **Path**: `c:\programme\RabbitMQ Server\`

### JavaPOS Driver (ZUC)
- **Delivered by**: ZUC
- **Components**:
  - Scanner
  - Printer & Till

### Retailforce (ZUC)
- **Delivered by**: ZUC
- **Version**: 
- **Install File**: ``
- **Path**: ``

## Verification

To verify that each component has been installed correctly, perform the following checks:

- **Cygwin64:** Ensure the path `c:\eurosuite\cygwin64\` exists and is accessible.
- **Java:** Run `java -version` in the command line to check the installed version.
- **Fonts:** Verify that the fonts are installed in the `c:\eurosuite\Fonts\` directory.
- **Erlang:** Run `erl` in the command line to check the installed version.
- **Python:** Run `python --version` in the command line to check the installed version.
- **RabbitMQ:** Open a web browser and navigate to `http://localhost:15672`. You should see the RabbitMQ management interface.
- **Retailforce:** Verify that the application is installed in the `c:\eurosuite\\` directory.
- **JavaPOS Driver:** Verify that the scanner, printer, and till components are functioning correctly.