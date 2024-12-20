# 🛰️ Page 对象

ChromiumPage和WebPage是 4.1 之前用于连接和控制浏览器的对象。4.1 这些功能由Chromium实现，但ChromiumPage和WebPage仍能正常使用。对比Chromium，ChromiumPage和WebPage在连接浏览器时可以少写一行代码，但在多标签页操作的时候容易造成混乱。更详细的用法可以看旧版文档。

## ✅️️ ChromiumPage

ChromiumPage把浏览器管理功能和一个标签页（默认接管时激活那个）控制功能整合在一起。可看作浏览器对象，但同时控制了一个标签页。如果项目只需要使用单标签页，用ChromiumPage会比较方便。ChromiumPage创建的标签页对象为ChromiumTab，没有切换模式功能。

| 初始化参数     | 类型                | 默认值 | 说明                                       |
| -------------- | ------------------- | ------ | ------------------------------------------ |
| addr_or_opts   | str<br>int<br>ChromiumOptions | None   | 浏览器启动配置或接管信息。传入 'ip: port' 字符串、端口数字或ChromiumOptions对象时按配置启动或接管浏览器；为None时使用配置文件配置启动浏览器 |
| tab_id         | str                | None   | 要控制的标签页 id，不指定默认为激活的       |

```python
from DrissionPage import ChromiumPage

page = ChromiumPage()
page.get('https://DrissionPage.cn')
print(page.title)
```

## ✅️️ WebPage

WebPage覆盖了ChromiumPage所有功能，并且增加了切换模式功能，创建的标签页对象为MixTab。

| 初始化参数         | 类型                | 默认值 | 说明                                                                 |
| ------------------ | ------------------- | ------ | -------------------------------------------------------------------- |
| mode               | str                 | 'd'    | 运行模式，可选'd'或's'                                               |
| chromium_options   | bool<br>ChromiumOptions | None   | ChromiumOptions对象，传入None时从默认 ini 文件读取，传入False时不读取 ini 文件，使用默认配置 |
| session_or_options | SessionOptions<br>None<br>False | None   | Session对象或SessionOptions对象，传入None时从默认 ini 文件读取，传入False时不读取 ini 文件，使用默认配置 |

```python
from DrissionPage import WebPage

page = WebPage()
page.get('https://DrissionPage.cn')
print(page.title)
page.change_mode()
print(page.title)
```
```