---
title: 超详细教程：如何在 Linux 上安装和配置 Sublime Text
published: 2026-08-14
tags: [教程, Linux, 数码]
category: 教程
draft: false
---

Sublime Text 是一款风靡全球的源代码编辑器，凭借其极快的启动速度、流畅的交互体验以及强大的插件扩展能力，它早已成为无数开发者手中的"神兵利器"。无论你是刚入门编程的萌新，还是刚刚从 Windows 或 macOS 切换到 Linux 系统的资深码农，Sublime Text 都能为你提供一致且高效的编码环境。它支持几乎所有主流的编程语言（如 Python、JavaScript、C++、Go 等）和标记语言（如 HTML、CSS、Markdown），并提供了命令面板、多行编辑、自动补全、代码片段等实用功能。

更重要的是，Sublime Text 完美覆盖了 Linux、macOS 和 Windows 三大平台。本文将为你奉上一份保姆级的指南，涵盖在各类 Linux 发行版上安装 Sublime Text 的多种方法，以及安装后必做的初始化配置，帮助你快速上手。

## 一、使用系统包管理器安装（推荐）

绝大多数 Linux 发行版都内置了强大的包管理器，通过包管理器安装软件不仅能自动处理依赖关系，还便于后续的更新与卸载。Sublime Text 官方为以下主流发行版提供了专属的软件源，操作非常简便。

### 1. Ubuntu / Debian 及其衍生版（如 Linux Mint）

对于 Ubuntu 和 Debian 系用户，我们使用 APT 包管理器。首先需要添加官方的 GPG 密钥，以确保下载的软件包是安全且未被篡改的。打开终端（快捷键 `Ctrl+Alt+T`），依次执行以下命令：

```bash
# 1. 下载并添加 GPG 密钥
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

# 2. 将 Sublime Text 官方存储库添加到系统源列表
echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

# 3. 更新本地软件包缓存并安装
sudo apt update
sudo apt install sublime-text
```

安装过程大约需要一两分钟。完成后，你可以在应用程序菜单中搜索 "Sublime Text" 并启动它。如果未来想要卸载该软件，只需执行：

```bash
sudo apt remove sublime-text
```

### 2. Arch Linux 及其衍生版（如 Manjaro）

Arch Linux 以其滚动更新和简洁性著称，我们使用 pacman 来管理软件包。首先同样需要导入 GPG 密钥，然后添加仓库信息：

```bash
# 1. 下载并导入 GPG 密钥
curl -O https://download.sublimetext.com/sublimehq-pub.gpg
sudo pacman-key --add sublimehq-pub.gpg
sudo pacman-key --lsign-key 8A8F901A
rm sublimehq-pub.gpg

# 2. 将仓库地址追加到 pacman.conf 配置文件（注意架构，此处以 aarch64 为例，x86_64 请替换）
echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/stable/aarch64" | sudo tee -a /etc/pacman.conf

# 3. 同步仓库并安装
sudo pacman -Syu
sudo pacman -S sublime-text
```

卸载命令为：

```bash
sudo pacman -R sublime-text
```

### 3. CentOS / RHEL 系列

CentOS 用户习惯使用 YUM 包管理器。操作流程与 Ubuntu 类似，先导入密钥，再添加仓库：

```bash
# 1. 导入 RPM GPG 密钥
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

# 2. 添加官方 YUM 仓库
sudo yum-config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

# 3. 更新并安装
sudo yum update
sudo yum install sublime-text
```

若需删除，执行 `sudo yum remove sublime-text`。

### 4. Fedora

Fedora 采用了新一代的 DNF 包管理器，本质上是 YUM 的升级版，命令几乎一致：

```bash
# 1. 导入密钥
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

# 2. 添加 DNF 仓库
sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

# 3. 更新并安装
sudo dnf update
sudo dnf install sublime-text
```

卸载命令：`sudo dnf remove sublime-text`。

### 5. openSUSE

openSUSE 使用 Zypper 作为包管理工具，操作方法同样非常直观：

```bash
# 1. 导入密钥
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

# 2. 添加仓库（-g -f 参数表示自动信任并刷新）
sudo zypper addrepo -g -f https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

# 3. 更新并安装
sudo zypper update
sudo zypper install sublime-text
```

卸载则使用 `sudo zypper rm sublime-text`。

## 二、通用安装方案：使用 Snap

如果你的 Linux 发行版不在上述列表之中（例如某些小众发行版），或者你在使用包管理器安装时遇到了依赖冲突等棘手问题，那么 Snap 将是你的最佳备选方案。Snap 是一种容器化的软件包格式，几乎能在所有现代 Linux 上运行，且自带了所有所需的依赖库。

首先，你需要确认系统中是否已安装 Snap。在终端中输入：

