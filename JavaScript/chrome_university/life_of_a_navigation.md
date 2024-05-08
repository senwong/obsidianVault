## navigation timeline

输入框中输入地址后，转换为一个URL，触发Begin Navigation， 启动网络进程请求URL，请求返回之后，根据返回的文件类型，找到一个合适的renderer进程，请求body加载完成之后，渲染进程开始渲染文件内容，渲染完成之后告诉浏览器UI进程结束加载。
![[navigation_timeline.png]]


## NavigationThrottle

- willStartRequest
- willRedirectRequest
- willFailRequest
- willProcessResponse

发起请求的时候，会询问每个实现willStartRequest的throttle，如果都ok再发起请求
当请求返回重定向(300)时，会询问每个实现willRedirectRequest的throttle, 如果都ok再重定向。

![[navigation_throttle.png]]

## WebContentsObserver
用于观察navigation发生变化，接受响应事件

![[webContentsObserver.png]]

## SiteInstance
相同站点使用同一个SiteInstance
![[siteInstance.png]]

