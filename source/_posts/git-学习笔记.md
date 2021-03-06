---
title: git 学习笔记
date: 2020-05-15 08:28:43
author:
categories: 技术
tags: git
---

# git 学习笔记
### 基础了解

![结构](https://cg2010studio.files.wordpress.com/2018/12/git-flow2.png)

1. git的三个区域（工作区（红色）、暂存区（绿色）、版本库）
2. 工作区（修改的、新增的，文件颜色：红色）
3. 暂存区（把修改的、新增的放入暂存区，文件颜色：绿色）
4. 版本库（生成后，查看状态不会提示）
5. 远程仓库（github/gitlab）

### 初次创建
1. **git init** :进入目标文件夹，初始化
2. **git status** :查看当前文件夹里文件的状态
3. **git add file1 file2**: 把需要管理的文件加入git管理
4. **git add . **:把没有管理的文件都加入git管理
5. **git config --global user.email 邮箱** 
6. **git config --global user.name 用户名**：
	个人信息配置（用户名、邮箱）
7. **git commit -m 'v1'**:生成一个v1版本

### 有第一个版本后，修改文件后
1. 查看状态：**git status**
2. 增加修改的新内容：**git add file**
	(file是被修改的文件或直接用” ***.*** “)
3. 生成下一个版本：**git commit -m 'v2'**
4. 查看版本日志：**git log**

### 回滚
1. 查看日志信息：**git log**
2. 回滚到版本1：**git reset --hard v1的版本号**
3. 查看变更日志信息：**git reflog**,找到版本2的版本号
4. 在回滚回版本2：**git reset --hard v2的版本号**
5. 将版本回滚到暂存区：**git reset --soft 前个版本号**
6. 将暂存区内文件回滚到工作区：**git reset HEAD**
7. 对已修改文件滚回到未修改时：**git checkout -- file**

### 分支(分支之间不会相互影响)
1. 查看分支：**git branch**
2. 创建新的分支：**git branch 新分支名**
3. 切换到新的分支下：**git checkout 新分支名**
4. 在新的分区下修改文件，保存添加新的版本
5. 在切换回主分支：**git checkout master**(没有新的版本日志)
6. 返回新的分支下，存在新的版本信息和修改
7. 再返回主分支，再可以创建多个分支
8. 分支合并，切换会主分区：**git merge 要合并的分支名**
9. 合并后可以删除被合并分支：**git branch -d 已经合并了的分支名**
10. 多分支基于同一版本的主分支合并可能会产生冲突！（需要手动修复）
11. **注意：**切换分支在合并

### 工作流
1. 建议从创建开始：建议创建两个分支（主分支master（默认）;副分支）
2. 主分支存放正式稳定内容;副分支用于开发测试内容
3. 等开发测试内容稳定后，在合并到主分区

### github代码托管
1. 注册github帐号,创建仓库
2. 将现有文件推送到github：
	**git remote add 别名 地址**(给地址添加别名使用)
3. 推送：**git push -u 别名 分支名**
4. 从github上下载代码：**git clone 仓库地址**
5. 这里克隆下来，查看分支只显示主分支，副分支也有，可直接切换
6. 克隆下来后，切换在副分支下开发，首先将副分支与主分支同步：
	**git merge master**
7. 当在云端产生新的修改后，在另一台已有部分文件的电脑，只需：
	**git pull 别名 分支名**
8. 分支名是指新修改或文件所在的分支，别名指地址的别名

### 解决本地和云端合并冲突
1. 当本地修改后未推送到云端，云端被其他用户修改后，
	与本地同步可能会产生冲突(修改了同一文件)
2. 手动修改冲突部分
3. 补充：
	**git pull 别名 分支名** 等同于
	**git fetch 别名 分支名** && **git merge 别名/分支名**

### rebase(变基) [面试考点]
1. **git rebase -i HEAD~n**:
	将最新n个版本记录合并（修改.git/config中的core下加入：editor=nvim）
2. 修改本地配置的编辑器，看你使用的是什么编辑器，我用的是nvim，
	记录合并最好不要与远程仓库已经记录的合并，减少冲突
3. 将分支记录和主分支记录合并成一条线：
	**git checkout 其他分支**
	**git rebase master**
	**git checkout master**
	**git merge 要合并的其他分支**
4. 将分支记录和主分支记录合并成带分支的记录：
	**git checkout master**
	**git merge 要合并的其他分支**
5. 云端本地合并冲突，防止记录分叉：
	**git fetch 别名 分支名**
	**git rebase 别名/分支名**
	提示：产生冲突 解决冲突;
	再**git rebase --continue**

### 多人协同开发 [面试考点]
***gitflow工作流思路***
1. 每个人或小组都会基于副分支创建一个自己的工作分支，各自在各自分支上进行开发
2. 开发项目完后成，检查通过，申请自己分支与副分支合并（合并过程中：有代码检查）
3. 可能会创建一个分支用于检测项目，修复项目，合格稳定后，再与主分支合并上线
4. 如果出现问题：要建立一个专门解决问题的分支（也可以叫修复BUG)，解决后合并

