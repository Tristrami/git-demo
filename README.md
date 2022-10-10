# Git

- [Git](#git)
  - [Basic](#basic)
    - [demo-repo](#demo-repo)
    - [SSH-KEY](#ssh-key)
    - [配置 git](#配置-git)
    - [git 常用命令](#git-常用命令)
    - [demo-repo2](#demo-repo2)
  - [Branching](#branching)
    - [在本地创建新分支](#在本地创建新分支)
    - [提交新分支到 Github](#提交新分支到-github)
    - [在 Github 提交 pull requset](#在-github-提交-pull-requset)
    - [拉取更新后的代码](#拉取更新后的代码)
    - [删除已经合并的 branch](#删除已经合并的-branch)
    - [提交冲突](#提交冲突)
  - [Undoing](#undoing)
    - [undoing add](#undoing-add)
    - [undoing commit](#undoing-commit)
  - [上传整个项目](#上传整个项目)

## Basic

### demo-repo

> 学习使用 git 从 github 仓库获取项目代码

在 github 创建仓库 **demo-repo**

![demo-repo](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/demo-repo.png)

在 vscode 中使用 git 通过 http url 从 github 拉取 demo-repo

```shell
git clone https://github.com/Tristrami/demo-repo.git
```



### SSH-KEY

> https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

使用 github 邮箱生成 ssh-key

```shell
ssh-keygen -t rsa -b 4096 -C "seamew121@gmail.com"
```

这里指定将生成的 key 存储到 /Users/mac/.ssh/github_ssh_key 文件中

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/mac/.ssh/id_rsa): /Users/mac/.ssh/github_ssh_key
Created directory '/Users/mac/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/mac/.ssh/github_ssh_key
Your public key has been saved in /Users/mac/.ssh/github_ssh_key.pub
The key fingerprint is:
SHA256:8COoe+JiG0QOHPd7maELeS1bBDza+2Ibd5uETR3UHl8 seamew121@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
| . ...     ..    |
|. o .o.   .  o  E|
|.o  o.oo   .. o .|
|+  ..o=o+ . .. . |
| o o.=o*S. .     |
|.  .o.*.+.       |
| ..  +.o +       |
|..o..oo.o o      |
|.++o..o  o       |
+----[SHA256]-----+
```

在 ~/.ssh/ 目录下查看生成的 key 文件

```shell
ls | grep github_ssh_key
```

一个是 public key，一个是 private key

```shell
github_ssh_key
github_ssh_key.pub
```

将 github_ssh_key.pub 中的内容复制到剪切板

```shell
pbcopy < ~/.ssh/github_ssh_key.pub
```

在 github 的 settings 中找到 **SSH and GPG keys** 选项，点击 **new SSH key**，将 github_ssh_key.pub 中的内容粘贴并保存

![ssh-key](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/ssh-key.png)

将 ssh-key 添加到 ssh-agent

- 启动 ssh-agent

```shell
exec ssh-agent zsh
```

- 将以下内容粘贴到 `~/.ssh/config` 文件中，如果文件不存在就创建该文件

```shell
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github_ssh_key
```

- 将 ssh-key 添加到 ssh-agent

```shell
ssh-add --apple-use-keychain ~/.ssh/github_ssh_key
```



### 配置 git

配置 username

```shell
git config --global user.name seamew
```

配置 email 

```shell
git config --global user.email seamew121@gmail.com
```

配置默认的 branch name

```shell
git config --global init.defaultBranch main
```

使用 `git config --list` 查看 git 配置

```shell
credential.helper=osxkeychain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
user.name=seamew
user.email=seamew121@gmail.com
core.editor=vim
init.defaultbranch=main
```



### git 常用命令

- 如果是本地新创建的 repo，使用 `git init` 来初始化该项目
- 使用 `git status` 查看当前 working tree 的状态，看看是否有修改或添加的文件
- 如果有更新的文件，使用 `git add <fileName>` 来 track 新的文件，或者直接使用 `git add .` 来 track 所有的文件
- 使用 `git commit` 来提交所有的更新，至此文件还是在本地，最常用的方式是 `git commit -m <heading> -m <description>`，这样可以为这次的提交作一些说明
- 使用 `git push origin main` 来将代码提交到 `origin` 仓库的 `main` 分支
- 如果当前仓库是使用 `git clone` 拉下来的项目，那么 `origin` 是已知的，如果是在本地新创建的项目，origin 需要自己指定，最简单的方式先在 github 上创建一个新 repo，然后复制 repo 的 url，使用 `git remote add origin <url>` 来指定 `origin`
- 使用 `git checkout -b <branchName>` 来创建一个新分支，使用 `git checkout <branchName>` 来切换分支



### demo-repo2

> 学习在本地创建项目并使用 git 提交到 github

在 vscode 中创建 `demo-repo2` 项目

```shell
mkdir demo-repo2
```

创建 README.md

```shell
touch README.md
```

初始化项目

```shell
git init
```

使用 `git status` 查看 working tree 的状态

```shell
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

将所有文件添加到索引中

```shell
git add .
```

再次使用 `git status` 查看 working tree 的状态

```shell
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

我们现在想要将 demo-repo2 这个项目 push 到 github 上，首先需要在 github 上创建 demo-repo2

![demorepo2](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/demo-repo2.png)

复制 ssh 的 url

```shell
git@github.com:Tristrami/demo-repo2.git
```

指定 origin 为 demo-repo2 的 ssh url，指定 origin 的目的是告诉 git 我们想要把代码 push 到哪个 github repo

```shell
git remote add oringin git@github.com:Tristrami/demo-repo2.git
```

使用 `git remote -v` 查看我们配置的 origin 

```shell
origin  git@github.com:Tristrami/demo-repo2.git (fetch)
origin  git@github.com:Tristrami/demo-repo2.git (push)
```

指定了 origin 之后，我们就可以将项目 push 到 github 了

```shell
git push origin main
```

这里可能会遇到一个错误

```shell
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

解决方法

```shell
ssh-keyscan github.com >> ~/.ssh/known_hosts
```

这里有个小技巧，如果觉得 `git push origin master` 每次都要指定 `origin` 和 `master` 很麻烦，我们可以使用 `git push -u origin main`，这个命令等价于 `git push --set-upstream origin main`,也就是为当前分支设置 upstream，  这样以后就可以只用输入 `git push` 命令，git 会默认 push 到我们在当前分支指定的 upstream ( `origin` 和 `branch`)



## Branching

> 学习分支的概念及操作，继续使用 demo-repo2

### 在本地创建新分支

我们可以为我们的工作树创建分支，不同分支上的代码互不影响，这里创建一个 `feature-readme-instructions` 分支

```shell
git checkout -b feature-readme-instructions
```

使用 `git branch` 查看当前的分支，现在处于 `feature-readme-instructions` 分支上

```sehll
git branch
* feature-readme-instructions
  main
```

在 README.md 添加一些内容

```markdown
# Demo 2

## Local Development

1. Open index.html in your browser

## Notice

1. Here's some notice
```

这是原来 `main` 分支上的 `README.md`

```markdown
# Demo 2
```

将变更添加到 `feature-readme-instructions ` 分支

```shell
git add README.md
```

在 `feature-readme-instructions` 分支上提交变更

```shell
git commit -m "updated readme" -m "added new sections Local Development and Notice"
```

此时我们返回 `main` 分支

```shell
git checkout main
```

发现 `README.md` 的内容在 `main` 分支上没有变化

```markdown
# Demo 2
```

我们可以使用 `git diff feature-readme-instructions` 来查看当前 `main` 分支和 `feature-readme-instructions` 分支的不同

```shell
diff --git a/README.md b/README.md
index ad2c15a..30ec187 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,9 @@
 # Demo 2
+
+## Local Development
+
+1. Open index.html in your browser
+
+## Notice
+
+1. Here's some notice
```



### 提交新分支到 Github

现在我们可以将本地的 `feature-readme-instructions` 提交到 github 仓库的 `feature-readme-instructions` 分支上了，这里的 `-u` 参数为 `feature-readme-instructions` 分支设置了 upstream，下次在该分支上 push 时就不需要指定 origin 和 branch 了

```shell
git push -u origin feature-readme-instructions
```



### 在 Github 提交 pull requset

我们回到 github，现在可以提交一个新的 pull request，尝试将  `feature-readme-instructions` 中新增的内容合并到 `main` 分支中

![pull-request](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/pull-request.png)

提交新的 pull request，注意是从 `feature-readme-instructions` 分支到 `main` 分支

![create-pull-request](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/create-pull-request.png)

pull request 提交完成后，我们可以在我们更改的代码上添加一些 comments

![add-comments-on-code](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/add-comments-on-code.png)

作为 repo 的拥有者，我们可以选择将 pull request 合并到 mian 分支中，也就是 merge

![check-out-pull-request](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/check-out-pull-request.png)

合并完成啦 🎉 ，查看 `main` 分支中的 `README.md` 发现和 `feature-readme-instructions` 分支中的一样了

![merge-completed](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/merge-completed.png)



### 拉取更新后的代码

我们在 github 上完成了两个不同 branch 之间的 merge，但对于本地来说，没有任何改变，所以我们需要从 github 上 `main` 分支拉取最新的内容并和我们本地的 `main` 分支内容合并，我们可以使用 `git pull origin main` （由于设置了 upstream，`origin` 和 `main` 也可以省略）

```shell
git pull origin main
```

此时查看 `main` 分支上的 `README.md` 文件内容，发现和 github 上同步了

```markdown
# Demo 2

## Local Development

1. Open index.html in your browser

## Notice

1. Here's some notice
```



### 删除已经合并的 branch

我们现在已经把  `feature-readme-instructions` 分支上的内容成功合并到了 `main` 分支中， `feature-readme-instructions` 这个分支就已经没有作用了所以我们可以删除它

```shell
git branch -d feature-readme-instructions
```



### 提交冲突

在 `main` 分支中有一个 `index.html ` 文件，并且已经 commit

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>hello</h1>
    <p>Today is a good day</p>
  </body>
</html>
```

现在创建一个新分支 `test`

```shell
git checkout -b test
```

修改  `index.html ` 中的内容

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>hello</h1>
    <p>Today is a good day</p>
    <p>How are you</p> <!-- 添加的新行 -->
  </body>
</html>
```

使用 `git commit -am "updated index.html" -m "added a new line"` 提交修改的内容，如果只修改或删除了文件内容，那么可以在 `git commit` 命令后加上 `-a` 参数，来让 git 自动 track 被删除或修改的文件，然后 commit，这样就省去了 `git add`

```shell
git commit -am "updated index.html" -m "added a new line"
```

这时我们再切换到 `main` 分支

```shell
git checkout main
```

修改 `index.html` 文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>hello</h1>
    <p>Today is a good day</p>
    <p>Hello there</p> <!-- 添加的新行 -->
  </body>
</html>
```

提交 `main` 分支的修改

```shell
git commit -am "updated index.html" -m "added a new line"
```

再切换到 `test` 分支

```shell
git checkout test
```

使用 `git diff main` 看看 `test` 分支和 `main` 分支上的内容有什么不同

```shell
diff --git a/index.html b/index.html
index 05f8029..6ed3a35 100644
--- a/index.html
+++ b/index.html
@@ -9,6 +9,6 @@
   <body>
     <h1>hello</h1>
     <p>Today is a good day</p>
-    <p>Hello there</p>
+    <p>How are you</p>
   </body>
 </html>
```

此时如果使用 `git merge main` 来合并两个分支，会出现错误，因为两个分支修改了同一个文件，git 不知道要采用哪一个，产生了矛盾

```shell
git merge main
CONFLICT (add/add): Merge conflict in index.html
Auto-merging index.html
Automatic merge failed; fix conflicts and then commit the result.
```

此时 vscode 会给我们一些提示，询问我们要采用哪一个修改

![merge-conflict](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/merge-conflict.png)

我们选择 Accept Both Changes 选项，同时保留两者

![Accept-Both-Changes](https://seamew.oss-cn-hangzhou.aliyuncs.com/img/courses/freecodecamp/git/Accept-Both-Changes.png)

此时我们在 `test` 分支查看 `git status`，发现需要重新提交修改

```shell
On branch test
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both added:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

重新提交修改，提交后 `test` 分支就和 `main` 分支同步了，我们可以继续在 `test` 分支上开发自己的东西

```shell
git commit -am "updated with main"
```



## Undoing

> 学习回退操作

### undoing add

在 `test` 分支下，我们修改一下 `Readme.md`

```markdown
# Demo 2

## Local Development

1. Open index.html in your browser
2. Have fun <!-- 新增加的内容 -->

## Notice

1. Here's some notice
```

stage 修改后的文件

```shell
git add README.md
```

如果这个时候我们返回了，想撤回刚刚的提交，将 `README.md` 变为 ustaged 状态，我们可以使用 `git reset README.md`，也可以直接 ``

```shell
git reset
```

```shell
Unstaged changes after reset:
M	README.md
```

再使用 `git status` 查看状态，发现 `README.md` 状态变成了 unstaged

```shell
On branch test
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```



### undoing commit

如果我们选择 commit 修改后的 README.md

```shell
git commit -m "added install steps"
```

但是我们在 commit 之后反悔了，想要撤回这次的 commit，返回上次 commit 的状态

```
git reset HEAD~1
```

`HEAD` 指向最后一次 commit，`HEAD~1` 指的是让 `HEAD` 指针指向上一个 commit，执行完该操作后发现 `README.md` 又变为了 unstaged 状态

```shell
Unstaged changes after reset:
M	README.md
```

但如果我们想要撤销之前的某一次 commit 呢，这就无法使用上面的方式了，我们尝试使用 `git log` 来看看 commit 日志，发现每一个 commit 都后自己的 hash value

```shell
git log
commit 74076c99a6337f46f9018516be10ee11c0239635 (HEAD -> test)
Merge: 744b7d6 5ae5a3c
Author: seamew <seamew121@gmail.com>
Date:   Sun Oct 9 18:52:50 2022 +0800

    updated with main

commit 5ae5a3c57cc0dad41576a6997f2d4d0725819720 (main)
Author: seamew <seamew121@gmail.com>
Date:   Sun Oct 9 17:21:10 2022 +0800

    updated index.html

    added a new line

commit d3fd56ad7108f206d89a0c0dbfa1ef5c2608e617
Author: seamew <seamew121@gmail.com>
Date:   Sun Oct 9 17:18:34 2022 +0800

    created index.html

commit 744b7d6c98d9d4e7bce67b1fda77106d3eb9156b
Author: seamew <seamew121@gmail.com>
Date:   Sun Oct 9 17:16:57 2022 +0800

    updated index.html

    added a new line

commit 08c70946b56e1666fd3754c53051ce2c3a5b09b0
Author: seamew <seamew121@gmail.com>
Date:   Sun Oct 9 17:15:49 2022 +0800

    created index.html

commit ac27f56d7897dfaaa39c8e73bb8666282c379567 (origin/main)
Merge: 2eb3285 708e615
Author: Tristrami <75435038+Tristrami@users.noreply.github.com>
Date:   Sun Oct 9 16:40:03 2022 +0800

    Merge pull request #2 from Tristrami/feature-readme-instructions

    updated readme
```

当我们想要撤回某一次 commit 的时候，可以直接使用 `git reset <hashvalue>`，例如想要撤回 hash value 为 `744b7d6c98d9d4e7bce67b1fda77106d3eb9156b` 的这个 commit，要注意当我们撤回 commit 后，修改的内容是还存在的，只是 git 来说，这些修改变成了 unstaged 状态

```shell
git reset 744b7d6c98d9d4e7bce67b1fda77106d3eb9156b
```

如果我们撤回 commit 时，我们还想完全删除这些修改，而不是让修改的内容变为 unstaged 状态，可以这样做

```shell
git reset --hard 744b7d6c98d9d4e7bce67b1fda77106d3eb9156b
```



## 上传整个项目

这里遇到了个小坑，上传整个项目到 githun 的时候，发现在 github 上 demo-repo 和 demo-repo2 打不开

```
git
├── demo-repo
│   ├── README.md
│   └── index.html
├── demo-repo2
│   ├── README.md
│   └── index.html
├── git.md
└── img
    ├── Accept-Both-Changes.png
    ├── add-comments-on-code.png
    ├── check-out-pull-request.png
    ├── create-pull-request.png
    ├── demo-repo.png
    ├── demo-repo2.png
    ├── merge-completed.png
    ├── merge-conflict.png
    ├── pull-request.png
    └── ssh-key.png
```

这里需要清除  demo-repo 和 demo-repo2 中的 .git/ 目录，并且把两个项目从 git 的 work tree 中删除

- 删除 `.git` 目录 : `rm -rf .git/`
- 把两个项目从 git 的 work tree 中删除 : `git rm --cached demo-repo`，`git rm --cached demo-rep2`
- 回到 git 目录下 : `git add .` => `git commit -m "remove demo-repo and demo-repo2 from work tree"` => `git push origin main`