```bash
snap
```

如果输出了一串命令帮助信息，说明已安装；如果提示 "command not found"，则需先参照官方文档安装 Snapd 服务。安装好 Snap 后，只需一行命令即可搞定 Sublime Text：

```bash
sudo snap install sublime-text --classic
```

这里的 `--classic` 参数赋予了 Sublime Text 访问系统文件的权限，这是作为代码编辑器所必需的。卸载同样简单：

```bash
sudo snap remove sublime-text
```

值得注意的是，Snap 版本通常会稍晚于官方 APT/YUM 仓库版本，但稳定性极佳，非常适合不想折腾环境的新手。

## 三、安装后的初始化配置与优化

软件装好了，但还只是个"毛坯房"。为了让编码体验更上一层楼，我们还需要进行一些必要的"精装修"。首次启动 Sublime Text 时，你会发现界面非常简洁，甚至有些功能默认是关闭的。别担心，跟着我一步步来。

### 1. 启用小地图视口（Sublime Text 4 专属）

如果你是 Sublime Text 的老用户，一定对右侧那个方便预览全局代码的"小地图"印象深刻。但在最新的 Sublime Text 4 中，小地图视口（即高亮显示当前可视区域的方框）默认被隐藏了。

恢复方法很简单：点击顶部菜单栏的 Preferences（偏好设置） > Settings（设置）。这会打开一个名为 `Preferences.sublime-settings` 的 JSON 配置文件。在花括号 `{}` 内部，另起一行添加以下配置项：

```json
"always_show_minimap_viewport": true,
```

保存文件（快捷键 `Ctrl+S`）。回到编辑界面，你会发现小地图右侧那个半透明的白色方框又回来了，浏览长文件时再也不会迷失方向。

### 2. 统一默认字体大小

很多新手会发现，Sublime Text 默认字体太小，看着费眼。每次手动调整又太麻烦。我们可以直接在配置文件中固定字体大小。在上面的同一个设置文件中加入：

```json
"font_size": 14,
```

数字 14 可以按需替换为 16 或 18，根据自己的屏幕和视力习惯来即可。从此打开任何文件，字体大小都会保持一致。

### 3. 设置缩进规则：Tab 转空格

在 Python、YAML 等对缩进严格的语言中，混用 Tab 和空格简直是灾难。Sublime Text 允许我们强制将 Tab 键转换为空格。在配置文件中添加：

```json
"tab_size": 2,
"translate_tabs_to_spaces": true,
```

这里我将 Tab 宽度设为 2（可根据团队规范改为 4），并开启自动转换。这样按下 Tab 键时，实际插入的就是空格，彻底避免缩进报错。

### 4. 安装 Package Control（插件管理器）

如果说 Sublime Text 是一把利剑，那么 Package Control 就是剑上的锋芒。它是 Sublime Text 的灵魂所在，没有它，你就无法便捷地安装各种第三方插件。

安装步骤极其简单：点击菜单栏 Tools（工具） > Install Package Control...（安装 Package Control）。点击后，界面底部会显示正在安装，通常只需几秒钟，无需任何额外操作即可完成。

安装成功后，按下快捷键 `Ctrl+Shift+P` 调出命令面板，输入 "Package Control"，你会看到一大堆相关命令，这代表安装已成功。

### 5. 实战演练：安装 Git 插件

作为示例，我们利用 Package Control 安装 Git 插件，方便在编辑器内直接管理版本控制。

1. 按下 `Ctrl+Shift+P` 打开命令面板。
2. 输入 `Package Control: Install Package` 并回车（选中该项）。
3. 在弹出的搜索框中输入 `Git`，在结果列表中找到 "Git" 并点击安装。
4. 稍等片刻，安装完成后，你会在顶部菜单的 Tools 下看到 Git 的相关选项。

请注意，这只是一个 Git 交互增强插件，你需要在系统层面自行安装原生的 Git 命令行工具（`sudo apt install git` 或 `sudo dnf install git`）才能配合使用。

## 四、结语

至此，你已经成功在 Linux 上安装了 Sublime Text，并完成了小地图修复、字体调优、缩进规整以及插件管理器的配置。现在的 Sublime Text 已经具备了一定的生产力基础。随着你使用时间的增长，你还会发现更多好用的插件（如 Emmet、Prettier、Dockerfile 语法高亮等），而这一切都可以通过 Package Control 轻松获取。

Sublime Text 虽然轻量，但功能绝不轻量。希望这篇教程能够帮助你在 Linux 平台下搭建起得心应手的开发环境，让写代码这件事变得更加优雅和高效。如果在安装过程中遇到任何问题，不妨检查一下网络连接或确认 GPG 密钥是否正确导入，通常都能顺利解决。祝你编码愉快！
