---

---

# git使用笔记

### git基本操作

个人理解：这里指针运用的十分频繁，每一步都是和指针有关的。HEAD就是指向当分支(maste：指向当前分支当前的版本)的当前版本。每当创建一个新的版本时，master就会指向最新的版本，而HEAD指向master。每一个版本的创建好比是一个时间线上(时间线是一个直线)的节点。当创建分支时，就好比在当前版本再创建一个指针(dev)，和当前master指向同一个版本，切换分支时HEAD指向dev，挡在分支dev操作时和maetr没有关系，master还是指向创建分时时的版本。合并分支时就是将master以向dev指向的版本（Faster-forward合并）。

__git-branch理解示意图__

![git-branch](git_picture/git-branch.jpg)

- 初始化git库

  `git init`

  会生成一个.git文件

- 创建用户名和邮箱

  当你提交修改时有提示

  `git config --global user.name 'username'`

  `git config --global user.email 'example@gmail'`

  说明：这两条语句也可修改用户名、邮箱（全局修改）

- 版本创建

  `git add file_name`

  说明：add 添加到暂存区中

  `git commit -m '版本信息'`

- 查看版本记录

  `git log`

  `git log --pretty=oneline`

  说明：每一次提交版本在一行中简要显示

  `git log --graph --pretty=oneline`

  说明：增加查看，合并记录

- 版本退回

  `git reset --hard HEAD^`

  说明：HEAD是指针，指向当前版本，HEAD^是前一个版本

  `git reset --hard 版本序号`

  说明：可以退回任意版本

- 查看操作记录

  `git reflog`

- 工作区、版本库和暂存区

  - git add 是把暂存区的修改放到工作区中
  - git commit 是把暂存区的修改一次性做以此版本记录

- 管理修改

  - git commit 只会把暂存区的修改提交到版本记录中

- 撤销修改

  1. 直接丢弃工作区改动

     `git checkout -- file_name`

  2. 修改已经添加到工作区，但未commit

     a. `git reset HEAD file_name`

     b.`git checkout -- file_name`

  3. 已经提交(commit)

     版本退回
  
- 对比文件不同

  1. 对比工作区和版本库某个文件

     `git diff HEAD -- file_name`

     说明：工作区和暂存区都可以是这条语句查看

  2. 对比两个版本中的文件

     `git diff HEAD HEAD^ -- file_name`

     说明：比较当前版本的file_name和前一个版本的file_name文件

- 删除文件

  1. 删除文件

     a. `rm file_name`

     b. `git rm file_name`

  2. 提交

     `git commit -m '版本说明信息'`

     

### git创建分支(branch)

- 查看分支

  `git branch`

- 创建分支

  `git branch name`

- 切换分支

  `git checkout name`

- 创建\+切换分支

  `git checkout -b name `

- 合并某分支到当前分支

  `git merge name`

  说明：有快速合并，就是用Faster-forward，但是版本没有说明信息，用的是分支的版本号

- 删除分支

  `git branch -d name`

- 解决合并冲突

  1. 手动合并

  2. 在进行提交

     `git merge dev`

     `git log --graph --pretty=oneline`

     说明：查看合并记录

  说明：产生合并冲突原因，由于创建分支时，master指向的分支当前版本也进行了修改，所以不能进行Faster-forward合并。
  
- 禁用快速合并

  `git merge --no-ff -m '禁用快速合并Faster-forward 说明信息(版本信息)' 'other_branch_name'`

  说明：一般我们都禁用快速合并功能，这样可以记录合并的信息

- git_bug分支

  1. 保护你当前分支正在编辑的文件

     a. `git stash`

  2. 切换到出现bug的分支，再创建临时bug_001分支进行修改bug，然后提交

  3. 进行bug分支合并时，要禁止快速合并（查看不了合并记录）

     a.`git merge --no-ff -m '修复bug_001' bug_001`

  4. 切换回你之前工作的分支，进行现场恢复

     a.查看保存的工作现场：`git stash list`

     b.恢复工作现场：`git stash pop`

  

### git 操作流程图

![git-操作流程图](git_picture/git-操作流程图.png)

### github使用

- 创建创库

