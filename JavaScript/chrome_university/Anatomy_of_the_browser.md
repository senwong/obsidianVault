## 浏览器的core principles
speed, security, simplicity, stability


chrome 1.0
![[chorme_1.0_arch.png]]

每一个方框是一个process。

一个简单的页面process组成，tab是浏览器主进程，页面是多个渲染线程，视频是插件线程。

![[simple_page_arch.png]]

渲染进程renderer process, 沙盒化
parsing, layout, executing javascript, decoding,

插件进程 plugin process.未沙盒化
 NPAPI， Flash, Java, Reader etc...

浏览器主进程main process
协调者，浏览器状态，浏览器设置，处理网络请求。

线程Threads
在子进程中，大部分有一个main thread和IPC thread。
主线程，main thread 是UI thread 。 IO thread 非阻塞式 files IPC 。

![[chrome_today_arch.png]]

红色的是sandboxed，

gpu process
web新特型比如webGL 放到Gpu Process。
大页面的composing和scrolling放到gpu process。

utility process
短期执行的short lived，浏览器新特型，不信任。安装扩展，json处理。

Pepper process
第三方不受控制的插件，比如pdf和Flash，新的插件process。

chrome content分割

src/chrome主要包含UI和浏览器特性的代码，比如书签，密码管理器，自动天窗
src/content主要包含多线程，沙盒，web平台
其他基于src/content的项目，ChromeCast， Electron。


站点隔离
为了安全性，
![[site_isolation.png]]


Mojo
新的IPC system。
IDL based。


文件目录
![[directory_layout.png]]