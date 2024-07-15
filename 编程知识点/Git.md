## git rebase和merge有什么区别

#### 现象
以feature和master分支为例，把master分支merge到feature，git merge会在feature分支产生一个新的commit，新的commit指向master分支。
如果是把master分支rebase到feature分支，feature分支会移动到master分支前面，和master分支会整合到一个分支上，feature分支上的commit会生成新的commit。
#### 优缺点
rebase：好处：可以产生线性的commit 记录，review时更加直观。
merge：好处：不会破坏原来的提交记录，缺点：会产生新的commit记录。

![[rebase-1.png|900]]
![[rebase-2.png|900]]
![[rebase-3.png|900]]
![[rebase-4.png|900]]
