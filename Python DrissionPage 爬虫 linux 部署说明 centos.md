

# DrissionPage 在 CentOS 系统的安装配置指南

## 环境说明
- 系统：Linux CentOS
- 浏览器：Chrome 115版本
- 官方文档：http://g1879.gitee.io/drissionpagedocs/

## 安装步骤

### 1. 安装 DrissionPage
```bash
pip3 install DrissionPage
```

### 2. 引入主程序
```python
from DrissionPage import ChromiumPage
```

### 3. 设置浏览器路径
```python
from DrissionPage.easy_set import set_paths

# Windows路径示例
set_paths(browser_path=r'C:/Users/AAA/AppData/Local/Google/Chrome/Application/chrome.exe')

# Linux路径示例
set_paths(browser_path=r'/opt/google/chrome/google-chrome')
```

### 4. 设置无头模式（Linux环境必需）
```python
from DrissionPage.easy_set import set_headless, set_paths
set_headless(True)
```

## 配置文件说明

配置文件路径（Linux）：
```
/usr/local/python3/lib/python3.7/site-packages/DrissionPage/configs
```

### ini配置示例
```ini
[paths]
chromedriver_path = /mkl/weipu/chromedriver-linux64/chromedriver
download_path =

[chrome_options]
debugger_address = 127.0.0.1:9211
binary_location = /opt/google/chrome/google-chrome
arguments = ['--no-first-run', '--no-sandbox', '--disable-infobars', '--disable-popup-blocking', '--headless=new']
extensions = []
experimental_options = {'prefs': {'profile.default_content_settings.popups': 0, 'profile.default_content_setting_values': {'notifications': 2}}}
page_load_strategy = normal
user = Default
auto_port = False
system_user_path = False

[session_options]
headers = {'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/603.3.8 (KHTML, like Gecko) Version/10.1.2 Safari/603.3.8', 'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8', 'connection': 'keep-alive', 'accept-charset': 'GB2312,utf-8;q=0.7,*;q=0.7'}

[timeouts]
implicit = 10
page_load = 30
script = 30

[proxies]
http =
https =
```

### 配置说明
1. chromedriver_path：Chrome驱动路径
2. debugger_address：调试端口（默认9222，可改为9211等）
3. binary_location：Chrome浏览器安装路径

## 代码示例

```python
import time
import re
import math
from DrissionPage import ChromiumPage
from DrissionPage.easy_set import set_paths
from DrissionPage import ChromiumOptions
from DrissionPage.easy_set import set_headless, set_paths

# 设置无头模式（Linux必需）
set_headless(True)

# 配置Chrome选项
co = ChromiumOptions()
co.set_argument('--incognito')
co.set_argument('--no-sandbox')

def start_test_spider(auth_name, institution_name, status_type):
    # 创建页面对象
    page = ChromiumPage()
    
    # 访问页面
    page.get('https://xxx.com/')
    time.sleep(1)
    
    # 点击操作
    page.ele('xpath://*[@id="basic_searchdomainfilter"]/div[1]/div[1]/div[1]/div/div/input').click()
    
    # 输入内容
    page.ele('xpath://*[@id="basic_searchdomainfilter"]/div[1]/div[1]/div[2]/input').input('输入的内容')
    
    # 关闭浏览器
    page.close_tabs()
```

> 注：本文遵循 CC 4.0 BY-SA 版权协议，原文链接：https://blog.csdn.net/sinat_39327967/article/details/132181129