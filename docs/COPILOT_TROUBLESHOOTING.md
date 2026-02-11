# VSCode Copilot ERR_CONNECTION_CLOSED 故障排查指南

## 问题描述

使用 VSCode Copilot 时出现以下错误：

```
抱歉，出现网络错误。请稍后重试。
请求 ID: 010accfb-8128-4353-9242-c300c140581f
原因: Please check your firewall rules and network connection then try again. 
Error Code: net::ERR_CONNECTION_CLOSED
```

**现象**：VSCode Copilot 使用一段时间后就报这个错误。

## 根本原因

此错误通常由以下原因引起：

1. **网络连接不稳定**：网络波动或间歇性断开
2. **防火墙阻止**：企业或个人防火墙阻止了 Copilot 的网络请求
3. **代理配置问题**：公司代理配置不当或需要认证
4. **SSL 证书问题**：企业网络的 SSL 检查干扰了连接
5. **服务端问题**：GitHub Copilot 服务暂时不可用
6. **VSCode 扩展缓存损坏**：扩展数据损坏导致连接失败

## 详细解决方案

### 方案 1：检查网络连接（最基础）

```bash
# Windows: 测试连接
ping github.com
ping api.github.com

# 测试 HTTPS 连接
curl https://api.github.com
```

**步骤**：
1. 打开命令提示符/终端
2. 运行上述命令
3. 如果无法连接，检查网络设置
4. 尝试切换网络（WiFi/有线）

### 方案 2：配置防火墙规则（常见解决方案）

#### Windows 防火墙

1. 打开 "Windows Defender 防火墙"
2. 点击 "允许应用通过防火墙"
3. 点击 "更改设置" > "允许其他应用"
4. 添加 VSCode（Code.exe）
5. 确保勾选"专用"和"公用"

#### macOS 防火墙

1. 系统偏好设置 > 安全性与隐私 > 防火墙
2. 点击锁图标解锁
3. 防火墙选项 > 添加 VSCode
4. 允许传入连接

#### 企业防火墙

需要 IT 管理员添加以下域名到白名单：
- `*.github.com`
- `*.githubusercontent.com`
- `copilot-proxy.githubusercontent.com`
- `api.github.com`
- `github.githubassets.com`

### 方案 3：配置代理设置（公司网络必需）

#### 方法 A：VSCode 设置

1. 打开 VSCode 设置（Ctrl+,）
2. 搜索 "proxy"
3. 设置以下值：

```json
{
  "http.proxy": "http://代理地址:端口",
  "http.proxyStrictSSL": false,
  "http.proxySupport": "on"
}
```

#### 方法 B：环境变量

```bash
# Windows (命令提示符)
set HTTP_PROXY=http://代理地址:端口
set HTTPS_PROXY=http://代理地址:端口

# Windows (PowerShell)
$env:HTTP_PROXY="http://代理地址:端口"
$env:HTTPS_PROXY="http://代理地址:端口"

# macOS/Linux
export HTTP_PROXY=http://代理地址:端口
export HTTPS_PROXY=http://代理地址:端口
```

#### 方法 C：代理认证

如果代理需要用户名和密码：

```json
{
  "http.proxy": "http://用户名:密码@代理地址:端口",
  "http.proxyStrictSSL": false
}
```

### 方案 4：处理 SSL 证书问题

#### 临时方案（不推荐用于生产环境）

```json
{
  "http.proxyStrictSSL": false
}
```

#### 永久方案：导入企业证书

1. 从 IT 部门获取企业 SSL 证书（.cer 或 .crt 文件）
2. 安装证书到系统：

**Windows**：
```bash
# 双击证书文件，选择"本地计算机"
# 选择"受信任的根证书颁发机构"
```

**macOS**：
```bash
# 打开"钥匙串访问"
# 拖拽证书到"系统"钥匙串
# 双击证书 > 信任 > 始终信任
```

**Linux**：
```bash
sudo cp your-cert.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
```

### 方案 5：清除缓存并重置扩展

