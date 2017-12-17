首先在github上创建一个账户
由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在C盘用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件。
如果已经有了，可直接跳到下一步。
如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
$ ssh-keygen -t rsa -C "youremail@example.com"
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，
由于这个Key也不是用于军事目的，所以也无需设置密码。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，
这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
在命令行里输入 cat ~/.ssh/id_rsa.pub 可以查看公钥

第2步：登陆GitHub，打开账户设置“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，目的是区分在哪台终端上使用git，
在Key文本框里粘贴id_rsa.pub文件的内容：
为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的。
而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，
只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。
第3步 远程提交到GitHub上
git remote add origin git@github.com:chenkeng/项目名字.git
git push -u origin master
从现在起，只要本地作了提交，就可以通过命令：
$ git push origin master
把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！
SSH警告
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，
需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
这个警告只会出现一次，后面的操作就不会有任何警告了。
如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。

小结
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，
而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！


建立分支
git checkout -b dev    建立一个 叫 dev 的分支  并且将master指向dev分支
相当于 以下两个命令
git branch dev  建立分支
git checkout dev  切换到dev 分支

可以使用 git branch 查看当前有哪些分支，
* 号代表着当前是哪个分支
git branch -a 查看本地和远程的所有分支
git branch -d dev 删除本地dev 分支，
git push origin --delete dev 删除github 上的远程分支 此时可以再用 git branch -a 查看所有的分支
当 在 dev 分支上完成工作，并push 到远程仓库之。此时远程仓库可看到和master分支不一样的地方

此时可以使用  git checkout master 切换回主分支
然后 git merge dev   将dev 分支 合并到master 分支上去。 可以看到提示关键词  fast-forward
直接将master 指向dev 当前分支，相当于快进
这个时候 master 和 dev 分支的代码都一样 ， 也就是确认没问题之后，可以删除掉dev分支

删除dev 分支    命令为     git branch -d dev
删除后，此时用 git branch 查看 分支情况，可以发现只有 master 分支
切换分支速度非常快，安全又保险。

/***************总结 *******************/
查看分支        git branch
创建分支        git branch  分支名
切换分支        git checkout  分支名
///////创建和切换分支合并///////////
git checkout -b  分支名
合并某分支到当前分支   git merge  某分支
删除本地分支  git branch -d 分支名
删除远程分支  git push origin --delete 分支名
查看本地和远程分支   git branch -a
/***************总结 *******************/

遇到冲突 需要手动解决
<<<<<<< HEAD
主分支的内容
=======
新分支的内容
>>>>>>> feature
可以手动去掉一个，然后重新push 到主分支
此命令可以查看分支合并图
git log --graph --pretty=oneline --abbrev-commit

/******************************/
*重命名远程分支（思路）
*1. 本地重命名某个分支
*2. 删除远程待修改的分支名
*3. 将本地新分支push 到远程仓库
/******************************/

重命名本地分支
git branch -m 将要重命名的分支  重命名为某某分支名

删除远程分支
git push --delete origin 欲删除的分支名
可能遇到的问题是  目前指向的就是要删除的分支，此时如果报错，
只需要将目前分支切换到主分支状态下，然后再执行删除远程分支
