## Git协作开发

### 2 Git分支



#### 2.1 什么是分支？

![](C:\Users\Administrator\Desktop\Typora文档\Git协作开发\绘图1.png)

##### 2.1 分支合并的命令

- 列出所有的分支

  ```
  git branch
  ```

- 创建分支

  ```
  git branch [branch name]
  ```

- 创建并切换到分支

  ```
  git checkout -b [branch name]
  ```

- 修改分支名字

  ```
  git branch -m [old branch name] [new branch name]
  ```

- 切换到指定分支

  ```
  git checkout [branch name]
  ```

- 将指定的分支合并到当前分支

  ```
  git merge [branch name]
  ```

- 合并指定的两个分支

  ```
  git cherry-pick [branch a name] [branch b name]
  ```

- 删除指定分支

  ```
  git branch -d [branch name]
  ```

  

##### 2.2 分支合并的合并冲突问题

​		分支合并的冲突判断是基于行的，例如当我们的Version3创建了BugFixVersion分支与DevelopVersion分支，并且都对这两个分支的第i行进行了修改，那么当我们将这两个分支版本合并时，Git就会认为发生了合并冲突。此时我们就需要手动进入项目文件在冲突的位置做出修改

### 3 Github云端托管

#### 3.1 使用Github云端托管我们的代码

1. 注册Github账号
2. 在Github中创建一个仓库
3. 使用命令将我们的本地代码推送到GIthub云端

#### 3.2 常用命令

- 给我们的仓库地址创建一个别名

  ```
  git remote add [别名] [仓库地址]
  ```

- 将指定分支推送到指定仓库的指定分支

  ```
  git push -u [仓库地址别名] [分支名]
  ```

- 将指定的仓库拉取到我们的主机上

  ```
  git clone [仓库地址]
  这样就可以把我们的仓库拉取到我们当前目录下,并且该仓库在该目录下以一个文件夹的形式存在
  ```

- 从Github云端仓库中拉取指定的分支并==合并==到本地的当前分支

  ```
  git pull [仓库地址] [分支名]
  等价于
  git fetch [仓库地址] [分支名]
  +
  git merge [仓库地址/分支名]
  注意:这里不使用git clone是因为我们本地有对应仓库,只是仓库内的代码待更新,因此只需使用git pull进行更新
  ```

- 删除Github上的指定分支

  ```
  git push [远程仓库别名/仓库地址] -d [分支名]
  ```

- 拉取Github云端仓库的指定分支到本地版本库([仓库别名]/[分支名])

==注意==:若我们的仓库包含了多个分支,而我们在拉取仓库后会发现只有一个分支,但是其实我们还是可以通过`git checkout [branch name]`切换到指定分支

### 4 Rebase

#### 4.1 Rebase的作用

1. 将多个版本提交记录合并

   <img src="C:\Users\Administrator\Desktop\Typora文档\Git协作开发\绘图1-16479260109081.png" style="zoom:67%;" />

2. 将分支结构合并到主线

   ```
   1.git checkout [分支名]
   2.git rebase [主线名]
   3.git checkout [主线名]
   4.git merge [分支名]
   ```

3. 在不产生分支合并记录的情况下从Github云端拉取

   原因：我们知道`git pull`等价于`git fetch`与`git merge`的组合因此其会涉及到合并操作，这样就会导致我们的日志记录不清晰

   ```
   git fetch [仓库地址] [分支名]
   git rebase [仓库地址/分支名]
   此操作会将抓取到的分支内的文件与我们的当前分支进行合并,并且不会产生合并记录
   注意:如果合并时发生了合并冲突,rebase操作便会自动暂停,此时我们需要手动解决冲突后再让rebase操作继续进行
   git rebase --continue
   ```

   

#### 4.2 常用命令

- 合并最新记录到指定记录之间的所有记录

  ```
  git rebase -i [记录的标识数字串]
  合并HEAD指针指向的记录以及其之前的三条记录合并为一条记录
  git rebase -i HEAD~3
  从根记录开始所有记录
  git rebase -i --root
  注意:合并时最好不要包含已经提交到Github云端仓库的版本的记录	
  ```

- 以图形结构呈现操作记录

  ```
  git log --graph
  以简洁的图形进行展示
  git log --graph --pretty=format:"%h %s"
  ```

#### 4.3 哪些操作会被纳入`git log`的记录中?

1. 版本提交操作
2. 分支合并操作

### 5 Git多人协同开发

#### 5.1 多人协同开发基本结构

<img src="C:\Users\Administrator\Desktop\Typora文档\Git协作开发\Git协同开发结构-16479370572601.png" style="zoom:75%;" />

#### 5.2 Github组织的创建与使用

### 6 Git的配置文件

- 项目配置文件设置

  ```
  git config --local user.name ''
  git config --local user.emial ''
  该配置文件放置在我们管理的文件夹的根目录下的.git目录下的config文件中
  Windows：.git/config
  ```

- 当前系统用户全局配置文件设置

  ```
  git config --global user.name ''
  git config --global user.emial ''
  其信息存储在当前用户的.gitconfig文件中
  
  Windows：C:\Users\Administrator\.gitconfig
  Linux：~/Home/.gitconfig
  
  注意：该配置文件维护的是电脑的某一个用户的配置文件
  如我们当前电脑有：86133，Administrator，CDFAccount，Default四个用户，该配置方法只能管理当前用户的配置，而全系统配置则管理的所有的这四个用户的配置
  ```

- 全系统配置文件设置

  ```
  git config --system user.name ''
  git config --system user.emial ''
  需要具备最高的管理权限才能修改
  注意：该配置文件维护的的是电脑的所有用户的配置文件
  Linux：~/etc/.gitconfig
  ```

==注意==:`git remote add `默认添加在本地的配置文件中

### 7 Git免密登录

1. 基于URL的免密登录

   ```
   原地址：https://github.com/tangling0112/GitTestProgram.git
   修改后的地址：https://用户名：密码@github.com/tangling0112/GitTestProgram
   
   使用修改后的地址作为我们的仓库地址便可以让我们能够在向Github云端Push代码时无需输入账户名与密码
   ```

2. 基于SSH的免密登录

   ```
   1.在自己电脑上生成本机的公钥与私钥（生成位置在生成过程中会有提示） 
   	ssh-keygen -r rsa
   2.此时在对应.ssh目录下会会生成id_rsa(私钥)与id_rsa.pub(公钥)两个文件
   3.拷贝公钥内容(以记事本打开公钥文件)
   4.在Github中的Settings中找到SSH and GRG keys将我们的公钥内容放置到其中
   5.拷贝Github中的SSH文件传输连接
   6.将第5步拷贝的SSH文件传输链接作为仓库地址设置好别名
   7.使用第6步的别名做Push操作
   ```

3. 基于Git的自动管理凭证实现免密登录

==注意==:当我们前面发现以任何方式都无法正常`Push`操作时(比如:`ping github.com`能有正常的回应等等),很可能是用户验证操作发生了一定错误,因此们可以通过配置==SSH免密登录==来修正这一错误,让我们能够正常的进行`Push`操作

### 8 使用`gitignore`忽略文件

#### 8.1 作用

#### 	8.2 常用的忽略配置

- 忽略指定文件

  ```
  [文件全名(包括后缀)]
  例:name.py
  ```

- 忽略所有指定后缀的名字

  ```
  *.[后缀名]
  例:*.py
  ```

- 忽略指定目录下的全部文件

  ```
  [目录名]/
  例:etc/
  ```

==注意==:可以在https://github.com/github/gitignore下找到我们需要的忽略文件的通用配置

### 9 Github任务管理
