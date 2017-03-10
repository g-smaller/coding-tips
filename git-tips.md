### Git 配置
> Git config 命令用来配置变量和外观；信息存在三个不同的位置。

- `/etc/gitconfig`

    > 针对所有用户。 如果使用带有 `--system` 选项的 git config 时，它会从此文件读写配置变量
- `~/.gitconfig或者~/.config/git/config`
    > 针对当前用户。 可以传递 `--global` 选项让 Git 读写此文件。
- `.git/config`
    > 针对当前仓库

### 初始化仓库
> git init [repo_name]

*如果缺少`repo_name`在当前文件夹下初始化仓库*

### 检出仓库
> git clone repo_url

*仓库可以是本地仓库，也可是服务器上的仓库*
```
git clone /path/repo_name
git clone usrname@host:/path/repo_name
```

### 工作流
- git在本地仓库维护这'三棵树',分别为
    - 工作区(working)
    
        > 修改文件
    - 暂存区(stage)
        > 暂存文件，将文件的快照放入暂存区域。
    - 本地仓库
        > 暂存文件，将文件的快照放入暂存区域；这时文件只存在本地仓库还未同步到远端服务器；

### 检查当前文件状态
> git status [-sb]

- s

   > --short  输出状态报告
- b 
   > 输出当前分支

### 忽略文件
文件名：`.gitignore`
##### 规范：
- 	所有空行或者以 *#* 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
    > glob 模式是指 shell 所使用的简化了的正则表达式
    
    ```
    星号（*）匹配零个或多个任意字符；
    [abc]匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；
    问号（?）只匹配一个任意字符；
    如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）;
    两个星号（*) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z, a/b/z 或 `a/b/c/z`。
    ```
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

### 查看已暂存和未暂存的修改
> git diff [--cached|HEAD|commit-id [commit-id1]]

- 默认值

    > 暂存前后的变化；
    > 比较工作区和暂存区的文件内容
- `--cached`
    > 已经暂存起来的变化；
    > 查看下次提交的已暂存的内容；就是暂存区和本地仓库内容的比较；
- `HEAD`
    > 查看本地文件修改的内容。就是工作区和本地仓库内容的比较；

### 添加文件到暂存区
> git add [.|filename|*]

### 提交文件到本地仓库
*从暂存区到本地本地仓库*
> git commit [|-m 'msg'|-i]

### 移除文件
*移除已跟踪的文件(从暂存区中移除)*
> git rm [-f|--cached] filename

- 默认

    > 删除被追踪没有修改的文件(即暂存没有修改的文件)；
- `-f`
    > 删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f（译注：即 force 的首字母）；
- `--cached`
    > 只想从暂存区中移除，希望保留工作区文件；

### 移动文件
*可以给文件重命名*
> git mv filename_from filename_to



```
运行 git mv 就相当于运行了下面三条命令:
    $ mv filename_old filename_new
    $ git rm filename_old
    $ git add filename_new
```

### 查看日志
查看提交日志
> git log

常用选项

- `-p`

    > 显示每次提交的内容差异
- `-n`
    > n是个数字，表示显示最近n次记录
- `--stat`
    > 显示每次提交的简略的统计信息：被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了
- `--graph`
    > 显示 ASCII 图形表示的分支合并历史
- `--oneline`   
    > 显示简短的提交信息
- `--decorate`
    > 显示分支、标签最后一个提交对应的提交对象
- `--since, --after`
    > 仅显示指定时间之后的提交。
- `--until, --before`
    > 仅显示指定时间之前的提交。
- `--author`
    > 仅显示指定作者相关的提交。
- `--committer`
    > 仅显示指定提交者相关的提交。
- `--grep`
    > 仅显示含指定关键字的提交
- `-S`
    > 仅显示添加或移除了某个关键字的提交

### 撤销操作

##### 修改提交信息

> git commit --amend

如果有暂存的文件时，运行次命令会覆盖原来的提交信息。

##### 取消暂存文件
> git reset HEAD filename

##### 取消对文件的修改
> git checkout -- filename

文件必须被跟踪才起作用

### 远程仓库的使用


##### 查看远程仓库
> git remote

    它会列出每个远程服务器的简写；
    如果是克隆一个远程服务器，默认有个默认名称`origin`。

- `-v`

    > 列出每个简写名称对应可读写的远程服务器地址；
        
##### 添加远程仓库
> git remote add *remote-repo-shortname* *url*

    shortname 是对URL的简称
    url       是远程仓库或者本地仓库地址
##### 从远程仓库抓取与拉取
> git fetch *remote-repo-shortname*

    该命令从远程仓库拉取本地还没的数据，它不会自动合并或修改；需要手动；
    如果本地分支跟踪了一个远程分支，可以使用`git pull` 来自动抓取然后合并远程分支到当前分支；
    
##### 推送到远程分支
> git push [remote-repo-shortname] [local-branch-name]

##### 查看远程仓库
> git remote show [remote-repo-shortname]

    会显现列表简称对于的远程仓库的信息已经本地分支情况。
##### 远程仓库的移除与重命名
> git remote rename [old-remote-repo-shortname] [new-remote-repo-shortname]

    修改远程仓库的简称
> git remote rm [remote-repo-shortname]

    移除本地对于远程仓库的简称

### 标签
##### 列出标签
> git tag 

- `-l 'tag'`
    
    > `tag` 可以是具体的标签名，也可以是表达式，例如：v*

##### 创建标签
> git tag -a 'tagname' -m 'tag-msg'

    还可以创建轻量标签git tag tagname，创建的标签只会显示提交信息，没有标签信息。

##### 后期打标签
> git tag -a tag-name commit-id
        
##### 共享标签
> git push [remote-repo-shortname] [tag-name|--tags]
    
##### 检出标签
> git checkout -b [branch-name] [tag-name]

### Git 别名
> git config [--system|--global] alias.<b>cmd-short-name</b> '命令'

    例如：git config --global alias.co checkout

### 分支

> git 每次提交，提交对象中都会包含一个提交对象、一个树对象和N个blob对象(保存着文件快照)

##### 创建分支
> git branch local-branch-name

##### 切换分支
> git checkout local-branch-name

##### 创建并切换分支
> git checkout -b local-branch-name

##### 合并分支
    git checkout current-branch-name
    git merge merge-branch-name
##### 删除分支
> git branch -d local-branch-name

    删除没有合并的分支会有警告

##### 强制删除分支
> git branch -D local-branch-name


##### 分支列表
> git branch

- `-v`

    > 查看每个分支最后一个提交
- `--merge` 
    > 查看分支列表中那些分支已经合并
- `--no-merge`
    > 查看分支列表中那些分支没有合并
    
#####  

    

