# Troubleshooting

## Restart Protocol
When restarting the machine please check the following items:
- The device is connected to the internet
- globalprotect is running
- ollama is running
- openwebui is starting 

## General Troubleshooting

1. Check the logs for more details:
  ```bash
  tail -n 100 /var/log/ollama.stderr.log
  tail -n 100 /var/log/ollama.stdout.log
  tail -n 100 /var/log/openwebui/open-webui.stderr.log
  tail -n 100 /var/log/openwebui/open-webui.stdout.log
  ```
2. Ollama not responding: Restart the Ollama service:
  ```bash
  sudo launchctl unload /Library/LaunchDaemons/com.ollama.daemon.plist
  sudo launchctl load /Library/LaunchDaemons/com.ollama.daemon.plist
  ```
3. Open WebUI not accessible: Confirm the service is running with `ps aux | grep open-webui`.
4. "Connection refused": Verify Remote Login is enabled in System Settings > Sharing.
5. "Permission denied": Check username and password, or verify SSH key is properly set up.
6. Can't find IP address: Check network settings on Eliza or use `ifconfig` on Eliza.

7. Firewall blocking SSH: Check macOS Firewall settings in System Settings > Privacy & Security > Firewall.
# Maintenance
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
## Upgrading Ollama and Open WebUI
- unload services before upgrading:
  ```bash
  sudo launchctl unload /Library/LaunchDaemons/com.ollama.daemon.plist
  sudo launchctl unload /Library/LaunchDaemons/com.openwebui.daemon.plist
  ```
- Ollama:
  - download the dmg from [Ollama Releases](https://ollama.com/download) and follow installation instructions.
- Open WebUI:
  ```bash
  # upgrade using pip
  pip3.11 install --upgrade open-webui
  sudo launchctl load /Library/LaunchDaemons/com.openwebui.daemon.plist
  ```

## References
- [Apple Support: Enable Remote Login](https://support.apple.com/en-us/HT201717)
- [Open WebUI Documentation: Installation](https://docs.openwebui.com/installation)
- [SSH Security Best Practices](https://www.ssh.com/ssh/best-practices)
