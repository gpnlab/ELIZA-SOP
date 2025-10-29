
# Standard Operating Procedure: SSH Setup for Open WebUI Access

### Purpose
This SOP provides step-by-step instructions for securely connecting to the Mac Studio ("eliza") via SSH to access the Open WebUI application. This is required for remote administration and troubleshooting of the Open WebUI service.

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

## Troubleshooting
| Issue                     | Solution                                                                                             |
| ------------------------- | ---------------------------------------------------------------------------------------------------- |
| "Connection refused"      | Verify Remote Login is enabled in System Settings > Sharing                                          |
| "Permission denied"       | Check username and password, or verify SSH key is properly set up                                    |
| Can't find IP address     | Check network settings on Eliza or use `ifconfig` on Eliza                                           |
| Open WebUI not accessible | Confirm service is running with `ps aux                                          \| grep open-webui` |
| Firewall blocking SSH     | Check macOS Firewall settings in System Settings > Privacy & Security > Firewall                     |

## Maintenance
- **Regularly update SSH configuration** to maintain security
- **Monitor SSH access logs**:
  ```bash
  tail -f /var/log/system.log | grep sshd
  ```
- **Check if ollama and Open WebUI services are running**:
  ```bash
  ps aux | grep ollama
  ps aux | grep open-webui
  ```
  - **Restart services if needed**:
    ```bash
    sudo launchctl unload /Library/LaunchDaemons/com.ollama.daemon.plist
    sudo launchctl load /Library/LaunchDaemons/com.ollama.daemon.plist
    sudo launchctl unload /Library/LaunchDaemons/com.openwebui.daemon.plist
    sudo launchctl load /Library/LaunchDaemons/com.openwebui.daemon.plist
    ```
- **If documents cannot be added to Open WebUI, Check if Docling is running**
  ```bash
   ps aux | grep docling
   ```
  - **Restart Docling-serve if needed**:
      ```bash
      sudo launchctl unload /Library/LaunchDaemons/org.docling.serve.plist
      sudo launchctl load /Library/LaunchDaemons/org.docling.serve.plist
      ```

## References
- [Apple Support: Enable Remote Login](https://support.apple.com/en-us/HT201717)
- [Open WebUI Documentation: Installation](https://docs.openwebui.com/installation)
- [SSH Security Best Practices](https://www.ssh.com/ssh/best-practices)

This SOP should be reviewed quarterly and updated as needed to reflect changes in SSH configuration or security policies.
