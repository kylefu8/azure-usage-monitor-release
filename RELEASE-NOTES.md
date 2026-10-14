# v0.2.0 Preview

首次公开预览发行。Windows x64，免安装 EXE/ZIP，未签名，不提供安装器。

## 功能

- 基于官方 Azure CLI 读取 Public Azure 的 Actual Cost；确认账单周期或按 UTC 自然月展示，未确认周期明确提示。
- 每订阅手动预算、币种和独立提醒金额；提醒总开关、订阅开关、勿扰和周期去重。
- 多 Azure CLI profile：应用管理的独立目录及现有目录引用；不全局切换 profile，不改变外部默认订阅或云环境。
- 启动环境检查：CLI、profile 路径/云、登录到期元数据、订阅访问和已选订阅费用权限。
- 金额区只列已选订阅；添加选择器搜索/分页；profile 可访问订阅列表及本次费用读取来源；共享订阅去重。
- 托盘、透明悬浮窗、独立设置、中英文、系统主题、每日及服务费用明细。

## 发行内容

- `AzureUsageMonitor-v0.2.0-windows-x64.zip`：`azure-usage-monitor.exe` 及三份使用说明。
- `SHA256SUMS.txt`：ZIP 的 SHA-256 校验值。
- 不含源码、安装包、个人设置、Azure 登录缓存或实际费用数据。

本机仍需 Azure CLI 和 WebView2。设置位于 AppData，并非完整目录便携。Windows 通知、自启、登录端到端、多显示器/DPI及移动目录行为尚待验收，详见 [KNOWN-ISSUES.md](KNOWN-ISSUES.md)。
