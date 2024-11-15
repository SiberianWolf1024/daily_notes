**常用Git命令：**

```c++
初始化一个新的Git仓库：git init
克隆远程仓库到本地：git clone <repository URL>
添加文件到暂存区：git add <file>
提交暂存区的更改到本地仓库：git commit -m "commit message"
将本地提交推送到远程仓库：git push
拉取远程仓库的更改到本地：git pull
查看当前状态：git status
查看提交历史：git log
查看远程分支：git branch -a
查看本地分支：git branch
创建并切换到新分支：`git checkout -b <branchname>`
切换到已存在的分支：`git checkout <branchname>`
合并分支：`git merge <branchname>`
    
远程仓库分支回退到上一次提交：git push --force origin <branch_name>
放弃所有本地修改：git reset --hard HEAD
放弃某个文件的本地修改：git checkout -- <filename>
```

  \- 解决冲突：在合并时可能会出现冲突，需要手动解决冲突后再提交。

#### **Git的工作原理：**

![202308031009419](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522655.png)
![202308031009950](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522831.png)

### Linux下服务器配置git公钥到GitLab

1.输入git 用户名和邮箱

```c++
git config --global user.name '你的用户名'
git config --global user.email '你的邮箱'
```

2.生成公钥

```c++
ssh-keygen -C '你的邮箱' -t rsa
```

3.接下来会出现让你设置生成的ssh的保存路径以及密码，一路【回车】可以跳过。跳过的话，ssh密钥默认保存在`~/.ssh/`下（也就是用户的Home下面）

4.查看公钥

在保存位置/home/.. 使用命令

```c++
cd .ssh/
ls
```

可以看到一个私钥 **id_rsa** ，一个公钥 **id_rsa.pub**

![202310251418483](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522193.png)

4.进入id_rsa.pub复制公钥内容

```c++
vim id_rsa.pub
```

5.进入GitLab官网点击头像，点击偏好设置（preference），进入**SSH Keys**，将公钥内容复制进去，选择到期时间后提交

![202310251426612](https://cdn.jsdelivr.net/gh/SiberianWolf1024/typora_images@master/img/202411061522860.png)

6.回到Linux终端，将 id_rsa加入ssh-gent：

```c++
ssh-add ~/.ssh/id_rsa
```

如果报错："Could not open a connection to your authentication agent"
那么执行下面两行，重新加入：

```c++
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```

完成
