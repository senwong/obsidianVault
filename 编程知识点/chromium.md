
## chromium
chromium从输入地址后发生了什么？
整个浏览器有一个main process，负责处理浏览器UI，存储，网络。按下回车之后浏览器UI线程会处理这个navigation，开始后会把让负责网络请求的thread去处理请求，涉及到网络部分，域名解析，http请求链接。网络请求处理完成之后，会根据返回结果的类型，这里假如返回的是html文件。
如果返回的是html文件，则会启动render process（chromium中是blink，safari中是webkit），对html文件处理。
html文件是咋被处理的呢？生成dom树，css生成cssDom树（绘制完成后放到gpu buffer里，GPU渲染画面），通过v8执行javascript。
v8执行JavaScript是，现词法分析生成token， 语法分析生成ast，解释器通过ast生成字节码（bytecode）并执行，解释器使用的是自己实现的寄存器，不是机器码，机器码使用的是操作系统提供的指令集，相当于是一个虚拟机。
除此之外，v8 有一个负责优化的编译器，turboFan, 会把在interpreter执行过程中的热点代码编译成machine code以提高性能，


[[inside_browser_阅读笔记]]