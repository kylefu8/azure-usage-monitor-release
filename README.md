# Azure Usage Monitor

Windows x64 Public Azure 费用与预算风险监控工具。此公开仓库仅用于分发预览版和使用说明，不包含应用源码。

**v0.2.0 Preview：免安装、未签名、非正式稳定版。** 当前并非所有数据都随程序目录移动的完整便携版。此工具不是 Microsoft 官方产品。

## 下载与启动

1. 从 [Releases](https://github.com/kylefu8/azure-usage-monitor-release/releases) 下载 `AzureUsageMonitor-v0.2.0-windows-x64.zip` 和 `SHA256SUMS.txt`。
2. 用 PowerShell 检查 ZIP 的 SHA-256，与校验文件比较：

   ```powershell
   Get-FileHash .\AzureUsageMonitor-v0.2.0-windows-x64.zip -Algorithm SHA256
   ```

   校验文件用于发现下载损坏，不是数字签名或发布者身份凭证。
3. 解压到固定的本地目录，运行 `azure-usage-monitor.exe`。无需安装本工具，无需 Node.js 或 Rust。
4. 本机需要已安装 [Azure CLI for Windows](https://learn.microsoft.com/cli/azure/install-azure-cli-windows) 和 [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)。程序不会自动安装这些依赖。

未签名程序可能触发 SmartScreen 或安全软件提示。请核对下载来源和组织政策，不要关闭系统安全防护。

## 第一次使用

- 启动后打开设置，查看 Azure CLI、profile、登录状态、订阅及费用权限检查结果。
- 已有 `%USERPROFILE%\.azure` / `.azure-profiles` 或其他 CLI 配置时，可引用现有目录；不会复制凭据或修改其默认订阅及云环境。CLI 查询仍可能刷新其自身认证缓存。
- 也可创建应用管理的 profile，在设置中点击登录，由 Azure CLI 在系统浏览器中完成认证。外部引用的 profile 必须在应用外自行重新登录。
- 在“添加订阅”中搜索并选择需要监控的订阅。金额区只列已选订阅；每个 profile 可展开查看可访问订阅、是否已监控及本次成功费用读取来源。
- 为每个订阅分别设置预算、币种和独立的提醒金额，保存后生效。这些仅是本地设置，不创建 Azure Budget。
- 提醒有总开关、订阅独立开关和勿扰时间。**免安装版本的 Windows 系统通知尚未完成验收，不能作为唯一的预算预警渠道。**

## 费用与预算口径

- 仅支持 Public Azure / AzureCloud，跳过 Azure China；不自动切换现有 profile 的云环境。
- Azure API 明确返回当前账单周期时，显示该周期开始至今的 Actual Cost；明确不支持时使用 UTC 自然月。
- 权限、网络或周期信息不完整时，临时按自然月展示并标注“账单周期未确认”，不根据出账日猜周期。
- Actual Cost 有入账延迟，不是实时用量、赠金余额或抵扣后的最终应付账单；不预测月底费用。
- 多个 profile 可访问同一订阅时按订阅 ID 去重，不重复累计。不同币种分别汇总，不自动换算。
- 预算使用率 80% 起为黄色，100% 起为红色。通知使用独立的提醒金额，当前周期达到金额后最多提醒一次；修改金额不会重新触发同周期已发提醒。
- 失败订阅不纳入本次汇总；无费用记录不猜金额为零或猜测币种。
- 费用查询需 Cost Management Reader 或等效权限及允许查询的账单策略；订阅可见不代表费用可读。账单周期查询权限可能不同。

## 界面

托盘面板、独立设置页、透明置顶悬浮窗；中英文、系统深浅色、手动/定时刷新、每日与服务费用明细。退出请使用托盘退出菜单。

## 数据与便携性

- 前端已内嵌 EXE，不运行本机 HTTP 服务，不加载远程网页。
- 设置保存在 `%APPDATA%\com.kyle.azure-usage-monitor\settings.json`；应用管理的 CLI profile 在其应用数据目录下的 `cli-profiles`。程序不把设置或认证放在 EXE 旁。
- 设置包含订阅选择、profile 路径/名称、预算与提醒、窗口偏好和提醒去重键。费用及趋势只保存在应用内存，应用不写费用日志或收集遥测。
- Azure CLI 在各 profile 中持久化认证缓存；Windows、CLI、WebView2 也可能维护缓存或诊断数据。不保证所有内容完全不落盘。
- **不要将 `.azure`、`.azure-profiles`、应用 profile 目录或个人设置打包分享。** 本发行包不包含账号、认证缓存、个人设置或实际 Azure 费用。
- 移动 EXE 不会迁移设置或认证，也不保证自启路径继续有效。认证缓存不保证跨机器或跨 Windows 用户可用。
- 删除 EXE/ZIP 不会删除 AppData 设置、CLI 缓存或已启用的自启项；删除程序前请先在设置中关闭自启。

详细限制见 [KNOWN-ISSUES.md](KNOWN-ISSUES.md)，版本内容见 [RELEASE-NOTES.md](RELEASE-NOTES.md)。
