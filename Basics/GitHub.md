本教程将介绍GitHub项目的创建以及Git Bash的使用（Windows环境）。在本文中链接了许多GitHub官方的帮助文档，文档描述了很多细节，仔细阅读很有帮助。
# 预备工作
- [安装Git](https://git-scm.com/downloads)

> 说明：Git Bash（基于MINGW） 是Windows下进行Git操作的Shell。

# 创建SSH Key
使用Git Bash进行命令行操作，首先要拥有一份ssh key进行身份验证。详细信息参见：[Connecting to GitHub with SSH](https://help.github.com/en/articles/connecting-to-github-with-ssh)。

1. 验证是否存在ssh keys

> ls -al ~/.ssh

默认情况下生成的ssh key放置于“C:\Users\UserName”下的.ssh文件夹中，该命令列出该文件夹下包含的文件。常见的ssh密钥文件可能是以下文件之一：
- id_dsa.pub
- id_ecdsa.pub
- id_ed25519.pub
- id_rsa.pub

2. 创建新的ssh key
如果不存在ssh密钥，则新建一个：

> ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

your_email@example.com替换成你的Github邮箱地址。随后会让你键入想要保存的ssh key的文件名

>  Enter a file in which to save the key

这里注意，建议不输入任何文件名，直接回车，这样就使用系统默认的设置。那么在“C:\Users\UserName”文件夹下就会创建.ssh文件夹，文件夹中生成“id_rsa”和“id_rsa.pub”两个文件，分别对应私钥和公钥。

随后复制“id_rsa.pub”的内容到GitHub网站的Settings-->SSH and GPG keys中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820154002405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xsZmpmeg==,size_16,color_FFFFFF,t_70 =600x350)
设置title（任意），并将“id_rsa.pub”的内容复制到“Key”之中。

3. 测试SSH Key是否配置成功：

> ssh -T git@github.com

第一次测试，在continue的时候，选择“yes”，即可显示成功认证。

==特别说明：如果你在上述创建SSH Key文件的时候键入新的文件名，则需要将新文件挂靠到ssh-agent上，否则会出现“git@github.com: Permission denied (publickey)”的错误。==

详细的操作参见：[Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

# 配置GitHub的用户名和邮箱
使用Git Bash配置本地使用Git的全局设置。

[配置用户名](https://help.github.com/en/articles/setting-your-username-in-git)
> git config --global user.name "your name"

"your name"替换成你的GitHub用户名。

[配置邮箱](https://help.github.com/en/articles/setting-your-commit-email-address)

> git config --global user.email "email@example.com"

这里"email@example.com"替换成你的GitHub邮箱。
# 创建GitHub项目并在本地进行同步
## 访问GitHub网站并新建代码仓库
GitHub可以很方便地创建新的代码仓库“New Repository”：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820110656821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xsZmpmeg==,size_16,color_FFFFFF,t_70 =500x500)
填写仓库的名称“Repository name”，添加描述“Description”，还可以添加README（此处先不添加，后续采用命令行操作），以及添加忽略文件和开源协议。
## 创建本地代码仓库
首先在本地规划好一处文件夹用于同步GitHub的项目，然后打开Git Bash，定位到此次你想要同步的GitHub项目的文件夹，使用“cd”命令。

[接下来将在此文件夹添加刚才创建的GitHub的项目。](https://help.github.com/en/articles/adding-an-existing-project-to-github-using-the-command-line)

1. 初始化本地文件夹作为一个Git仓库：

> git init

2. 拷贝GitHub网站中的项目网址：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820113457659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xsZmpmeg==,size_16,color_FFFFFF,t_70 =450x230)
3. 添加远程代码仓库的URL：

> git remote add origin `remote_repository_URL`

`remote_repository_URL`替换为刚才拷贝的项目的URL。说明，origin指代远程代码仓库（GitHub中），master表示本地的主分支。验证一下添加是否成功：

> git remote -v


4. 首先从远程代码仓库拉取数据

> git pull origin master

5. 新建README文档，README文档是每个GitHub项目必备，说明项目内容。上文没有创建，在此处完成。

> touch README.md

6. 添加文件夹中的所有文件：

> git add .

7. 提交文件：

> git commit -m "First commit"

注意commit只在本地提交，并未同步到远程服务器。

8. 推送本地更新至远程服务器：

> git push -u origin master

注意：这里第4步，首先从GitHub拉取数据，很多教程上忽略这个步骤。这样容易导致出现本地版本和远程版本冲突的困境。详见错误[Updates were rejected because the tip of your current branch is behind](https://stackoverflow.com/questions/22532943/how-to-resolve-git-error-updates-were-rejected-because-the-tip-of-your-current)。

# 其他说明
GitHub官方提供了GUI的工具[GitHub Desktop](https://desktop.github.com/)，可以比较方便操作Git，有兴趣的同学可以参考相关文档。我个人认为还是命令行方便，确保对所有细节的把握，在实际应用中更好的控制代码版本。 此外Git协议相关的内容还很多，本文只是阐述如何开始一个Github项目，更多内容可以参考[相关详细资料](https://www.liaoxuefeng.com/wiki/896043488029600)。
