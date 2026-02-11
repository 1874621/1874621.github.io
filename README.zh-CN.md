# 1874621.github.io

使用 [Hexo](https://hexo.io/) 和 [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly) 主题搭建的个人博客。

## VSCode Copilot 网络错误解决方案

### 错误：ERR_CONNECTION_CLOSED

如果遇到以下错误信息：
```
抱歉，出现网络错误。请稍后重试。
原因: Please check your firewall rules and network connection then try again. 
Error Code: net::ERR_CONNECTION_CLOSED
```

这是 VSCode 与 GitHub Copilot 服务之间的网络连接问题。以下是解决方案：

#### 常见解决方法

1. **检查网络连接**
   - 确保网络连接稳定
   - 尝试断开并重新连接网络
   - 如果使用 VPN，尝试暂时关闭

2. **防火墙配置**
   - 为 VSCode 和 GitHub Copilot 添加例外规则：
     - `*.github.com`
     - `*.githubusercontent.com`
     - `copilot-proxy.githubusercontent.com`
     - `api.github.com`
   - 允许 HTTPS 出站流量（端口 443）

3. **代理设置**
   - 如果在公司代理后面，配置 VSCode 代理设置：
     ```json
     {
       "http.proxy": "http://proxy.company.com:端口",
       "http.proxyStrictSSL": false
     }
     ```
   - 添加到 VSCode 的 settings.json（文件 > 首选项 > 设置 > 搜索 "proxy"）

4. **证书问题**
   - 某些公司网络使用 SSL 检查
   - 尝试设置：`"http.proxyStrictSSL": false`
   - 或导入公司的 SSL 证书

5. **重置 VSCode 扩展**
   - 禁用 GitHub Copilot 扩展
   - 重新加载 VSCode
   - 重新启用扩展
   - 重新登录

6. **清除 VSCode 缓存**
   ```bash
   # Windows
   %APPDATA%\Code\User\globalStorage\github.copilot
   
   # macOS
   ~/Library/Application Support/Code/User/globalStorage/github.copilot
   
   # Linux
   ~/.config/Code/User/globalStorage/github.copilot
   ```
   - 删除 copilot 文件夹
   - 重启 VSCode

7. **检查 GitHub Copilot 服务状态**
   - 访问 [GitHub 状态页面](https://www.githubstatus.com/)
   - 检查 Copilot 服务是否正常

8. **更新软件**
   - 更新 VSCode 到最新版本
   - 更新 GitHub Copilot 扩展
   - 重启电脑

#### 网络要求

GitHub Copilot 需要访问：
- `https://api.github.com`（端口 443）
- `https://copilot-proxy.githubusercontent.com`（端口 443）
- `https://*.githubusercontent.com`（端口 443）

#### 仍然有问题？

1. 检查 VSCode 输出面板：查看 > 输出 > GitHub Copilot
2. 在设置中启用日志记录：
   ```json
   {
     "github.copilot.advanced": {
       "debug.overrideEngine": "gpt-4",
       "debug.showScores": true
     }
   }
   ```
3. 向 [GitHub Copilot 支持](https://support.github.com/) 报告问题

## 博客开发

这是由 Hexo 生成的静态网站。源文件通常在单独的仓库或分支中，此仓库包含为 GitHub Pages 生成的 HTML、CSS 和 JavaScript 文件。

### 访问网站

访问：https://1874621.github.io/

---

有关 Hexo 的更多信息，请访问[官方文档](https://hexo.io/zh-cn/docs/)。
