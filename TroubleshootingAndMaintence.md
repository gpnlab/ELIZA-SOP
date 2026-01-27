# Troubleshooting
- "Connection refused": Verify Remote Login is enabled in System Settings > Sharing.
- "Permission denied": Check username and password, or verify SSH key is properly set up.
- Can't find IP address: Check network settings on Eliza or use `ifconfig` on Eliza.
- Open WebUI not accessible: Confirm the service is running with `ps aux | grep open-webui`.
- Firewall blocking SSH: Check macOS Firewall settings in System Settings > Privacy & Security > Firewall.

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

## References
- [Apple Support: Enable Remote Login](https://support.apple.com/en-us/HT201717)
- [Open WebUI Documentation: Installation](https://docs.openwebui.com/installation)
- [SSH Security Best Practices](https://www.ssh.com/ssh/best-practices)
