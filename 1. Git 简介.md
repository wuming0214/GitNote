### Git 简介
	❄ 版本控制
	  数据备份：保存所有历史版本、对应的修改人、修改时间、日志信息。
	  协同修改：管理文件的内容，能够让多个开发人员并行地修改文件。
	  分支管理：方便不同的开发人员并行开发多个功能。
	
	❄ 版本控制系统
	  分类：
	    集中式版本控制系统：本地没有完整的版本库。举例：SVN
	    分布式版本控制系统：本地有完整的版本库。举例：Git
	  比较：
	    集中式版本控制工具存在单点故障，分布式版本控制工具可以避免单点故障
	
	❄ Git
	  简介：Git 是一个开源的分布式版本控制系统
	  官网：https://git-scm.com
	  作者：Linus
	  历史：
	    1. Linus 手动合并 Linux 代码
	    2. Linux 社区免费使用 BitKeeper 管理 Linux 源码
	    3. BitMover 公司不让 Linux 社区使用 BitKeeper 了
	    4. Linus 发明 Git
	  安装：前往官网下载安装即可

### Git 基本概念
	• working tree：工作区，即项目目录及其子目录（除了 .git 文件夹），树状结构，对应一个正在开发版本的项目版本。
	• index/staging area：暂存区，暂时存储一个项目版本
	• repository：版本库，存储有多个项目版本。其中一个版本很特殊，叫 current HEAD，即当前分支上的最新提交。
	• working directory：工作目录，可以使用 pwd 查看
	• commit：提交，每个提交对应一个新的项目版本，每一个提交记录对应一个版本记录。
	• branch：分支，提交对象的链表
	• branch head：分支头，提交对象的链表的表头，即分支上的最新提交
	• HEAD：同 branch head
	• current branch head：当前分支的头
	• current HEAD：同 current branch head
	• tracked：追踪状态，如果暂存区有 f1，则说 f1 处于 tracked 状态