- 生成ssh公钥（Linux版Ubuntu）

  1. 切换用户目录下

     `cd ~`

  2. 查找隐藏文件（.gitconfig）编辑文件（实际在设置用户是就已经编辑好了，打开看一下用户名、邮箱）

     `ll -a`

     `vim .gitconfig`

  3. 生成ssh密钥

     `ssh-keygen -t rsa -C 'ShenDeZ@163.com'`

     ```c
     Generating public/private rsa key pair.
     Enter file in which to save the key (/home/ss/.ssh/id_rsa): 
     Created directory '/home/ss/.ssh'.
     Enter passphrase (empty for no passphrase): 
     Enter same passphrase again: 
     Your identification has been saved in /home/ss/.ssh/id_rsa.
     Your public key has been saved in /home/ss/.ssh/id_rsa.pub.
     The key fingerprint is:
     SHA256:weCTO83pWeZXqCsuVobfKVVf808v59n49F7pMuoNTxw ShenDeZ@163.com
     The key's randomart image is:
     +---[RSA 2048]----+
     |      .          |
     |     . +         |
     |      + o        |
     |       = o  .. ..|
     |      o.S o..E..o|
     |      .oo=....o +|
     |       +oooo.o o=|
     |      o + oo=oooO|
     |     . o.ooo.o+O*|
     +----[SHA256]-----+
     ss@ubuntu:~$ cd .ssh
     ss@ubuntu:~/.ssh$ ls
     id_rsa  id_rsa.pub
     ss@ubuntu:~/.ssh$ cat id_rsa.pub
     ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJkzO+jEgG3da3R6JylGmV0mULYcy/ElRbY8psSV2sVZtcMsKrW/QRKw5ZLK7/CDqZsK04nl7BmO87ReNP+64mrNIxVaEUUsAvMM6KKO+u0PCRnJP06+cbcZqX+vMfpajo+s5/CmHk/ZS+uIZRzSosCjmSAe0PCSuq3VU4KHwrm+lWN0N0C4uNqAqIi/MBtUKXPolLjnZ5D6b8e4lLk68YbdR6BdvZ4OrAbl3+p7/3X6C7/K1Bmp8dipH5cNRvph+bRyJ7/DAarD3qgTefRTfU0ANFgTAxoqLZYFTUToxGYBTiTpr0kPkfyF322awZ1Q7ubOXXEXmD3dvPL01+b4tJ ShenDeZ@163.com
     
     ```

     说明：输入上面语句，出现待输入的（密码，确认值）都__按回车__，__id_ras为私钥__、__id_rsa.pub__为公钥

  4. 将ssh公钥复制粘贴到github上去

     说明：这样本地可以操作github上的项目
     
  5. window上的操作和Linux相同
  
     - 切换到用户目录
     - 查看 .gitconfig 文件
     - 使用 `ssh-keygen -t rsa -C '邮箱'` 生成,公钥、私钥在用户目录 .ssh 文件夹中
     - 查看`.ssh`文件夹
     - 参考Linux操作

- 就将公钥添加到’__人设置中__的__SSH and GPG keys__

- 克隆项目

  1. 查找克隆地址（https、ssh）选择uoge协议进行克隆

     `git clone 克隆地址`

  2. 如果克隆出错执行下面两句

     `eval "$(ssh-agent -s)"`

     `ssh-add`

     说明：不是很理解！！！！

- 进行开发

  - 创建分支进行编辑代码

- 推送分支

  1. `git push origin 'branch_name'`

     说明：将本地分支推送到github服务器上，服务器会自动创建一个分支

- 本地分支跟踪远程服务器分支

  1. `git branch --set-upstream-to=origin/'远程branch_name' '本地branch_name'`

     ```c
     Branch 'smart' set up to track remote branch 'smart' from 'origin'.
     
     使用 git status
         On branch smart
         Your branch is up to date with 'origin/smart'.
     
         nothing to commit, working tree clean
     ```

  2. 当编辑本地分支并提交一个版本时，使用`git status`查看工作区时提示（本地分支，领先服务器分支一次提交）

     ```c
     On branch smart
     Your branch is ahead of 'origin/smart' by 1 commit.
       (use "git push" to publish your local commits)
     
     nothing to commit, working tree clean
     
     ```

  3. 推送代码（是服务器分支与本地分支，保持一致性）

     `git push`

     说明：就是将本地的分支合并到服务器上

  4. 拉取代码

     `git pull origin 'branch_name'`

     说明：把服务器的分支合并到本地