### github邀请合作开发
1. 第一种方式：创建一个项目仓库，然后在设置中邀请合作者加入，就ok了(适用于个人)
2. 第二种方式：创建一个组织，邀请成员，可以在组织中创建多项目，项目成员权限分配
基于tag的版本管理：
	**git tag -a v1 -m '版本1'**(本地版本)
	**git push 别名 --tags**(版本信息推送云端)
#### 多人合作开发实现（基于第二种方式）
1. 本地创建进入副分支（dev)：**git checkout -b dev**
2. 推送云端（云端地址别名：0x34Hz): **git push 0x34Hz dev**
3. 合作成员注册账户，邀请成员（填写成员，发送邀请）
4. 针对某项目设置成员权限
5. 成员将项目克隆到自己电脑：**git clone 项目地址**
6. 成员是基于dev分支：**git checkout dev**
7. 创建负责部分的分支：**git checkout -b gt**(命名可以按项目部分或责任人名)
8. 然后，在gt分支下开发项目，项目完成
9. review(代码检查)，在github上dev分支设置中添加规则
	与dev分支合并的规则：谁来检查
10. 通过pull request发送给leader
11. 合并请求（dev <- gt）将gt分支合并到dev分支，说明原因（如：xx项目完成）
12. leader将会进行项目review，没问题就会执行合并
13. leader在dev下创建release分支：**git checkout -b release**
14. 并将release推送到云端：**git push 0x34Hz release**
15. 测试小组从云端克隆下来，测试;没问题，重新执行10步
16. 请求合并到mater（主分支）
17. 先在dev分支下进行：**git merge release**
18. （可选）删除测试分支：**git branch -d release**
19. 再回到主分区：**git pull 0x34Hz master**
20. 利用tag进行控制：**git tag -a v2 -m '第二个版本'**
21. 将tag推送云端：**git push 0x34Hz --tags**

### 给开源项目贡献代码 [面试考点]
1. 先fork项目（这个我常干，只要好的都想要）：
	也就是将别人的项目拷贝到自己账户（仓库）下
2. 在自己的账户（仓库）下修改代码(优化)或修改bug
3. 完成后，给源代码作者提交修复bug或优化申请(new pull request)
4. 填写requset，发送OK！

### 其他内容
#### git的配置文件
1. 本地配置：.git/config（对某个项目有效）
2. 全局配置：~/.gitconfig（对某个用户下所有的项目有效）
3. 系统配置：/etc/.gitconfig(需要root权限)（对系统中所有项目有效）

#### 免密登录
1. url中体现：给地址其别名时，
	修改地址为：https://用户名:密码@github.com/0x34Hz/test.git
	或修改配置文件中的地址
2. SSH实现：电脑生成公钥和私钥（终端运行：ssh-keygen）[企业常用]
	默认将公钥和私钥存放在用户目录下~/.ssh/  id_rsa(私钥)
	id_rsa.pub(公钥)：将其设置到github设置中的ssh keys
	在git本地配置ssh地址：**git remote add 0x34Hz ssh地址**
3.git自动管理凭证（通过软件保存）

### git忽略的文件
创建.gitignore文件，写如不需要git管理的 文件名.后缀，
也可以写 目录/ 表示忽略目录文件 !文件名.后缀(排除)

### 任务管理相关
- issues(项目问题管理：提问/回复)
- wiki(项目介绍描述)


