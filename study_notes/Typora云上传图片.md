# Typoar使用PicGo上传图片至GitHub仓库保存



## 1.建立GitHub仓库

打开GitHub网站，点击**new**新建仓库

![fefefefeg](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291716424.png)

输入**Repository name**、**Description**，勾选**Public**和**Add a README file**选项，然后点击右下角**create**创建

![dwhku](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291720252.png)

点击右上角头像选择**settings**

![jiewuijgeuiegwu](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291721270.png)

下滑到设置最下面选择**Developer settings**

![deve](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291721836.png)

点击**Tokens（classic）**，然后点击右边**generate new token**选择**classic**

![classicfeg](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291722404.png)

填写**Note**，选择有效期，勾选**repo**

![repofegreh](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291722257.png)

然后点击最下面**generate token**

![gerfg](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291722503.png)

复制密钥保存，因为它只会显示这一次，之后都看不到了

## 2.安装PicGo

访问[Releases · Molunerfinn/PicGo (github.com)](https://github.com/Molunerfinn/PicGo/releases)，下载安装

![xisdjwf](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291723250.png)

打开PicGo，选择**PicGo设置**，打开时间戳重命名

![c801c7c032ccca80715a5f838ecf37c](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291723859.png)

打开图床设置，选择**Github**，点击**+**

![9c1d68138290bc729d39c3d2ba0a79b](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291724615.png)

填写好信息![efewfefef](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291124164.png)

其中仓库名参考GitHub页面，直接复制完整的，注意格式；token就是之前在GitHub页面生成的密钥

![image-20230729113415248](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291134327.png)

由于GitHub的特殊环境，上传和加载速度可能较慢，这里我们使用CDN加速。

在浏览器输入`https://cdn.jsdelivr.net/gh/用户名/仓库名/`，(注意最后的`/`不要丢)，可以打开我们在 GitHub 上创建的仓库的文件列表，说明我们在 Github 上创建的仓库已经默认被 jsDelivr 缓存了。

在设置自定义域名的地方填写`https://cdn.jsdelivr.net/gh`/用户名/仓库名@master

![dd70e9dc7e4d2e2c56af95ee119bea2](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291658466.png)

## 3.Typora设置

点击**文件**中的**偏好设置**

![image-20230729170059556](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291700588.png)

勾选框中的选项，在4中选择**PicGo（app）**后选择自己电脑中PicGo.exe的安装目录，最后点击**验证图片上传选项**，如果成功就已经完成了全部配置

![image-20230729170423425](https://cdn.jsdelivr.net/gh/Siberianwolf926/Typora_images@master/img/202307291704479.png)

PS:直接复制QQ聊天记录中的图片粘贴后可能会上传失败，需要截图重新粘贴或者保存到本地后重命名后粘贴方可。问题原因好像出在QQ保存图片时命名中含有特殊字符的问题，复制微信聊天记录中的图片完全没有这个问题。