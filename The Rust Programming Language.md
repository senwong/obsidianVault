
# what is ownership?
-   Each value in Rust has an _owner_.
-   There can only be one owner at a time.
-   When the owner goes out of scope, the value will be dropped.


# slice和reference的区别

slice也是引用，表示部分区间的引用，

  

Reference是整体的引用

  

slice的语法 &str &s[1..2]

  

Reference的语法 &String &s
  

引用和slice可以同时有多个读，但是不能同时有多个&mut

在有一个&mut的情况下，必须要其他引用完成才能声明&mut

## smart pointer
### Box
store data in heap not in stack. 
