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

2.安装[oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)
 - 首先安装`zsh`，在**Ubuntu**的窗口中执行下列命令：
   ```bash
   sudo apt install zsh

   # 设置zsh为默认Shell
   chsh -s $(which zsh)

   # 安装oh-my-zsh
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

   # 安装zsh-syntax-highlighting插件
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```
   
3.修改zsh插件配置
 - 编辑`~/.zshrc`文件，将`plugins`的参数更改为`plugins=(git encode64 zsh-syntax-highlighting)`

4.让配置生效，在终端中输入`source ~/.zshrc`即可。

## 为Ubuntu安装nodejs

- 推荐使用[nvm](https://github.com/nvm-sh/nvm)来安装nodejs
  1.安装`nvm`
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash

  # nvm已经自动设置了.zshrc文件, 执行下列命令让.zshrc生效
  source ~/.zshrc
  ```

  2.使用`nvm`来安装`nodejs`
  ```bash
  # 安装LTS版本的nodejs
  nvm install --lts
  ```

  3.nodejs的配置
  ```bash
  # 修改NPM为国内镜像
  npm config set registry https://registry.npmmirror.com/
  
  # 使用pnpm
  corepack enable pnpm
  ```

## 安装MySQL
 
 1.在`Ubuntu`命令窗口中执行以下命令安装最新的MySQL：
   ```bash
   sudo apt install -y mysql-server
   ```

 2.验证`MySQL`是否安装成功，查看`MySQL`服务状态，显示`active (running)`即为正常。
  ```bash
  sudo systemctl mysql

  # 查看MySQL的版本
  mysql --version
  ```

 3.修改`root`用户密码
 - MySQL8.0默认无密码，需手动配置，在`Ubuntu`终端中输入以下命令：
   ```bash
   # 默认密码为空，直接回车即可。
   sudo mysql -u root -p

   # 修改root用户的密码，将your_password替换成你的密码。
   ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_password';

   # 刷新权限，使配置生效。
   FLUSH PRIVILEGES;

   # 退出MySQL
   exit;
   ```

 4.MySQL的安全配置（执行MySQL官方安全脚本，优化默认配置，禁用匿名用户、禁止root远程登录、删除测试数据库等）
 ```bash
 sudo mysql_secure_installation

 # 1.输入ubuntu的root密码
 # 2.输入mysql的root密码
 # 3.是否启用密码强度检测（推荐选择Y，按需求选择强度）
 # 4.是否修改root密码（已设置，选择N即可）
 # 5.是否删除匿名用户（选择Y）
 # 6.是否禁止root账户远程登录（选择Y，后续需要远程访问可以修改）
 # 7.是否删除test数据库（选择Y）
 # 8.是否刷新权限（选择Y）
 ```

 5.MySQL常用命令：
 ```bash
 # 启动MySQL
 sudo systemctl start mysql

 # 停止MySQL
 sudo systemctl stop mysql

 # 重启MySQL
 sudo systemctl restart mysql

 # 设置开机自启（默认已开启）
 sudo systemctl enable mysql
 ```

## 安装GoLang
  1.打开`Ubuntu`的终端窗口，输入下列命令：
   ```bash
   # 下载Go的最新版本
   wget https://go.dev/dl/go1.26.6.linux-amd64.tar.gz

   # 解压Go到指定路径
   sudo tar -xzf go1.26.6.linux-amd64.tar.gz -C /usr/local/
   ```

  2.配置`Go`的相关环境路径
   - 编辑下列环境变量到`~/.zshrc`文件中
   ```bash
   export GOROOT=/usr/local/go
   export GOPATH=$HOME/go
   export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
   ```
