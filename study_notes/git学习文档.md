**常用Git命令：**

```c++
// 初始化一个新的Git仓库：
git init
    
// 克隆远程仓库到本地：
git clone <repository URL>
    
// 添加文件到暂存区：
git add <file> 
git add . // 添加所有更改的文件
    
// 提交暂存区的更改到本地仓库：
git commit -m "commit message"
    
// 将本地提交推送到远程仓库：
git push
    
// 拉取远程仓库的更改到本地：
git pull
    
// 查看当前状态：
git status
    
// 查看提交历史：
git log
    
//分支操作
git branch -a // 查看远程分支：    
git branch // 查看本地分支：    
git checkout -b <branchname> // 创建并切换到新分支：
git checkout <branchname> // 切换到已存在的分支：
git merge <branchname> // 合并分支：
    
//放弃未提交的本地修改（还未使用git add添加到暂存区）
git checkout -- <file>
git checkout -- . // 丢弃所有未提交的更改
    
// 丢弃暂存区的更改（已经添加到暂存区）
git reset <file>  // 将指定的文件从暂存区移除
git reset // 取消所有文件的暂存

// 撤销最近的 commit(未使用git push推送)
git reset --soft HEAD~1 // 撤销最近的一次提交，但保留文件的更改在暂存区，准备重新提交
git reset --mixed HEAD~1 // 撤销最近的一次提交，文件会回到未暂存的状态，你需要再次使用 git add
git reset --hard HEAD~1 // 彻底撤销提交，丢弃工作区和暂存区中的所有修改

// 撤销最近的 commit(已经git push推送)
git revert <commit-id> //  <commit-id> 是指你想撤销的提交的唯一标识符，历史记录保持不变，但文件的内容会恢复到撤销前的状态
```

  \- 解决冲突：在合并时可能会出现冲突，需要手动解决冲突后再提交。

#### **Git的工作原理：**

![202308031009419](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522655.png)
![202308031009950](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522831.png)

### 服务器配置git公钥到GitLab（Windows/Linux）

1.输入git 用户名和邮箱

```c++
git config --global user.name '你的用户名'
git config --global user.email '你的邮箱'
    
// 验证配置
git config --list
```

2.生成公钥

```c++
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
```

3.接下来会出现让你设置生成的ssh的保存路径以及密码，一路【回车】可以跳过。跳过的话，ssh密钥默认保存在`~/.ssh/`下

4.查看公钥，复制公钥内容

```c++
cat ~/.ssh/id_rsa.pub
```

5.进入GitLab官网点击头像，点击偏好设置（preference），进入**SSH Keys**，将公钥内容复制进去，选择到期时间后提交

![202310251426612](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522860.png)

6.Linux下（Windows下忽略此步骤）

```c++
// 回到Linux终端，将 id_rsa加入ssh-gent：
ssh-add ~/.ssh/id_rsa

// 如果报错："Could not open a connection to your authentication agent"
// 那么执行下面两行，重新加入：
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```

完成



