# 1. SSH

首先需要配置 ssh.

查看现有 ssh 密钥.

```
ls -al ~/.ssh 		# 如果是windows, 查看用户目录下的.ssh文件夹
```

如果看到类似

```
id_rsa      id_rsa.pub
```

则已有密钥对, 否则需重新生成.

生成 SSH 密钥对

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

复制公钥

```
cat ~/.ssh/id_rsa.pub
```

并将公钥配置到 github/gitlab. 之后我们即可使用 ssh 免密对 github/gitlab 仓库执行各种操作.

# 2. clone

用于将一个远端仓库 clone 到本地. 这个相信都不陌生, 就略过了.

# 3. 建立仓库

>工作场景: 在本地已有项目的基础上, 建立git仓库并推送到远端.

`.git` 是一个隐藏文件夹，它位于 Git 仓库的根目录中。这个文件夹用于存储 Git 仓库的所有版本控制信息、配置文件和对象数据。Git 使用 `.git` 目录来管理和追踪文件的历史版本、分支、提交记录等元数据。简单来说，`.git` 是 Git 仓库的核心部分，所有关于项目的版本控制信息都保存在这个文件夹中。

首先切换到项目根目录, 初始化git仓库.

```
git init
```

我们即可看到一个 .git 文件夹, 里面是这个仓库的 git 信息. 可以看到我们默认在 master / main分支.

```bash
$ git log
致命错误：您的当前分支 'master' 尚无任何提交
```

此时可以看到, 我们的 git 仓库内没有任何提交. 下面进行第一次提交:

```bash
$ git add .
$ git commit -m "init"
[master（根提交） cb18cd3] init
 1 file changed, 58 insertions(+)
 create mode 100644 README.md
```

再次 `git log`, 即可看见我们第一次提交信息.

![image-20241126155343722](/home/lg/.config/Typora/typora-user-images/image-20241126155343722.png)

但此时我们的操作都是在本地进行的, 云端在哪呢? 如何提交到云端呢? 

在你的 github / gitlab 新建一个空仓库(注意不要添加README)文档. 建立成功后是如下这样的:

![image-20241126155630660](https://raw.githubusercontent.com/mlger/Pict/main/new_path_tblg_arch/image-20241126155630660.png)

这里意味着云端仓库为空, 等待本地仓库提交. 但本地仓库如何识别云端呢? 注意到这里有一个 `remote add` 语句.

```bash
$ git remote add origin git@github.com:mlger/gittutor.git
```

这里是建立本地仓库对远端仓库的链接. 以后所有的 push / pull 对应的远端, 都是 add url 对应的云端仓库.

此时我们可以将本地的提 `push` 到云端.

```bash
$ git push
致命错误：当前分支 master 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

    git push --set-upstream origin master

为了让没有追踪上游的分支自动配置，参见 'git help config' 中的 push.autoSetupRemote。
```

可以看到, 不出意外的失败了. 我们来分析一下原因.

本地和云端分别是一个 git 仓库. 每个 git 仓库可以看作由多个分支构成的有向无环图结构, 如下:

![image-20241126160151395](https://raw.githubusercontent.com/mlger/Pict/main/new_path_tblg_arch/image-20241126160151395.png)

由于我们本地的每次 `commit`, 都是在某个分支上进行的, 也就是只更新这个分支. 因此推送到远端的时候, 我们也需要设定远端分支与其对接. 我们可以使用 `--set-upstream` 或 `-u` 进行设定.

```bash
$ git push -u origin master       # 将当前分支(master)的提交推送到远端(origin)的master分支
枚举对象中: 3, 完成.
对象计数中: 100% (3/3), 完成.
使用 16 个线程进行压缩
压缩对象中: 100% (2/2), 完成.
写入对象中: 100% (3/3), 1.14 KiB | 1.14 MiB/s, 完成.
总共 3（差异 0），复用 0（差异 0），包复用 0（来自  0 个包）
To github.com:mlger/gittutor.git
 * [new branch]      master -> master
分支 'master' 设置为跟踪 'origin/master'。
```

再刷新我们的远端仓库, 可以看到本地目录已经传输到远端.

![image-20241126160513101](https://raw.githubusercontent.com/mlger/Pict/main/new_path_tblg_arch/image-20241126160513101.png)

至此, 我们完成了:

- 将本地项目初始化为 git 仓库.
- 建立 github 空仓库.
- 本地仓库提交(add commit)
- 将本地仓库链接到远端. (add remote)
- 将本地仓库的变更提交到远端(push)
- 将本地分支链接到远端分支(push -u origin)

# 4. commit and add

