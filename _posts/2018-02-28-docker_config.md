---
layout: default
title:  Docker 配置
---

<h1>{{ page.title }}</h1>

## Windows 10 配置 Docker
### 安装 Hyper-V
在 windows 10 上使用 Docker 需要使用 Hyper-V 隔离，安装 Hyper-V 有三种方式。  

#### 使用 PowerShell 启用 Hyper-V  
1. 以管理员身份打开 PowerShell 控制台。  
2. 运行以下命令：  
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName:Microsoft-Hyper-V -All
```  

#### 使用 CMD 和 DISM 启用 Hyper-V  
1. 以管理员身份打开 PowerShell 或 CMD 会话。
2. 运行以下命令：
```powershell
DISM /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V
```  

#### 手动启用 Hyper-V 角色  
1. 右键单击 Windows 按钮并选择“应用和功能”。
2. 选择“打开或关闭 Windows 功能”。
3. 选择“Hyper-V”，然后单击“确定”。

安装过程中需要使用管理身份执行，安装完成后需要重启计算机。  

### 配置 Docker

#### 安装 Docker
下载并安装 [Windows 的 Docker](https://download.docker.com/win/stable/InstallDocker.msi)，可参考[详细的安装说明](https://docs.docker.com/docker-for-windows/install/)，安装完成后需要注销重新登录。

#### 使用 Docker
打开 PowerShell 或 CMD 会话，执行 docker 命令，如 docker version，可以看到 docker 已经安装成功并能正常运行，执行 docker run hello-world 拉取第一个 docker 镜像并运行一个 docket 容器。


#### 参考
* [在 Windows 10 上安装 Hyper-V](https://docs.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
* [Windows 10 上的 Windows 容器](https://docs.microsoft.com/zh-cn/virtualization/windowscontainers/quick-start/quick-start-windows-10#1-install-docker-for-windows)

<p>{{ page.date | date_to_string }}</p>
