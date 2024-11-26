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

![image-20241126154630243](https://raw.githubusercontent.com/mlger/Pict/main/new_path_tblg_arch/image-20241126154630243.png)

```bash
$ git log
致命错误：您的当前分支 'master' 尚无任何提交
```

此时可以看到, 我们的 git 仓库内没有任何提交.