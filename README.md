# PyServiceTemplate

**PyServiceTemplate** is a lightweight boilerplate for deploying Python scripts as background systemd services on Linux. It automates the creation of virtual environments, dependency management, and service registration.

## 📋 Features

* **Automated Virtual Environment:** Creates an isolated environment using `virtualenv`.
* **Systemd Integration:** Automatically generates, enables, and starts a systemd service file.
* **Clean Uninstallation:** Handles the removal of the service daemon, the virtual environment, and compiled bytecode (`__pycache__`).
* **Wrapper Script:** Includes a `pystart` wrapper to handle environment activation before execution.

## 🛠️ Prerequisites

* Linux OS with `systemd`
* Python 3 installed
* `virtualenv` installed (`pip3 install virtualenv`)
* Root privileges (required for service registration)

## ⚙️ Configuration

Before running the scripts, you must configure the variables at the top of the files to match your specific project details.

### 1. Configure `install`
Open the `install` file and set the following:
* `APP_NAME`: The name of the directory for your virtual environment.
* `PY_PACKAGE`: The Python package/library you wish to install via pip.
* `DESC`: A short description for the systemd service unit.
* `SERVICE_NAME`: The name the service will use in systemd (e.g., `my-script.service`).

### 2. Configure `pystart`
Open the `pystart` file and set the following:
* `APP_NAME`: Must match the `APP_NAME` used in the `install` script.
* `PYTHON_FILE_PATH`: The path to the specific Python script you want to execute.

### 3. Configure `uninstall`
Open the `uninstall` file and set the following:
* `APP_NAME`: Must match the directory name created during installation.
* `SERVICE_NAME`: Must match the service name defined in the `install` script.

## 🚀 Usage

### Installation
1.  Make the scripts executable:
    ```bash
    chmod +x install uninstall pystart
    ```
2.  Run the installer with root privileges:
    ```bash
    sudo ./install
    ```
    This will create the virtual environment, install dependencies, and start the service immediately.

### Uninstallation
To stop the service and clean up generated files:
1.  Run the uninstaller with root privileges:
    ```bash
    sudo ./uninstall
    ```
    This disables the systemd service, removes the unit file, and deletes the virtual environment directory.

## 📂 Project Structure

```text
.
├── install       # Setup script (root required)
├── uninstall     # Cleanup script (root required)
├── pystart       # Service entry point wrapper
└── ...           # Your Python scripts
```
## 📝 Notes
* **User Context:** The service is configured to run under the user who executes the `install` script (`User=$USER`).
* **Root Check:** Both `install` and `uninstall` scripts enforce execution as root to ensure permission to write to `/etc/systemd/system/`.
