# DrissionPage 在 Linux 系统上的安装与配置指南

## 简介
DrissionPage 是一个跨平台的网页自动化工具，对 Windows、Mac、Linux 都有良好的兼容性。特别是在 Debian 和 Ubuntu 系统上，配置相对简单。

## 前置要求
由于 Debian 和 Ubuntu 默认只预装 Firefox，需要额外安装 Chromium 内核的浏览器（推荐 Google Chrome 或 Microsoft Edge）。

> 注意：`chrome` 是 DrissionPage 配置文件中的默认启动路径。如果在终端输入该路径能正常打开浏览器，DrissionPage 就可以正常使用。

## 浏览器安装

### Chrome 安装
1. 下载 deb 安装包：
```bash
wget "https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb" -O chrome.deb
```

2. 安装：
```bash
sudo dpkg -i chrome.deb
```

### Edge 安装
- 下载页面：https://www.microsoft.com/zh-cn/edge/business/download?form=MA13FJ
- 直接下载链接：https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-stable/microsoft-edge-stable_120.0.2210.91-1_amd64.deb

## DrissionPage 安装

### 1. 创建并激活 conda 环境
```bash
conda create -n dp4
conda activate dp4
```

### 2. 安装 Python
```bash
conda install python=3.8
```

### 3. 安装 DrissionPage
```bash
pip install DrissionPage
```

### 4. 验证安装
```bash
pip show DrissionPage
```

## 使用配置

### 图形界面环境使用

1. 基本使用方式：
```python
from DrissionPage import ChromiumPage
page = ChromiumPage()
page.get('http://g1879.gitee.io/DrissionPageDocs')
```

2. 查找浏览器可执行文件路径的方法：
- 方法一：使用默认路径（Chrome：`chrome`，Edge：`microsoft-edge`）
- 方法二：在浏览器地址栏输入 `chrome://version` 查看
- 方法三：使用 `which` 命令查询

3. 配置浏览器路径：
```python
from DrissionPage import ChromiumPage, ChromiumOptions
path = r'microsoft-edge'  # 替换为实际路径
co = ChromiumOptions().set_browser_path(path)
page = ChromiumPage(co)
page.get('http://g1879.gitee.io/DrissionPageDocs')
print(page.title)
```

4. 保存配置：
```python
from DrissionPage import ChromiumOptions
path = r'microsoft-edge'  # 替换为实际路径
ChromiumOptions().set_browser_path(path).save()
```

### 无图形终端使用

配置无头模式：
```python
from DrissionPage import ChromiumPage, ChromiumOptions
co = ChromiumOptions()
co = co.set_argument('--no-sandbox')  # 关闭沙箱模式
co = co.set_headless(True)  # 开启无头模式
page = ChromiumPage(co)
page.get('http://g1879.gitee.io/DrissionPageDocs')
print(page.title)
```

注意事项：
- `set_argument('--no-sandbox')` 只需执行一次
- `set_headless(True)` 每次启动都需要
- 如遇到连接问题，使用 `page.quit()` 关闭之前的实例
- 开发阶段建议使用有头模式，便于调试