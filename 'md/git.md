# 共通线

## Gerrit 登录

> http://128.196.0.211:8080

+ 右上角 sign in, 输入 云桌面 权限

+ Settings 设置 ssh key. 

## SSH key

> 本地生成 ssh key

```sh
# git bash 内
ssh-keygen -t rsa -C "Gerrit上自己的邮箱"
# 一路回车
```

ssh key 目录位置, C:/Users/你的云桌面ID/.ssh/id_rsa.pub 公钥文件里的内容, 新增到 Gerrit - Settings - ssh key 中

并且, 在 id_rsa.pub 的同级目录下新建文件 config, 写入如下内容:

```
Host *
    KexAlgorithms +diffie-hellman-group1-sha1
```

 否则会报错 "diffie-hellman-group1-sha1 "
 
## clone 工程到本地

+ Gerrit  Projects-list-对应工程-General-clone with commit-msg hook - SSH

  指定路径 右键打开 git bash, git clone 贴在 git bash 命令行里
 
 从服务器 clone 代码到本地应该采取 clone with commit-msg hook 方式

 否则会报错 ERROR: missing Changed-id in commit message footer

# 审核者

## 权限设置
 
 Projects-list-对应工程-Access

## 创建分支

 在 ICDP 内创建: 研发服务 - 代码仓库 - 代码集成管理 页面, 点击 + 创建

## 审核者审核

 All-open-选择对应工程

 右侧添加 review， 点 reply， 打分后 submit

# 开发者

## 与 git 的不同

+ Gerrit 不允许用户直接 push 代码到分支上

git push origin HEAD:refs/for/分支名

如: git push origin HEAD:refs/for/master

否则会报错 prohibited by Gerrit

+ 不允许推送本地创建的分支到 gerrit 上

 建议采取, 创建本地流, 完成开发后 merge 到 DEV1 流提交的方式开发

# APPENDIX: 

可能能解决 git 大文件限制

3.git fetch/git clone 失败

【现象】git clone 一个大的项目时失败，错误类似fatal: The remote end hung up unexpectedly | fatal: early EOF | fatal: index-pack failed

【原因】项目过大，受硬件限制（类似过载保护），clone过程中会中断

【解决】    先做一个浅：git clone --depth 1 <repo_URI>；将浅repo回复完全：git fetch --unshallow ；then do regular pull ：git pull --all