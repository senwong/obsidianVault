>原文链接：https://developer.chrome.com/blog/inside-browser-part3/#compositing


# style
- 说明：根据dom节点和css节点，计算每个节点的style属性，即每个元素的`computed style`。
- 进程：main process

# Layout
- 说明：计算每个元素的几何属性，生成`layout tree`。
- 进程：main process


# Paint
- 说明：计算每个元素的先后渲染顺序，生成`paint records`。
- 进程：main process

# Compositing
- 说明：把整个页面分层，生成`layer tree`，`main thread` 把`layer tree`和`paint records`交给`compositor thraed`，`compositor thraed`把每个layer分成小的tile，交给`raster thread`，`raster thread`把tile光栅化之后，把数据放到GPU内存，然后GPU显示图像。
- 进程：main process . main thread --> compositer thread
