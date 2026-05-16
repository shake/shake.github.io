---
layout:     post
title:      WSL2 安装 Claude Code，OpenClaw，Hermes Agent 
subtitle:   WSL2 install agent
date:       2026-05-16
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

发现使用笔记软件，也是会导致版本太多，导致经常自己查看都很不方便。记忆感觉确实因为AI，在逐步变差，唯一的办法，就是停下来，整理文章。

文章内容笔记里，公众号，不同的内容混杂一次。我需要整理一个适合我使用的版本。

## WSL 2

对于 Windows 资深用户来说，无论是安装 OpenClaw 还是 Claude Code，WSL2 (Windows Subsystem for Linux) 都是官方推荐且唯一的“最优解”。

微软其实应该2024年，才算彻底解决WSL2的各种技术问题。用最新版本，可以避免很多麻烦。

### 确认WSl 2正常

遇到过安装所谓的精简win11，导致WSL2给精简的案例。确保win11，是2024后到版本

```
## Powershell admin 运行

# 检查版本信息
wsl --version

# 强制更新至最新版本
wsl --update

```

### 开启“镜像网络”模式 (Critical)

这是 Windows 用户避坑的最关键一步。默认的 WSL 网络是 NAT 模式，在处理代理、本地浏览器通讯（OpenClaw 核心需求）时极其痛苦。镜像模式 (Mirrored) 能让 Linux 直接共享 Windows 的 IP 和网络配置。

1. 打开资源管理器，进入当前用户目录：C:\Users\<你的用户名>\。  
2. 新建文件：.wslconfig (注意文件名前有小数点)。  
3. 写入以下配置：

```
[wsl2]
# 开启镜像网络模式
networkingMode=mirrored

```

### WSL 常用命令

WSL启动的虚拟机，有点类似容器，你是无法在虚拟机关机，必须在Powershell里才能关机，习惯就好，差异不多。

```
## 关机
wsl --terminate Ubuntu-24.04


# 注销原系统：
wsl --unregister Ubuntu-24.04

# 导入到新位置：
wsl --import Ubuntu-24.04 D:\WSL\Ubuntu-24.04 D:\backups\ubuntu-24.04-snapshot-claude-20260410.tar

```

### 安装Ubuntu 24.04

```
# 查看可用分发版本
wsl --list --online
# 安装 Ubuntu 24.04
wsl --install -d Ubuntu-24.04

# 安装过程中会提示输入 Unix 用户名 和 密码（建议记住，后续 sudo 命令需要使用）。我的用户名：shake

```
装完系统后，关闭powershell。因为这个时候，你运行 wsl命令，是不管用的。


### 启动Ubutnu

windows 搜索 Ubuntu 24.04，看到图标，点击就可以打开一个Ubuntu 24.04.

sudo vi /etc/wsl.conf

```
[boot]
systemd=true
[user]
default=shake
```

配置 default 用户，可防止后续导出恢复后默认以 root 登录。

### sudo无密码

```
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USER
```

### 更新

```
sudo apt update -y
sudo apt upgrade -y
sudo apt install unzip jq -y

```


### WSL存储移动到D盘

重新打开Power shell

```
# 关闭并注销（在 PowerShell 运行）：
wsl --terminate Ubuntu-24.04

# 导出快照（建议在 D 盘创建 backups 文件夹）
wsl --export Ubuntu-24.04 D:\backups\ubuntu-24.04-init.tar

# 注销原系统：
wsl --unregister Ubuntu-24.04

# 导入到新位置：（建议在 D 盘创建 WSL 文件夹）
# 格式：wsl --import <分发名> <安装位置> <备份文件路径>
wsl --import Ubuntu-24.04 D:\WSL\Ubuntu-24.04 D:\backups\ubuntu-24.04-init.tar

```

windows，搜索Ubuntu 24.04，就可以再次启动Ubuntu 24.04.

### 虚拟机持续运行

默认情况下，关闭终端窗口会导致 WSL 挂起，且 Windows 重启后服务无法自启。我们要实现“开机即后台运行”。

Ubuntu 虚拟机里，创建一个脚本：wsl-autostart.sh

