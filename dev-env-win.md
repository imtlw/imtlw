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

# 安装Ubuntu-26.04
wsl --install -d Ubuntu-26.04

# 当Ubuntu下载安装完之后会自动提示创建默认用户
# 按照提示输入一个用户名和密码即可。
```

## WSL2配置

1.WSL性能配置
- 在`%UserProfile%\`路径中创建一个`.wslconfig`配置文件，完整路径：`%UserProfile%\.wslconfig`
- 复制下列内容到`%UserProfile%\.wslconfig`配置文件当中，具体配置可参考宿主机进行略微调整。
- 官方配置说明：[WSL中的高级设置配置](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)

```ini
[wsl2]
memory=8GB
processors=4
swap=4GB
```

2.WSL存储位置优化

```powershell
# 1.导出WSL分发版
wsl --export Ubuntu-26.04 D:\Develop\wsl\ubuntu2604.tar

# 2.注销原有的WSL分发版
wsl --unregister Ubuntu-26.04

# 3.导入WSL分发版到新位置
wsl --import Ubuntu2604 D:\Develop\wsl\ubuntu2604 D:\Develop\wsl\ubuntu2604.tar --version 2

# 4.启动定制好的WSL分发版
wsl -d Ubuntu2604
```

## 对Ubuntu系统的一些配置

1.更新`package`
 - 在**Ubuntu**的窗口中执行下列命令：
   ```bash
   sudo apt update && sudo apt full-upgrade -y
   ```
