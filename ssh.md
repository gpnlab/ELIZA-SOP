
# Standard Operating Procedure: SSH Setup for Open WebUI Access

### Purpose
This SOP provides step-by-step instructions for securely connecting to the Mac Studio ("eliza") via SSH to access the Open WebUI application. This is required for remote administration and troubleshooting of the Open WebUI service. This should be a backup access method in case the url is not available.

### Prerequisites
- Terminal application installed on your local machine
- Basic understanding of command line operations
- Network access to the Mac Studio's local IP address
- GlobalProtect VPN access if connecting from outside the Pitt Network
- Request an account for open-webui through Linghai (liw82@pitt.edu or slack)

## Step-by-Step SSH Setup Procedure


### 1. Verify connection to Eliza via SSH
**On your local machine (macOS/Linux):**
```bash
ssh username@oac-eliza.oac.pitt.edu
```
Replace "username" with your Eliza login account name, and "oac-eliza.oac.pitt.edu" with the actual IP address. On MacOS or linux you can use the terminal application and on Windows you can use PowerShell, an SSH client like PuTTY, or the terminal application. 

After the connection is verified, you can exit the SSH session with the following command:
```bash
exit
```

### 2. Access Open WebUI via SSH Tunneling
 
If you cannot access Open WebUI directly through a browser:
```bash
ssh -L 8080:localhost:8080 username@oac-eliza.oac.pitt.edu
```
This forwards port 8080 on your local machine to the Open WebUI server on the Mac Studio.

Then open your browser and go to:
<http://localhost:8080>

### 3. Security Considerations (optional but recommended)
- **Use SSH key authentication instead of passwords** (more secure):
  ```bash
  # On your local machine (macos/linux):
  ssh-keygen -t ed25519
  ssh-copy-id username@oac-eliza.oac.pitt.edu

  # PowerShell (Windows 10+ with OpenSSH)
    Start-Service ssh-agent
    ssh-add $env:USERPROFILE\.ssh\id_ed25519
  ```

- **Troubleshooting** 


If the SSH key is not being used (i.e. you are still prompted to enter a password):

1. Check the SSH connection with verbose output:
    ```bash
    ssh -v username@remote.host     
    ```
2. Try disabling password authentication on the server:
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    Set `PasswordAuthentication no` and restart SSH:
    ```bash
    sudo systemctl restart sshd
    ```

# Contributing To This Document
Contributions to this SOP are welcome. Please follow these guidelines when contributing:
- Fork the repository and create a new branch for your changes.
- Submit a pull request with a clear description of the changes made.
- Ensure all changes are reviewed and approved before merging.
- Merge Requests should be reviewed by at least one other team member before merging.
- The GitHub Pages will automatically update upon merging.