```
sudo tee ~/wsl-autostart.sh > /dev/null << 'EOF'
#!/bin/bash

# ==========================================
# 脚本名称: wsl-autostart.sh
# 适用场景: 通用 WSL 2 持久化后台运行
# ==========================================

# 1. 自动获取当前用户和家目录
USER_NAME=$(whoami)
USER_HOME=$(eval echo ~$USER_NAME)
LOG_FILE="$USER_HOME/wsl-session.log"

# 2. 记录启动时间（用于排查开机自启是否触发）
echo "[$(date '+%Y-%m-%d %H:%M:%S')] WSL Session Started for $USER_NAME" >> "$LOG_FILE"


# 4. 【核心指令】防止 WSL 闲置关闭
# tail -f /dev/null 是最轻量的占座方式，不消耗 CPU 但能维持进程活跃
echo "Keep-alive process (tail) started." >> "$LOG_FILE"
tail -f /dev/null
EOF

# 赋予可执行权限
chmod +x ~/wsl-autostart.sh

```

授权

```
chmod +x ~/wsl-autostart.sh
```

设置 Windows 任务计划程序

按下 Win + R，输入 taskschd.msc。  

```
#创建任务（非基本任务）：  
  ○ 常规：名称设为 wsl-autostart；勾选“不管用户是否登录都要运行”；勾选“使用最高权限运行”。  
  ○ 触发器：新建 -> 选择“启动时”；高级设置勾选“延迟任务运行 1 分钟”。  
  ○ 操作：新建 -> 程序：wsl；参数：-d Ubuntu-24.04 -u <你的用户名> -e /home/<你的用户名>/wsl-autostart.sh。  
  ○ 设置：取消勾选“只有在计算机使用交流电源时才启动此任务”；取消勾选“如果任务运行时间超过以下时间则停止”。
#  确定并输入 Windows 登录密码。

```

## 初始化Ubuntu

因为是给Claude code，OpenClaw，Hermes agent使用，所以为了方便，先安装基本必须工具。

### nodejs

OpenClaw需要用到，官方推荐24的版本，最低版本要求是，skill很多都需要。

```
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install nodejs -y

## 验证
node --version
npm --version

```

### bun

```
# 安装bun
curl -fsSL https://bun.sh/install | bash

source .bashrc
bun --version
```

### uv

```
curl -LsSf https://astral.sh/uv/install.sh | sh

 source $HOME/.local/bin/env
 uv --version

```

### brew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

设置环境变量

```

echo >> /home/shake/.bashrc
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"' >> /home/shake/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
```

安装环境需要包

```
sudo apt-get install build-essential -y
```

## Claude Code

用curl，一行命令就完成安装

```
curl -fsSL https://claude.ai/install.sh | bash
```

查看目录

```
$ ls ~/.claude
backups  cache  downloads

```



### 设置模型

每个模型有差异，阅读官方文档：[Minimax官方文档](https://platform.minimaxi.com/docs/token-plan/claude-code)

配置文件路径： ~/.claude/settings.json

```
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.minimaxi.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "MINIMAX_API_KEY",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "ANTHROPIC_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_SMALL_FAST_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "MiniMax-M2.7"
  }
}
```

### 快捷键

修改 .bashrc ,追加下面内容

```

# ============================================================
# 开发快捷别名
# ============================================================

# --- Claude ---
alias cc='CLAUDE_CODE_NO_FLICKER=1 claude'  # Claude Code（防闪烁）

# --- Git ---
alias gs='git status'      # 状态
alias ga='git add'         # 添加
alias gaa='git add --all'  # 添加全部
alias gc='git commit'      # 提交
alias gcm='git commit -m'  # 提交带消息
alias gp='git push'        # 推送
alias gl='git pull'        # 拉取
alias gd='git diff'        # 差异
alias gb='git branch'      # 分支
alias gco='git checkout'   # 切换
alias glog='git log --oneline --graph -10'  # 日志

# --- npm/pnpm ---
alias ni='npm install'     # 安装依赖
alias nrd='npm run dev'    # 开发
alias nrb='npm run build'  # 构建
alias nrt='npm run test'   # 测试
alias pi='pnpm install'    # pnpm 安装
alias prd='pnpm run dev'   # pnpm 开发

# --- Python/uv ---
alias uvr='uv run'         # uv 运行
alias uvs='uv sync'        # uv 同步依赖
alias py='python'          # Python

# --- 目录导航 ---
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -la'
alias la='ls -A'

# --- 实用工具 ---
alias cls='clear'          # 清屏
alias h='history'          # 历史
alias ports='lsof -i -P -n | grep LISTEN'  # 查看端口
```

### Claude 文件夹配置


