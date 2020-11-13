# Windows系统使用笔记

## 禁用锁屏（win + L）快捷键
更改注册表：计算机\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\System
新建 DWORD（32位）值 > 名：DisableLockWorkstation > 值：1
