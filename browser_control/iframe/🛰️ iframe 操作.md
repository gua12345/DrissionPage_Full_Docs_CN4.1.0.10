# 🛰️ iframe 操作

`<iframe>`元素是一种特殊的元素，它既是元素，也是页面。  
DrissionPage 无需切入切出即可处理`<iframe>`元素。可实现跨级元素查找、元素内部单独跳转、同时操作`<iframe>`内外元素、多线程控制多个`<iframe>`等操作。

## ✅️ 获取 `<iframe>` 对象

获取`<iframe>`对象的方法有两种，可用获取普通元素的方式获取，或者用`get_frame()`方法获取。  
推荐优先使用`get_frame()`方法，因为当作普通元素获取时，IDE 无法正确识别获取到的是`<iframe>`元素。

### 📌 get_frame()

此方法用于获取页面中一个`<frame>`或`<iframe>`对象。

| 参数名称      | 类型            | 默认值 | 说明                                                                 |
|---------------|-----------------|--------|----------------------------------------------------------------------|
| loc_ind_ele   | str / int / ChromiumFrame | 必填   | 定位符 `<iframe>`元素序号（从1开始，负数表示倒数）ChromiumFrame对象 id属性内容 name属性内容 |
| timeout       | float           | None   | 超时时间（秒），为None时使用页面超时时间                              |

**返回类型**  
说明  
ChromiumFrame `<frame>`或`<iframe>`元素对象  
NoneElement 找不到时返回NoneElement  

**注意**  
需要特别注意的是，如果页面中有嵌套的`<iframe>`，用序号获取的方式会存在不准确。比如，用`get_frames()`可获取到 6 个元素，但用`get_frame(6)`却获取不到最后一个。这是因为有两个`<iframe>`是嵌套关系，导致获取不准确。

**示例：**

```python
# 使用定位符获取
iframe = tab.get_frame('t:iframe')

# 获取第1个iframe
iframe = tab.get_frame(1)
```

### 📌 get_frames()

此方法用于获取页面中多个符合条件的`<frame>`或`<iframe>`对象。

| 参数名称 | 类型              | 默认值 | 说明                      |
|----------|-------------------|--------|---------------------------|
| locator  | str / Tuple[str, str] | None   | 定位符，为None时返回所有 |
| timeout  | float             | None   | 超时时间（秒），为None时使用页面超时时间 |

**返回类型**  
说明  
List[ChromiumFrame] `<frame>`或`<iframe>`元素对象组成的列表  

**提醒**  
获取所有`<iframe>`会很慢，而且浪费资源，一般使用获取需要用到的就好。

### 📌 普通元素方式

可以用获取普通元素的方式获取`<iframe>`对象：

```python
iframe = tab('t:iframe')
```

这个ChromiumFrame对象既是页面也是元素。由于 IDE 不会提示`<iframe>` 元素对象相关的属性和方法，因此用这种方式获取时建议再用`get_frame()`包装一下：

```python
iframe = tab('t:iframe')
iframe = tab.get_frame(iframe)
```

## ✅️ 查找`<iframe>`内元素

当`<iframe>`与标签页是同域的，我们并不需要先切入`<iframe>`，就可以获取到里面的元素。  
如果是异域的，则先要获取这个标签页的ChromiumFrame对象，再用这个对象在自己内部搜索。

### 📌 页面跨`<iframe>`查找

如果`<iframe>`元素的网址和主页面是同域的，我们可以直接用页面对象查找`<iframe>`内部元素，而无需先获取ChromiumFrame对象。  
以下示例页面中有一个`<iframe>`元素，和标签页是同域的，可直接通过 Tab 对象查找它内部的元素。  
只要是同域名的，无论跨多少层`<iframe>`都能用页面对象直接获取。

```python
from DrissionPage import Chromium

tab = Chromium().latest_tab
tab.get('https://DrissionPage.cn/demos/iframe_same_domain.html')
ele = tab('概述')
print(ele)
```

**输出：**

```
<ChromiumElement h2 class='anchor anchorWithStickyNavbar_LWe7' id='️-概述'>
```

### 📌 在`<iframe>`内查找

如果`<iframe>`跟当前标签页是不同域名的，不能使用页面对象直接查找其中元素，只能先获取其ChromiumFrame元素对象，再在这个对象中查找。  
即使是同域的，也可以通过这种方法查找。  
但创建ChromiumFrame对象会增加系统资源的使用，一般建议异域的才创建对象。  
以下示例页面中有一个`<iframe>`元素，和标签页是不同域的，需要先获取ChromiumFrame对象，再在里面找元素。

```python
from DrissionPage import Chromium

tab = Chromium().latest_tab
tab.get('https://DrissionPage.cn/demos/iframe_diff_domain.html')
iframe = tab.get_frame('t:iframe')
ele = iframe('网易首页')
print(ele)
```

**输出：**

```
<ChromiumElement a class='ntes-nav-index-title ntes-nav-entry-wide c-fl' href='https://www.163.com/' title='网易首页'>
```

## ✅️ 方法和属性

正如上面所说，ChromiumFrame既是元素也是页面，它可以获取自身元素方面的属性或执行操作。  
详见相关章节。

```python
iframe.tag
iframe.html
iframe.remove_attr()
iframe.states.is_alive
iframe.get()
iframe.get_screenshot()
# 等等
```
```