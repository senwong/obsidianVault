我们经常在配置文件中配置路径匹配，比如：`/**/*.js`这种，文档说使用了glob paterna，今天就讲解一下什么是glob pattern。
wildcard:
`*` 星号代表零个或者多个字符串，但是路径分隔符要排除在外，Unix中的`/`和Windows中的`\`。
`?` 问号代表一个字符串，比如`?.text` 表示一个字符后面跟着`.text`.  `??.text` 表示两个字符后面跟着`.text`。
`**` 两个星号匹配零个或多个非隐藏文件夹。
`[abc]` 括号表示匹配其中一个字符。
`[a-z]` 括号加横线表示匹配从范围字符串。
