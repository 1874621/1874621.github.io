# 1874621.github.io

Personal blog powered by [Hexo](https://hexo.io/).

## VSCode Copilot Network Error Troubleshooting

If you encounter the error `net::ERR_CONNECTION_CLOSED` when using GitHub Copilot in VSCode, here are some common solutions:

### Error Message
```
抱歉，出现网络错误。请稍后重试。
原因: Please check your firewall rules and network connection then try again. 
Error Code: net::ERR_CONNECTION_CLOSED
```

### Common Causes and Solutions

#### 1. Network Connectivity Issues
- **Check your internet connection**: Ensure you have a stable internet connection
- **Restart your router/modem**: Sometimes a simple restart can resolve connection issues
- **Try a different network**: If possible, test on a different network to isolate the issue

#### 2. Firewall or Proxy Issues
- **Check firewall settings**: Ensure your firewall allows connections to `*.github.com` and `*.githubcopilot.com`
- **Configure proxy settings**: If you're behind a corporate proxy, configure VSCode to use it:
  ```json
  {
    "http.proxy": "http://your-proxy:port",
    "http.proxyStrictSSL": false
  }
  ```
- **Disable VPN temporarily**: Some VPNs can interfere with Copilot connections

#### 3. VSCode and Copilot Extension Issues
- **Update VSCode**: Make sure you're running the latest version of VSCode
- **Update GitHub Copilot extension**: Check for updates in the Extensions marketplace
- **Reload VSCode**: Use `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac) → "Developer: Reload Window"
- **Re-authenticate**: Sign out and sign back in to GitHub Copilot:
  - Open Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P`)
  - Type "GitHub Copilot: Sign Out"
  - Then "GitHub Copilot: Sign In"

#### 4. DNS Issues
- **Flush DNS cache**:
  - Windows: `ipconfig /flushdns`
  - Mac: `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`
  - Linux: `sudo systemd-resolve --flush-caches`
- **Try different DNS servers**: Switch to Google DNS (8.8.8.8, 8.8.4.4) or Cloudflare DNS (1.1.1.1)

#### 5. Rate Limiting or Service Issues
- **Check GitHub Status**: Visit [GitHub Status](https://www.githubstatus.com/) to see if there are any ongoing issues
- **Wait and retry**: If you've been using Copilot extensively, you might hit temporary rate limits
- **Clear VSCode cache**: Delete the VSCode cache folder and restart

#### 6. SSL/TLS Certificate Issues
- **Update root certificates**: Ensure your system has up-to-date root certificates
- **Check system time**: Incorrect system time can cause certificate validation failures

### Additional Resources
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [VSCode Network Connections](https://code.visualstudio.com/docs/setup/network)
- [GitHub Support](https://support.github.com/)

### Still Having Issues?
If none of these solutions work, consider:
1. Checking VSCode logs: Help → Toggle Developer Tools → Console tab
2. Checking Copilot logs: Command Palette → "GitHub Copilot: View Logs"
3. Filing an issue on the [GitHub Copilot Discussion forum](https://github.com/orgs/community/discussions/categories/copilot)

---

## About This Site
This is a personal blog built with Hexo and hosted on GitHub Pages. The content includes technical articles and notes.
