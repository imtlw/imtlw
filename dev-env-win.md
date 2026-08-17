# Windows11 Web开发环境配置

- 在Windows11中配置Web开发环境的一些相关设置记录。

## 安装和配置WSL2

1. 以管理员身份运行**PowerShell**。
2. 在**PowerShell**窗口中依次执行下列命令来启用`WSL`和虚拟机平台功能：

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 重启计算机以应用更改。
Restart-Computer
```

## 更新WSL2内核并安装Ubuntu

1. 以管理员身份运行**PowerShell**。
2. 在**PowerShell**窗口中依次执行下列命令：

```powershell
# 更新WSL2内核
wsl --update

# 将WSL2设置为默认的版本
wsl --set-default-version 2

```
