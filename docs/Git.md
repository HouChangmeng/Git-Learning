# Github 工作流

Reference:  
https://www.bilibili.com/video/BV19e4y1q7JJ

___

## 基础流程

1. git pull origin main 拉取代码到本地 (or git clone,或克隆到本地)  
2. git checkout -b newfeature 切换至新分支newfeature  
3. 修改或者添加本地代码（部署在硬盘的源文件上）  
4. git diff 查看自己对代码做出的改变  
5. git add 上传更新后的代码至暂存区  
6. git commit 可以将暂存区里更新后的代码更新到本地git  
7. git push origin newfeature 将本地的newfeature分支上传至github上的newfeature分支  
8. 项目主人采用pull request 中的 squash and merge 合并所有不同的commit  

___

## 如果在写自己的代码过程中发现远端GitHub上代码出现改变

1. git add + git commit 保存本分支的代码  
2. git checkout main 切换回main分支  
3. git pull origin master(main) 将远端修改过的代码再更新到本地  
4. git checkout newfeature 回到xxx分支  
5. git rebase main 我在newfeature分支上，先把main移过来，然后根据我的commit来修改成新的内容  
（中途可能会出现，rebase conflict --> 手动选择保留哪段代码）  
6. git push -f origin newfeature 把rebase后并且更新过的代码再push到远端github上(-f --> 强行)  
7. 项目主人采用pull request 中的 squash and merge 合并所有不同的commit  

___

## 远端完成更新后

1.git branch -D xxx 删除本地的git分支  
2.git pull origin main 再把远端的最新代码拉至本地  