```bash
# 1. 关闭 VSCode

# 2. 删除 Copilot 缓存
# Windows
rmdir /s "%APPDATA%\Code\User\globalStorage\github.copilot"

# macOS
rm -rf ~/Library/Application\ Support/Code/User/globalStorage/github.copilot

# Linux
rm -rf ~/.config/Code/User/globalStorage/github.copilot

# 3. 重启 VSCode
# 4. 重新登录 GitHub Copilot
```

### 方案 6：检查 GitHub Copilot 订阅和权限

1. 访问 https://github.com/settings/copilot
2. 确认订阅状态为"Active"
3. 检查组织权限设置
4. 如果是企业账户，确认管理员已启用 Copilot

### 方案 7：更新软件

```bash
# 检查 VSCode 版本
# 帮助 > 关于

# 建议版本：1.85.0 或更高

# 更新 GitHub Copilot 扩展
# 1. 打开扩展面板（Ctrl+Shift+X）
# 2. 搜索 "GitHub Copilot"
# 3. 如果有更新，点击"更新"
```

### 方案 8：使用诊断命令

在 VSCode 中：

1. 按 `F1` 或 `Ctrl+Shift+P`
2. 输入 "GitHub Copilot: Diagnostics"
3. 查看输出面板的错误信息
4. 截图并报告给支持团队

### 方案 9：检查系统时间

```bash
# 确保系统时间正确
# Windows: 设置 > 时间和语言 > 日期和时间
# macOS: 系统偏好设置 > 日期与时间
# Linux: timedatectl
```

系统时间不正确可能导致 SSL 证书验证失败。

### 方案 10：网络抓包诊断（高级）

```bash
# Windows: 使用 Wireshark 或 Fiddler
# macOS: 使用 Charles Proxy 或 tcpdump
# Linux: 使用 tcpdump

# 检查是否能连接到：
# - api.github.com:443
# - copilot-proxy.githubusercontent.com:443
```

## 预防措施

1. **稳定网络连接**：使用有线连接而非 WiFi
2. **定期更新**：保持 VSCode 和扩展最新
3. **避免频繁切换网络**：可能导致 token 失效
4. **合理使用**：避免在网络不稳定时大量使用
5. **备用方案**：配置备用网络或热点

## 常见问题 FAQ

### Q1: 为什么工作一段时间就断开？

A: 可能是：
- Token 过期（默认有效期）
- 网络连接超时
- 代理服务器断开连接
- 防火墙规则周期性重置

**解决方法**：重新登录或重启 VSCode

### Q2: 在家可以用，在公司不行？

A: 这是典型的企业网络限制问题。
**解决方法**：联系 IT 部门配置防火墙和代理

### Q3: 其他 GitHub 功能正常，只有 Copilot 不行？

A: Copilot 使用不同的 API 端点。
**解决方法**：确保 `copilot-proxy.githubusercontent.com` 可访问

### Q4: 提示"You don't have access to Copilot"？

A: 这是权限问题，不是网络问题。
**解决方法**：
1. 检查 GitHub Copilot 订阅
2. 访问 https://github.com/settings/copilot
3. 重新订阅或联系管理员

## 收集诊断信息

如果问题仍未解决，收集以下信息联系支持：

1. **VSCode 版本**：帮助 > 关于
2. **Copilot 扩展版本**：扩展面板查看
3. **操作系统**：Windows/macOS/Linux + 版本
4. **网络环境**：公司/家庭/VPN
5. **错误日志**：
   - VSCode 输出面板 > GitHub Copilot
   - 开发者工具控制台（帮助 > 切换开发人员工具）
6. **网络测试结果**：
   ```bash
   curl -v https://api.github.com
   curl -v https://copilot-proxy.githubusercontent.com
   ```

## 联系支持

- **GitHub Support**: https://support.github.com/
- **GitHub Community**: https://github.com/orgs/community/discussions
- **VSCode Issues**: https://github.com/microsoft/vscode/issues

## 参考资料

- [GitHub Copilot 官方文档](https://docs.github.com/en/copilot)
- [VSCode 网络设置](https://code.visualstudio.com/docs/setup/network)
- [GitHub Status](https://www.githubstatus.com/)
- [Copilot 故障排查](https://docs.github.com/en/copilot/troubleshooting-github-copilot)
